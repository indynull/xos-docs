# Other tracks

Parent: [../VISION.md](../VISION.md)

Related ideas that are **not automatically this project**. Keep them separate so this stays clear.

| Track | One line | Relation |
|-------|----------|----------|
| Multicall core + toolchain opts | BusyBox/toybox/custom multicall, real perf work | **In** base strategy for xOS |
| CRIU checkpoint/restore | Freeze/restore process trees ([criu.org](https://criu.org/)) | **In** kernel-deep with btrfs; first-class |
| Formats / linkers / loaders / debug | Dump-friendly runtime identity | **In** ([../product/LINKERS_LOADERS.md](../product/LINKERS_LOADERS.md)) |
| Cosmopolitan / APE | Portable single-binary demos | Optional side path; **not** system libc |
| btrfs snapshots | Agent homes, workspaces, backups under agent traffic | **In** as default durability story |
| systemd unit isolation | Per-agent services, sandbox, restarts | **In** as lifecycle/isolation backbone (or compatible) |
| Archy / Enso | Humane command nucleus, persistence, lightweight invoke | **Inspiration** ([../concepts/UX_LINEAGE.md](../concepts/UX_LINEAGE.md)); not a clone |
| Custom kernel / install profiles | Real-hardware opts ([linux-rg](https://github.com/HaoZeke/linux-rg)-style, [hzArchiso](https://github.com/HaoZeke/hzArchiso)-style discipline); CRIU-capable | **In** ([../product/HARDWARE_PROFILES.md](../product/HARDWARE_PROFILES.md)); not QEMU-as-product |
| Rust-as-system-libc + shims | Research alternate libc | **Future version**; browser-on-new-libc is proposal-sized |
| Pluggable malloc under glibc | e.g. snmalloc speedups | **In** carefully; gate on app matrix (Chromium-class breakage) |
| Sway / DE clone as the project | Tiling WM product | **Out**—compositor is a piece ([../product/OS_VS_DE.md](../product/OS_VS_DE.md)) |
| Coding-agent apps only (opencode/hermes-class) | Great harness pieces | **Not enough**—we need OS integration ([../product/WEDGE.md](../product/WEDGE.md)) |
| Dual LLM (remote + local) | Grok + llama.cpp-class | **In** as model stack ([../product/MODEL_STACK.md](../product/MODEL_STACK.md)) |
| Custom net stack (beyond iwd/NM) | Better wifi/bt story | Later; start with NM/iwd |
| Hostile-web automation farm | Puppeteer + residential IP | **Out** of v1 wedge |
| **xOS (this repo)** | Goal-first desktop + capabilities + supervised agents | Main idea here |
| App-in-a-box “OS” | Avoid drivers; userspace environment | Can sit beside |
| App store / platform | Plugins and stable APIs | Later, not v1 core |
| Agent plumbing | Loops, sandboxes, CI | Shared engineering; supervisor/ACLs are product shape here |
| Security research | Abuse cases, agent threat models | Feeds security notes |
| Mobile | Different constraints | Separate |
| Edge / appliance *product* | Tiny system sold as the product | Separate product; may share multicall core / kernel later |
| Browser-first AI product | Browser is the product | Different idea |
| AI desktop chrome demos | NL shell + generated wallpaper focus | Different idea |
| QEMU demo distro | Boot image as the goal | CI only; not this product |

Shared code later is fine. Do not merge by renaming everything “the OS.”
