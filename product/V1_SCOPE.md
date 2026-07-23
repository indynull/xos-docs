# v1 scope

Parent: [../VISION.md](../VISION.md) · See: [SUCCESS_CRITERIA.md](./SUCCESS_CRITERIA.md) · [WEDGE.md](./WEDGE.md)

## Goal

Show the **OS agent harness** works on a few **real** tasks. Not a full workplace product. Not toy-only demos. Not “every subsystem rewritten.”

**Deliverable:** bootable image (**QEMU OK**) where the normal path is goal → **supervised** agent → capability → work mode, with dual path (instant tools + goals).

## In scope (product)

| Area | Need |
|------|------|
| Shell | CLI + plain-language goal entry; **instant** normal tools without model wait; always show mode; at least Develop and Research |
| Context | Shared project/workspace across views |
| Agent supervisor | Lifecycle + policy; action log; at least two agent identities |
| Agents | Goal entry; read and act under limits; status, stop, log; durable data paths |
| Isolation | Per-agent identity (user + unit-class isolation + ACLs); no default “run as me” |
| Durability | Agent/workspace data on disk; **btrfs** preferred when available |
| Capabilities | Small real set; one create-or-**teach**-and-reuse path; sandboxed |
| Develop | Real edit + terminal + agent session |
| Security | Written rules match behavior ([SECURITY.md](./SECURITY.md)) |
| Base | Multicall-leaning core—not consumer distro minus packages |
| Wedge | Harness uniqueness clear—not only chat CLI ([WEDGE.md](./WEDGE.md)) |
| Product shape | Goal-first surface + agents—not only a compositor/DE fork ([OS_VS_DE.md](./OS_VS_DE.md)) |

### Capability shapes (pick concrete targets later; keep the shape)

| Shape | Meaning |
|-------|---------|
| Code + project | Real repo in Develop |
| Investigation | Explain a failure using logs/tests (fixtures OK) + log of steps |
| Write-up | Multi-source summary with citations |
| Bind a system | Create or teach capability → save → second goal uses it |
| Optional tiny | Small pattern demo—not the star |

## Direction for v1 (not pass/fail gates)

| Area | Intent |
|------|--------|
| Checkpoint/restore | Prefer freeze/resume of agent process state when ready; **CRIU is candidate**—kernel-deep + tested when we ship it, not a v1 checklist item yet |
| Display | Quiet, mode-centered UI; **display server not locked** (Wayland is a candidate) |
| Models | Dual remote/local is direction; v1 may be remote-first ([MODEL_STACK.md](./MODEL_STACK.md)) |
| Hardware profiles | Useful later; not required to tell the product story |
| Runtime ABI | Stay near real system libc by default; deep linker/loader notes are exploration |

## Out of v1

Full industry workflows · multi-arch program · app store · theming product · every website · hostile-web puppeteer farm · new kernel from scratch · mobile/edge as main product · single privileged agent with human rights · window-manager-first UX · boiling every layer · local LLM required on virtio-gpu · novel system libc · replace NM/iwd as first priority.

## Time

Roughly 6–10 weeks to a yes/no demo when a team exists. Prefer a few solid paths over many toys.
