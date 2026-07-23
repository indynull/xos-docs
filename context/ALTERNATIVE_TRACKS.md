# Other tracks

Parent: [../VISION.md](../VISION.md)

Related ideas that are **not automatically this project**. Keep them separate so this stays clear.

| Track | One line | Relation |
|-------|----------|----------|
| Multicall core + toolchain opts | BusyBox/toybox/custom multicall, real perf work | **In** base strategy for xOS |
| Cosmopolitan libc / APE | Libc alternative + portable binaries | **In** as runtime ABI path ([../product/LINKERS_LOADERS.md](../product/LINKERS_LOADERS.md)); not whole DE day one |
| CRIU checkpoint/restore | Freeze/restore process trees (often typed “ciru”) | **In** kernel-deep with btrfs; not DE-wide migrate as v1 |
| Linkers / loaders / libc identity | Dump-friendly runtime ABI for agents | **In** ([../product/LINKERS_LOADERS.md](../product/LINKERS_LOADERS.md)) |
| btrfs snapshots | Agent homes, workspaces, backups under agent traffic | **In** as default durability story |
| systemd unit isolation | Per-agent services, sandbox, restarts | **In** as lifecycle/isolation backbone (or compatible) |
| Archy / Enso | Humane command nucleus, persistence, lightweight invoke | **Inspiration** ([../concepts/UX_LINEAGE.md](../concepts/UX_LINEAGE.md)); not a clone |
| Custom kernel / install profiles | Real-hardware opts ([linux-rg](https://github.com/HaoZeke/linux-rg)-style, [hzArchiso](https://github.com/HaoZeke/hzArchiso)-style discipline); CRIU-capable | **In** ([../product/HARDWARE_PROFILES.md](../product/HARDWARE_PROFILES.md)); not QEMU-as-product |
| Multi-site scientific module stacks (e.g. EESSI) | Shared HPC install trees | **Out** of product shape—wrong layer for agent restore |
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
