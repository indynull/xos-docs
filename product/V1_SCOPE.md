# v1 scope

Parent: [../VISION.md](../VISION.md) · See: [SUCCESS_CRITERIA.md](./SUCCESS_CRITERIA.md)

## Goal

Show the idea works on a few **real** tasks. Not a full workplace product. Not toy-only demos. Not “boots in QEMU” as the story.

**Deliverable:** bootable image (prefer real hardware for demos; QEMU OK for CI) where the normal path is goal → **supervised** agent → capability → work mode, on a Wayland modal shell.

## In scope

| Area | Need |
|------|------|
| Shell | CLI + plain-language goal entry; quiet UI; always show mode; at least Develop and Research |
| Desktop | Wayland; modal focus (mode + agent); multi-window allowed, not default |
| Context | Shared project/workspace across views |
| Agent supervisor | Lifecycle control; start/stop; action log; at least two agent identities |
| Agents | Goal entry; read and act under ACLs; status, stop, log |
| Isolation | Per-agent Unix user + ACLs; no default “run as me” |
| Capabilities | Small real set (below); one create-and-reuse path |
| Develop | Real edit + terminal + agent session |
| Security | Written rules match behavior ([SECURITY.md](./SECURITY.md)) |
| Base | Multicall-leaning core (BusyBox-class or equivalent), not a mystery distro rootfs; optimizations and optional Cosmopolitan/APE helpers where they help ([TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md)) |

### Capability shapes (pick concrete targets later; keep the shape)

| Shape | Meaning |
|-------|---------|
| Code + project | Real repo in Develop |
| Investigation | Explain a failure using logs/tests (fixtures OK) + log of steps |
| Write-up | Multi-source summary with citations |
| Bind a system | Create capability for a real-ish target → save → second goal uses it |
| Optional tiny | One small capability only to show the same pattern at small scale—not the star of the demo |

## Out of v1

Full industry workflows · multi-arch program · app store · theming as a feature · every website · new kernel from scratch · mobile/edge as main *product* · QEMU as product proof · single privileged agent with human rights · window-manager-first UX · Cosmopolitan as the whole desktop · busybox-only image with no goal/agent path.

## Time

Roughly 6–10 weeks to a yes/no demo when a team exists. Prefer a few solid paths over many toys.
