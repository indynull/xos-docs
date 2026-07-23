# Non-goals

Parent: [../VISION.md](../VISION.md)

Things we are **not** trying to be. Saying no keeps the project clear.

| Not this | Why |
|----------|-----|
| Theme / wallpaper / ricing *product* | No desktop-ricing subsystem; light/dark prefer implicit or simple system behavior, not a theme app |
| Agent-only UI with no instant tools | Model latency is real; always keep normal/fast paths (terminal, launcher, editor) |
| App store as the main product | Different product; maybe later on top |
| Chat box on a normal desktop as the end state | Leaves the old workflow intact |
| Toy demos as the identity | Real work is the bar |
| Huge default install | Multicall core + layers; we do not ship “everything just in case” |
| Consumer distro minus packages as the architecture | Prefer building up from a small core (BusyBox-class / similar), not deleting our way to thin |
| Full multi-arch distro as v1 | Too large for the first proof |
| New kernel from scratch (v1) | Use Linux; own the desktop/agent layer; optimize what we ship |
| Cosmopolitan as system libc | Interesting for single-binary demos; not usable as system libc here |
| Full libc replacement in v1 (Rust/FFI shims, etc.) | Future-scale; apps depend on libc details/bugs; browser port alone is huge |
| Multi-site package/module stacks as the restore fix | Wrong layer; formats/linkers/loaders + kernel-deep CRIU |
| “Build once run anywhere” as product identity | Avoid Java-style slogans; ship a real OS runtime |
| Browser as the main product | App layer; embed when needed; not the nucleus |
| “Just a Sway/DE alternative” as the product | Compositor is a piece; product is goals + agents + OS-adjacent base |
| Apps as the only way to work | Goals and capabilities come first |
| Endless UI customization | Defaults should be strong; modal focus over window soup |
| Mobile or edge *product* as v1 | Different product; multicall base technique may still be shared |
| Big committee process | Small group; ship |
| QEMU-only forever as the product story | QEMU-first *bring-up* is fine; still need real HW for drivers/local LLM/demos |
| Hostile-web agent farm as v1 (puppeteer + residential IP) | Expensive, brittle, legal/ethical minefield; not the harness wedge |
| Shelling out to a chat CLI as the OS agent | Need OS-level harness, routing, and budgets |
| Replacing NetworkManager/iwd in v1 | Start with them; replace later if earned |
| Single agent with the user’s full rights | Supervisor + per-agent users/ACLs |
| Window-manager-first UX | Many windows allowed; not the default story |

“Not for v1” does not mean “never.” To add something later, put scope and success criteria under [../product/](../product/).

Other product ideas: [../context/ALTERNATIVE_TRACKS.md](../context/ALTERNATIVE_TRACKS.md).
