# v1 scope

Parent: [../VISION.md](../VISION.md) · See: [SUCCESS_CRITERIA.md](./SUCCESS_CRITERIA.md)

## Goal

Show the **OS agent harness** works on a few **real** tasks. Not a full workplace product. Not toy-only demos. Not “every subsystem rewritten.”

**Deliverable:** bootable image (**QEMU-first** OK for bring-up; real HW for CRIU/local-LLM claims) where the normal path is goal → **supervised** agent → capability → work mode, on a Wayland modal shell.

## In scope

| Area | Need |
|------|------|
| Shell | CLI + plain-language goal entry; **instant** normal tools without model wait; quiet UI; always show mode; at least Develop and Research |
| Desktop | Wayland; modal focus (mode + agent); multi-window allowed, not default |
| Context | Shared project/workspace across views |
| Agent supervisor | Units + policy: start/stop; action log; at least two agent identities |
| CRIU | **First-class + kernel-deep:** supervisor dump/restore of canonical agent tree; test on QEMU and/or primary real-hardware profile ([../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md)) |
| Model path | Documented dual-stack direction; v1 may be remote-first + local stub ([MODEL_STACK.md](./MODEL_STACK.md)) |
| Wedge | Harness uniqueness clear—not only chat CLI ([WEDGE.md](./WEDGE.md)) |
| Agents | Goal entry; read and act under ACLs; status, stop, log; durable data paths; dump-friendly **link/load layout** required for the happy path |
| Isolation | Per-agent Unix user + unit sandbox + ACLs; no default “run as me” |
| Durability | btrfs (or honest fallback) for agent/workspace paths; snapshots as default backup story when available |
| Capabilities | Small real set (below); one create-and-reuse path (teach path OK); sandboxed; CRIU as platform capability |
| Develop | Real edit + terminal + agent session |
| Security | Written rules match behavior ([SECURITY.md](./SECURITY.md)) |
| Base | Multicall-leaning core; at least one **hardware profile** ([HARDWARE_PROFILES.md](./HARDWARE_PROFILES.md)); not QEMU-as-product |
| Runtime ABI | Stay near **existing system libc**; documented formats + linker/loader + debug policy ([LINKERS_LOADERS.md](./LINKERS_LOADERS.md)); no novel libc on critical path |
| Product shape | Goal-first surface + agents—not only a compositor/DE fork ([OS_VS_DE.md](./OS_VS_DE.md)) |

### Capability shapes (pick concrete targets later; keep the shape)

| Shape | Meaning |
|-------|---------|
| Code + project | Real repo in Develop |
| Investigation | Explain a failure using logs/tests (fixtures OK) + log of steps |
| Write-up | Multi-source summary with citations |
| Bind a system | Create capability for a real-ish target → save → second goal uses it |
| Optional tiny | One small capability only to show the same pattern at small scale—not the star of the demo |

## Out of v1

Full industry workflows · multi-arch program · app store · theming as a feature · every website · new kernel from scratch · mobile/edge as main *product* · QEMU as product proof · single privileged agent with human rights · window-manager-first UX · Cosmopolitan as system libc · busybox-only image with no goal/agent path · full Wayland DE checkpoint · cross-machine live migration as the v1 demo · GPU CRIU as required · rewrite-libc-in-Rust as v1 gate.

## Time

Roughly 6–10 weeks to a yes/no demo when a team exists. Prefer a few solid paths over many toys.
