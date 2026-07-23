# Checkpointing (CRIU + btrfs)

Parent: [../VISION.md](../VISION.md) · See: [AGENTS.md](./AGENTS.md) · [../product/TECHNICAL_SHAPE.md](../product/TECHNICAL_SHAPE.md) · [../product/SECURITY.md](../product/SECURITY.md)

**Name note:** often typed “ciru”; the project is **[CRIU](https://criu.org/)** — Checkpoint/Restore In Userspace (pronounced *kree-oo*). GPLv2 utility; freeze a process tree, write images to disk, restore later. Mostly userspace + kernel features (`ptrace`, namespaces, TCP repair, lazy pages, etc.). Ecosystem includes [go-criu](https://github.com/checkpoint-restore/go-criu), Podman/Docker/K8s hooks, experimental GPU paths (`cuda-checkpoint` / CRIUgpu).

## First-class OS capability (not a bolt-on)

CRIU is a **baked-in platform capability**: shipped on the image, wired into the supervisor, documented as a skill agents and humans can rely on—not “maybe install criu later.”

Agent restore in the wild is often **hit-or-miss** (unsupported fds, library drift, GPU, hand-wavy dumps). xOS treats that as a product defect to close, not an expected UX:

| Guarantee | Direction |
|-----------|-----------|
| **Default agent layout is dump-friendly** | Ordinary files, pipes, sockets; no mystery devices on the happy path |
| **Supervisor owns dump/restore** | One code path; tested; logged |
| **Runtime ABI under the agent** | Same linker/loader/libc graph at dump and restore ([../product/LINKERS_LOADERS.md](../product/LINKERS_LOADERS.md)); Cosmopolitan-linked trees where that helps |
| **Kernel we ship** | CRIU is integrated with the **project kernel/profile**—not bolted onto a random distro kernel ([../product/HARDWARE_PROFILES.md](../product/HARDWARE_PROFILES.md)) |
| **Honest degrade** | If dump fails: structured error + btrfs durability + resume-from-log—not silent corruption |
| **CI + metal** | Restore tests on real hardware for the primary profile; QEMU optional extra |

“First-class” means the **happy path is reliable**, not that every exotic tree must dump.

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

- Ship CRIU integrated with the **kernel we build**; btrfs-progs on the image.  
- Multicall core stays small; CRIU + btrfs are explicit **base layers** for agent runtime durability.  
- Cosmopolitan as **libc alternative** can make agent trees more restore-friendly; APE for portable skills. Checkpoint images remain host/kernel-bound.

## Kernel-deep CRIU (big idea)

CRIU is not “install a package and hope.” For xOS it is a **base feature of the system we ship**:

- Kernel config and features required for C/R are part of each **hardware profile**.  
- Breakages between our kernel and CRIU are **base bugs**, not user error.  
- Userspace policy (dump-friendly link/load, Cosmo vs glibc) is designed **with** that kernel, not against a random host.  
- Optional later: tighter kernel support for agent-oriented freeze if something earns its keep (distinct from vague “agentic -rt” tourism).

## Hardware and performance

CRIU and btrfs are not a substitute for **real-hardware kernel and driver profiles**. Product proof stays on metal; QEMU is CI smoke. See [../product/HARDWARE_PROFILES.md](../product/HARDWARE_PROFILES.md). Restores also need a stable **linker/loader/libc** story ([../product/LINKERS_LOADERS.md](../product/LINKERS_LOADERS.md)).

## v1 vs later

| v1 (first-class bar) | Later |
|----------------------|--------|
| CRIU **on the image** and **validated against our kernel/profile**; supervisor dump/restore of the **canonical agent tree** | Cross-machine migrate |
| Automated test: dump → restore → agent continues a known task (real hardware, primary profile) | Incremental series UI; GPU |
| Documented linker/loader/libc policy for that tree (glibc and/or Cosmo) | Broader Cosmo-linked agent cores |
| btrfs default for agent homes / workspaces when the image allows | Full DE C/R |
| systemd (or compatible) unit per agent identity | Deeper custom kernel C/R hooks if measured |
| Fail-soft only after a real dump attempt; never pretend success | Tight CRIU+snap orchestration UI |

## Pointers

- https://criu.org/ · https://github.com/checkpoint-restore/criu  
- go-criu / P.Haul for programmatic control from a Go supervisor  
- btrfs subvolumes, snapshots, send/receive  
- systemd unit sandboxing / `User=` / cgroup isolation  
- NVIDIA: CUDA checkpoint + CRIU (experimental)
