# Checkpointing (CRIU)

Parent: [../VISION.md](../VISION.md) · See: [AGENTS.md](./AGENTS.md) · [../product/TECHNICAL_SHAPE.md](../product/TECHNICAL_SHAPE.md) · [../product/SECURITY.md](../product/SECURITY.md)

**Name note:** often typed “ciru”; the project is **[CRIU](https://criu.org/)** — Checkpoint/Restore In Userspace (pronounced *kree-oo*). GPLv2 utility; freeze a process tree, write images to disk, restore later. Mostly userspace + kernel features (`ptrace`, namespaces, TCP repair, lazy pages, etc.). Ecosystem includes [go-criu](https://github.com/checkpoint-restore/go-criu), Podman/Docker/K8s hooks, experimental GPU paths (`cuda-checkpoint` / CRIUgpu).

## Why it fits xOS

Supervised agents are process trees with identity and ACLs. Kill/restart loses expensive context (tool loops mid-run, warm caches, open sockets). CRIU turns **stop** into **freeze and save**, and **start** into **restore where we left off**—when the tree is CRIU-friendly.

```text
goal in flight
  → supervisor freezes agent tree (CRIU dump)
  → images on disk (versioned, ACL-scoped)
  → later: restore same uid/paths → continue
```

This is OS-level state, not “chat history only.”

## What we can build (by layer)

| Build-out | Idea | Priority for product |
|-----------|------|----------------------|
| **Agent freeze / resume** | Supervisor `stop` = dump; `continue` = restore. Idle agents park to images instead of dying | Core supervisor feature |
| **Session snapshots** | Named checkpoints of a Develop/Research agent tree + workspace path; revert or branch a run | Strong for real work |
| **Incremental dumps** | Series of states; roll back a bad agent step without full re-prompt | High value once freeze works |
| **Warm start** | Checkpoint after agent runtime + tools are loaded; cold boot restores warm agent in seconds | Boot / mode switch speed |
| **Hung-agent capture** | Dump a stuck agent, restart clean path, debug the dump offline | Ops / security |
| **Duplicate agent** | Restore same image under a **new** agent user (fork-like); A/B a plan | Advanced multi-agent |
| **Migrate session** | Dump on machine A, restore on B (same-ish kernel/CPU features)—live “take my desk with me” | Later; needs hardware parity |
| **Mode switch park** | Leaving Develop dumps agent; returning restores | UX polish |
| **GPU agent work** | CUDA/ROCm checkpoint with CRIU + vendor tools | Later; experimental stack |
| **Desktop surface** | Full Wayland session C/R is hard; prefer agent trees and TTY/shell jobs first | Deferred |

Upstream usage scenarios that map cleanly: slow-boot speedup, app snapshots / incremental dumps, hung-app debugging, process duplication, HPC periodic save, (later) live migration. See [CRIU usage scenarios](https://criu.org/Usage_scenarios).

## Supervisor integration (product shape)

| Concern | Direction |
|---------|-----------|
| Who calls CRIU | **Supervisor only** (not the agent dumping itself by default) |
| What is dumped | Agent process tree under that agent’s uid; optional child tool processes it started |
| Where images live | Per-agent store under ACL paths; human can list/delete; not world-readable |
| Log | Every dump/restore is an action-log event (which agent, path, success/fail) |
| Stop | Soft stop → dump then freeze/exit; hard kill still available |
| Policy | Dump/restore of trees that hold secrets needs same confirm rules as high-risk acts when images leave the machine |
| Failure | If CRIU cannot dump (unsupported fd, device, GPU), fall back to kill + structured “resume from last capability log”—honest degrade |

## What CRIU does **not** magically solve

| Limit | Implication for us |
|-------|---------------------|
| Linux + kernel feature surface | Fits our Linux base; not Cosmopolitan-portable as the dump tool itself |
| Kernel / CPU feature parity on restore | Migration and long-lived images need versioned base images and `cpuinfo` checks |
| Not everything can be dumped | Some devices, exotic fds, some GPU paths fail—design agent trees to be dump-friendly |
| Wayland / full DE C/R | Hard; do not bet v1 on “suspend whole desktop with CRIU” |
| Security of image files | Checkpoint = memory + secrets snapshot; treat as sensitive as swap |
| License | CRIU is GPLv2; packaging next to Apache/MIT code needs the usual separation discipline |

## Relation to multicall / Cosmopolitan base

- **Ship `criu` on the installed system** (or a tightly versioned package)—it is a host tool, not an APE blob.  
- Multicall core stays small; CRIU is an explicit **layer** or optional package for agent runtime.  
- Cosmopolitan helpers are orthogonal (portable skills); checkpoint images are **host-bound**.  
- Prefer agent trees that use ordinary files, pipes, and network sockets CRIU already handles well.

## v1 vs later

| v1 (stretch / strong optional) | Later |
|--------------------------------|--------|
| CRIU present on image; supervisor can dump/restore **one** simple agent tree on real hardware | Incremental series UI; cross-machine migrate |
| Document dump-friendly agent layout (no mystery devices) | GPU agent checkpoint |
| Fail soft with log if dump fails | Mode-switch auto-park; desktop session C/R |

Full live migration and DE-wide suspend are **not** the v1 story. Agent freeze/resume and session snapshots **are** the product-shaped use of CRIU.

## Pointers

- https://criu.org/ · https://github.com/checkpoint-restore/criu  
- Releases: e.g. 4.2 (late 2025) and ongoing kernel chase  
- go-criu / P.Haul for programmatic control from a supervisor written in Go  
- NVIDIA: CUDA checkpoint + CRIU for GPU processes (experimental coordination)
