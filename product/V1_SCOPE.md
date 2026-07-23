# v1 scope

Parent: [../VISION.md](../VISION.md) · See: [SUCCESS_CRITERIA.md](./SUCCESS_CRITERIA.md)

## Goal

Show the idea works on a few **real** tasks. Not a full workplace product. Not toy-only demos.

**Deliverable:** bootable image (QEMU first) where the normal path is goal → agent → capability → work mode.

## In scope

| Area | Need |
|------|------|
| Shell | Quiet UI; always show mode; at least Develop and Research |
| Context | Shared project/workspace across views |
| Agent | Goal entry; read and act; status, stop, log |
| Capabilities | Small real set (below); one create-and-reuse path |
| Develop | Real edit + terminal + agent session |
| Security | Written rules; limited tools ([SECURITY.md](./SECURITY.md)) |
| Base | Thin Linux; remove clutter ([TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md)) |

### Capability shapes (pick concrete targets later; keep the shape)

| Shape | Meaning |
|-------|---------|
| Code + project | Real repo in Develop |
| Investigation | Explain a failure using logs/tests (fixtures OK) + log of steps |
| Write-up | Multi-source summary with citations |
| Bind a system | Create capability for a real-ish target → save → second goal uses it |
| Optional tiny | One small capability only to show the same pattern at small scale—not the star of the demo |

## Out of v1

Full industry workflows · multi-arch program · app store · theming as a feature · every website · new kernel · mobile/edge as main target.

## Time

Roughly 6–10 weeks to a yes/no demo when a team exists. Prefer a few solid paths over many toys.
