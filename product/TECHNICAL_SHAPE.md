# Technical shape

Parent: [../VISION.md](../VISION.md)

Guesses only. Swap parts if the idea still holds. The spine below is intentional: **Wayland + modal shell**, **agent supervisor**, **per-agent Unix identity**, **radical small base**. Not “stock desktop + chat,” not “Ubuntu minus packages,” and not “QEMU demo distro.”

## Layers

| Layer | Direction |
|-------|-----------|
| Kernel | Linux (v1). Own config and knobs that earn their keep—not a from-scratch kernel |
| Core userspace | Prefer a **small multicall core** (BusyBox / toybox / custom multicall) over a full GNU userspace by default |
| Portable tools | Cosmopolitan Libc / APE-style **actually portable** binaries where they help (agent helpers, bootstrap tools, cross-host skills)—not required for every process |
| Limits | Process isolation; network/file limits; **per-agent Unix user + ACLs** |
| Agent supervisor | Supervisord-style: lifecycle, restart policy, inter-agent channels, policy hooks (can itself be a small dedicated binary) |
| Capability store | Versioned tools; local first |
| Agents | One or more workers under the supervisor; each identity-scoped; reuse existing stacks if they fit |
| Shell | CLI + plain-language goal entry; modes; action log; editor/terminal surfaces |
| Desktop | Wayland compositor; **modal** focus (mode + agent), multi-window allowed not default |
| Web engine | Embed when needed—not the main UI |
| Packaging | Reproducible images; fast rebuilds; prefer real hardware for product proof |

## Think larger about the base

The default mental model is **not** “fork a consumer distro and install an agent.” Prefer:

```text
Linux kernel
  → small multicall core (BusyBox-class applets: mount, sh, networking basics, …)
  → supervisor + agent runtime (isolated users/ACLs)
  → shell (CLI + plain language) + Wayland modal surface
  → capabilities / optional fuller tools pulled in as needed
```

That is closer to **Talos / appliance multicall** thinking (kernel + one coherent userspace story) than to “ship half of Debian.” Edge *product* is still out of v1 ([../principles/NON_GOALS.md](../principles/NON_GOALS.md)); **multicall as the base strategy** is in scope.

### Multicall core (BusyBox-class)

| Idea | Why it fits |
|------|-------------|
| One (or few) binaries, many applets | Small attack surface; understandable system; fast rebuilds |
| Static or nearly static | Reproducible images; fewer shared-lib footguns for the core |
| Explicit applet set | Only what the OS needs; rest is capability or optional layer |
| Implement language open | C (BusyBox/toybox), Go multicall, Rust multicall—engineering choice |

The agent supervisor and shell can sit **beside** the multicall core, or parts of them can be applets of a larger multicall. Product rule: the human still sees one goal path; process layout is an implementation detail.

### Cosmopolitan / APE-style portability

[Cosmopolitan Libc](https://github.com/jart/cosmopolitan) / Actually Portable Executable is a **tool and bootstrap** direction, not a second desktop stack:

- Ship agent helpers, fix-up tools, or capability binaries that run on many hosts without a rebuild matrix.  
- Useful when a skill must run on a coworker’s machine, in CI, or on a recovery environment with the same blob.  
- Complements multicall core: core stays small on the installed system; portable blobs travel with capabilities.  
- Does **not** replace Linux+Wayland for the interactive desktop. Does **not** mean “browser-only OS.”

Use Cosmopolitan where portability pays for itself. Do not force the compositor or the whole desktop through APE.

### What we still optimize

Even with a tiny core, **speed of real work** matters: allocators, compiler/runtime choices, kernel config, hot-path patches. Multicall + portable tools are about **shape and surface**; optimizations are about **measured performance** on real hardware.

## Desktop and shell

- **Wayland** as the display path. No X11-first design.  
- **Modal UI:** work mode and agent interaction are the center. Extra windows and tabs exist for workflows that need them; they are not the default layout.  
- **Shell is first-class:** ordinary command line remains (can be a real shell applet or a dedicated shell binary). Plain-language goals hit the same supervised agent stack (not a second, unconstrained agent).  
- Surfaces (editor, terminal, files, embedded web) share project/mode context.  
- Heavier tools (full browser, IDE-class editor) are **opt-in layers**, not the default rootfs bulk.

## Agent supervisor

Think **supervisord / init for agents**, not “spawn a chat process and hope.”

| Concern | Direction |
|---------|-----------|
| Lifecycle | Start, stop, restart, idle kill; explicit, inspectable |
| Identity | Each agent runs as its own Unix user (or equivalent mapped identity) |
| Rights | ACLs and capability grants per agent; human user is not the agent’s effective uid for work |
| Intercommunicate | Named channels / IPC the supervisor allows; no free-for-all among agents |
| Policy | Confirm hooks for high-risk steps; shared action log; stop always works |
| Multi-agent | Multiple agents concurrent when useful; still one visible mode for the human |

Which library implements the agent loop is an engineering choice. **Supervisor + identity + ACLs are product shape.**

## Base system rules

- Prefer multicall core + explicit layers over a full consumer userspace.  
- Pull in fuller packages only when a real path needs them (Develop mode may need a real toolchain—not the whole app menu).  
- Cosmopolitan/APE where portability of tools/capabilities matters.  
- **Not enough:** stock desktop plus an AI box.  
- **Not enough:** only a busybox image with no goal/agent path.  
- **Not the whole product:** a fast toolchain or a clever single binary alone.  
- **Not the product goal:** “boots in QEMU.” QEMU is fine for CI smoke; product proof is real tasks on real hardware when available.

## Composition sketch (not a frozen design)

| Piece | Example options | Notes |
|-------|-----------------|--------|
| Kernel | Linux | Own defconfig; optimize what we ship |
| Core applets | BusyBox, toybox, custom multicall | Mount, net, basic utils |
| Init / agent control | Dedicated supervisor binary | May double as pid 1 helper or run under a tiny init |
| Shell | Real shell + plain-language front | Same agent stack |
| Desktop | Wayland compositor + modal shell UI | Windows optional |
| Portable helpers | Cosmopolitan/APE builds | Capabilities, bootstrap, recovery |
| Heavy tools | Separate install/layer | Editor, browser engine, compilers as needed |

## v1 image

x86_64 first. Prefer install/boot on real hardware for demos. CI may use QEMU. Image should be **explainable**: small core + agent stack + enough for the v1 real-work paths—not a mystery distro rootfs. More architectures later (not v1 program). Cosmopolitan portability of helpers is a plus in v1 if it unblocks a real capability path; it is not a checkbox.
