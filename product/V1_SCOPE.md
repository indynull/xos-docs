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
| Agent supervisor | Units + policy: start/stop; action log; at least two agent identities |
| CRIU | **First-class:** on image; supervisor dump/restore of canonical agent tree; automated restore test on primary real-hardware profile ([../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md)) |
| Agents | Goal entry; read and act under ACLs; status, stop, log; durable data paths; dump-friendly layout **required** for the happy path |
| Isolation | Per-agent Unix user + unit sandbox + ACLs; no default “run as me” |
| Durability | btrfs (or honest fallback) for agent/workspace paths; snapshots as default backup story when available |
| Capabilities | Small real set (below); one create-and-reuse path; CRIU exposed as a platform capability |
| Develop | Real edit + terminal + agent session against **pinned stack** |
| Security | Written rules match behavior ([SECURITY.md](./SECURITY.md)) |
| Base | Multicall-leaning core; at least one **hardware profile** documented ([HARDWARE_PROFILES.md](./HARDWARE_PROFILES.md)); not QEMU-as-product |
| Build / deploy | Stack revision pinned in image + CI (EESSI-class guarantees; [BUILD_DEPLOY.md](./BUILD_DEPLOY.md)) |

### Capability shapes (pick concrete targets later; keep the shape)

| Shape | Meaning |
|-------|---------|
| Code + project | Real repo in Develop |
| Investigation | Explain a failure using logs/tests (fixtures OK) + log of steps |
| Write-up | Multi-source summary with citations |
| Bind a system | Create capability for a real-ish target → save → second goal uses it |
| Optional tiny | One small capability only to show the same pattern at small scale—not the star of the demo |

## Out of v1

Full industry workflows · multi-arch program · app store · theming as a feature · every website · new kernel from scratch · mobile/edge as main *product* · QEMU as product proof · single privileged agent with human rights · window-manager-first UX · Cosmopolitan as the whole desktop · busybox-only image with no goal/agent path · full Wayland DE checkpoint · cross-machine live migration as the v1 demo · GPU CRIU as required.

## Time

Roughly 6–10 weeks to a yes/no demo when a team exists. Prefer a few solid paths over many toys.
