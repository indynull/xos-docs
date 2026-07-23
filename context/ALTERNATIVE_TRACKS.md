# Other tracks

Parent: [../VISION.md](../VISION.md)

Related ideas that are **not automatically this project**. Keep them separate so this stays clear.

| Track | One line | Relation |
|-------|----------|----------|
| Thin Linux / toolchains / optimizations | Small base, solid core tools, real perf work | **In** base system for xOS; not a standalone product story |
| **xOS (this repo)** | Goal-first desktop + capabilities + supervised agents | Main idea here |
| App-in-a-box “OS” | Avoid drivers; userspace environment | Can sit beside |
| App store / platform | Plugins and stable APIs | Later, not v1 core |
| Agent plumbing | Loops, sandboxes, CI | Shared engineering; supervisor/ACLs are product shape here |
| Security research | Abuse cases, agent threat models | Feeds security notes |
| Mobile | Different constraints | Separate |
| Edge / appliance | Tiny system / single binary | Separate (may share kernel/toolchain later) |
| Browser-first AI product | Browser is the product | Different idea |
| AI desktop chrome demos | NL shell + generated wallpaper focus | Different idea |
| QEMU demo distro | Boot image as the goal | CI only; not this product |

Shared code later is fine. Do not merge by renaming everything “the OS.”
