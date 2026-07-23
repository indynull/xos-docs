# Checkpointing (CRIU + btrfs)

Parent: [../VISION.md](../VISION.md) · See: [AGENTS.md](./AGENTS.md) · [../product/TECHNICAL_SHAPE.md](../product/TECHNICAL_SHAPE.md) · [../product/SECURITY.md](../product/SECURITY.md)

**Name note:** often typed “ciru”; the project is **[CRIU](https://criu.org/)** — Checkpoint/Restore In Userspace (pronounced *kree-oo*). GPLv2 utility; freeze a process tree, write images to disk, restore later. Mostly userspace + kernel features (`ptrace`, namespaces, TCP repair, lazy pages, etc.). Ecosystem includes [go-criu](https://github.com/checkpoint-restore/go-criu), Podman/Docker/K8s hooks, experimental GPU paths (`cuda-checkpoint` / CRIUgpu).

## Does CRIU fit with btrfs + systemd + ACLs?

**Yes — at a different layer.** They compose; they do not replace each other.

| Layer | Tool | What it freezes / restores |
|-------|------|----------------------------|
| **Process / RAM** | CRIU | Running agent tree: memory, open fds, sockets, CPU state |
| **Filesystem** | **btrfs** subvolume snapshots | Agent homes, workspaces, capability store, CRIU image dirs, OS layers |
| **Lifecycle / isolation** | **systemd** (or compatible) units | Per-agent units: start, stop, cgroup, sandbox flags, restart policy |
| **Identity** | Unix user + **ACLs** | Who may read which paths, sockets, and secrets |

```text
systemd unit (agent-N.service)
  → process tree under agent-N uid
  → CRIU dump  →  images on btrfs subvol  →  snapshot that subvol
  → later: restore snapshot (files) then CRIU restore (process)  — or either alone
```

With high agentic traffic, **btrfs snapshots become the normal durability story** (workspace + memories + capability blobs). **CRIU** is the sharper tool when you care about *live* process state (warm agent, mid-tool-loop freeze, hung dump). Use both.

### Pairing rules

| Situation | Prefer |
|-----------|--------|
| Save project files, agent memory dirs, capability store | btrfs snapshot (fast, cheap, seamless backups) |
| Park a running agent without losing RAM-side context | CRIU dump; store images on a snapshotted subvol |
| “Just in case” rollback of a whole agent home | btrfs |
| Seamless backup / send to another disk | btrfs send/receive |
| Soft stop in the supervisor | CRIU first; always keep durable state on btrfs |
| CRIU dump fails | Fall back to kill + resume from log; durable data still on btrfs |

**btrfs is already aligned with core prototypes** under discussion; treat it as the default FS story for agent-heavy installs, not an optional extra.

## Why process checkpointing still matters

Supervised agents are process trees with identity and ACLs. Kill/restart loses expensive context (tool loops mid-run, warm caches, open sockets). CRIU turns **stop** into **freeze and save**, and **start** into **restore where we left off**—when the tree is CRIU-friendly.

```text
goal in flight
  → supervisor freezes agent tree (CRIU dump)
  → images on btrfs (ACL-scoped subvol; optional snapshot)
  → later: restore same uid/paths → continue
```

This is OS-level state, not “chat history only.”

## What we can build (by layer)

| Build-out | Idea | Priority for product |
|-----------|------|----------------------|
| **Agent freeze / resume** | Supervisor `stop` = CRIU dump; `continue` = restore. Idle agents park instead of dying | Core supervisor feature |
| **btrfs session/workspace snaps** | Snapshot agent home + project subvols on mode switch or timer | Default durability |
| **Combined save point** | Named “session”: btrfs snap of data + CRIU image of process (when dump works) | Strong for real work |
| **Incremental CRIU dumps** | Series of process states; roll back a bad agent step | High value once freeze works |
| **Warm start** | Checkpoint after runtime loaded; cold boot restores warm agent | Boot / mode switch speed |
| **Hung-agent capture** | Dump stuck agent; restart clean path; debug offline | Ops / security |
| **Duplicate agent** | New unit + uid; restore CRIU image and/or clone btrfs subvol | Advanced multi-agent |
| **Migrate session** | btrfs send data; CRIU only if kernel/CPU parity | Later |
| **GPU agent work** | CUDA/ROCm + CRIU | Later; experimental |
| **Desktop surface** | Full Wayland C/R hard; agent trees + TTY first | Deferred |

Upstream CRIU scenarios that map cleanly: slow-boot speedup, app snapshots, hung-app debugging, process duplication, HPC periodic save, (later) live migration. See [CRIU usage scenarios](https://criu.org/Usage_scenarios).

## Supervisor + systemd integration

| Concern | Direction |
|---------|-----------|
| Who calls CRIU | **Supervisor / unit hooks only** (not the agent dumping itself by default) |
| Unit model | One **systemd unit** (or equivalent) per agent: `User=`, sandbox, cgroup, restart |
| What is dumped | Agent process tree under that unit’s uid; optional children it started |
| Where images live | Per-agent btrfs subvol under ACL paths; snapshot-friendly; not world-readable |
| Memories / data | Durable agent state lives on disk (subvol), not only in process heap; CRIU is additive |
| Log | Every dump/restore/snapshot is an action-log event |
| Stop | Soft: CRIU dump → optional unit stop; hard: kill unit still available |
| Isolation | systemd features (PrivateTmp, ProtectSystem, capability bounding, etc.) + ACLs + CRIU/btrfs |
| Failure | If CRIU cannot dump → kill unit + resume from capability log; btrfs data remains |

## What CRIU does **not** magically solve

| Limit | Implication for us |
|-------|---------------------|
| Linux + kernel feature surface | Fits our Linux base; not Cosmopolitan-portable as the dump tool itself |
| Kernel / CPU feature parity on restore | Migration needs versioned base + `cpuinfo` checks; btrfs data migrates more easily than CRIU images |
| Not everything can be dumped | Design dump-friendly agent trees; rely on btrfs for durable truth |
| Wayland / full DE C/R | Hard; do not bet v1 on whole-desktop CRIU |
| Security of image files | CRIU image = memory/secrets; same care as swap; btrfs snaps of secrets dirs need same rules |
| License | CRIU GPLv2; packaging separation discipline |

## Relation to multicall / Cosmopolitan base

- Ship `criu` and btrfs-progs on the installed system (host tools).  
- Multicall core stays small; CRIU + btrfs are explicit **base layers** for agent runtime durability.  
- Cosmopolitan helpers are orthogonal (portable skills); checkpoint images and subvols are **host-bound**.

## Hardware and performance

CRIU and btrfs are not a substitute for **real-hardware kernel and driver profiles**. Product proof stays on metal; QEMU is CI smoke. Kernel config / optional agent-oriented patches (if any ever earn their keep) are separate from snapshot strategy—see [../product/TECHNICAL_SHAPE.md](../product/TECHNICAL_SHAPE.md).

## v1 vs later

| v1 | Later |
|----|--------|
| btrfs as default story for agent homes / workspaces where the image allows | Cross-machine migrate |
| CRIU present; supervisor can dump/restore **one** simple agent tree (stretch) or fail-soft | Incremental CRIU series UI; GPU |
| systemd (or compatible) unit per agent identity | Full DE C/R |
| Combined “save session” may start as btrfs-only if CRIU not ready | Tight CRIU+snap orchestration |

## Pointers

- https://criu.org/ · https://github.com/checkpoint-restore/criu  
- go-criu / P.Haul for programmatic control from a Go supervisor  
- btrfs subvolumes, snapshots, send/receive  
- systemd unit sandboxing / `User=` / cgroup isolation  
- NVIDIA: CUDA checkpoint + CRIU (experimental)
