# Other tracks

Parent: [../VISION.md](../VISION.md)

Related ideas that are **not automatically this project**. Keep them separate so this stays clear.

| Track | One line | Relation |
|-------|----------|----------|
| Multicall core + toolchain opts | BusyBox/toybox/custom multicall, real perf work | **In** base strategy for xOS |
| Cosmopolitan / APE tools | Actually portable helpers and capability blobs | **In** as tool/bootstrap layer; not the desktop product |
| CRIU checkpoint/restore | Freeze/restore process trees (often typed “ciru”) | **In** with btrfs; process layer; not DE-wide migrate as v1 |
| btrfs snapshots | Agent homes, workspaces, backups under agent traffic | **In** as default durability story |
| systemd unit isolation | Per-agent services, sandbox, restarts | **In** as lifecycle/isolation backbone (or compatible) |
| Archy / Enso | Humane command nucleus, persistence, lightweight invoke | **Inspiration** ([../concepts/UX_LINEAGE.md](../concepts/UX_LINEAGE.md)); not a clone |
| Custom kernel / install profiles | Real-hardware opts (e.g. purpose-built kernels/images) | **In** base track; not QEMU-as-product |
| **xOS (this repo)** | Goal-first desktop + capabilities + supervised agents | Main idea here |
| App-in-a-box “OS” | Avoid drivers; userspace environment | Can sit beside |
| App store / platform | Plugins and stable APIs | Later, not v1 core |
| Agent plumbing | Loops, sandboxes, CI | Shared engineering; supervisor/ACLs are product shape here |
| Security research | Abuse cases, agent threat models | Feeds security notes |
| Mobile | Different constraints | Separate |
| Edge / appliance *product* | Tiny system sold as the product | Separate product; may **share** multicall core, kernel, Cosmopolitan helpers |
| Browser-first AI product | Browser is the product | Different idea |
| AI desktop chrome demos | NL shell + generated wallpaper focus | Different idea |
| QEMU demo distro | Boot image as the goal | CI only; not this product |

Shared code later is fine. Do not merge by renaming everything “the OS.”
