# Technical shape

Parent: [../VISION.md](../VISION.md)

Guesses only. Swap parts if the idea still holds. The spine below is intentional: **Wayland + modal shell**, **agent supervisor (systemd-class units)**, **per-agent Unix identity + ACLs**, **btrfs + CRIU durability**, **radical small base**, **real-hardware kernel profiles**. Not “stock desktop + chat,” not “Ubuntu minus packages,” and not “QEMU demo distro.”

UX lineage (Archy / Enso inspiration): [../concepts/UX_LINEAGE.md](../concepts/UX_LINEAGE.md).

## Layers

| Layer | Direction |
|-------|-----------|
| Kernel | Linux (v1). **Hardware-specific profiles** and knobs that earn their keep—not a from-scratch kernel; not QEMU-as-target |
| Filesystem | **btrfs** default for agent homes, workspaces, capability store, CRIU image dirs (subvols + snapshots + send/receive) |
| Core userspace | Prefer a **small multicall core** (BusyBox / toybox / custom multicall) over a full GNU userspace by default |
| Libc / portable runtime | **Cosmopolitan** as libc alternative and APE where it pays; glibc remains for heavy desktop pieces until proven otherwise |
| Limits | Process isolation; network/file limits; **per-agent Unix user + ACLs** |
| Init / units | **systemd** (or compatible replacement that keeps unit isolation): one unit per agent, sandbox features, restart policy |
| Agent supervisor | Orchestrates units + policy + channels; may be systemd itself plus thin policy layer, or a dedicated supervisor that *uses* units |
| Checkpoint / restore | **CRIU first-class**, **kernel-integrated** (process) + **btrfs** snapshots (data) ([../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md)) |
| Runtime ABI | Linkers, loaders, libc identity—glibc and/or **Cosmopolitan as libc alternative** ([LINKERS_LOADERS.md](./LINKERS_LOADERS.md)) |
| Capability store | Versioned tools; local first; lives on snapshotted subvols |
| Agents | One or more workers; each **user account** + unit + memory/data paths; reuse existing stacks if they fit |
| Shell | CLI + plain-language goal entry; modes; action log; editor/terminal surfaces |
| Desktop | Wayland compositor; **modal** focus (mode + agent), multi-window allowed not default |
| Web engine | Embed when needed—not the main UI |
| Packaging | Reproducible images; fast rebuilds; prefer real hardware for product proof |

## Think larger about the base

The default mental model is **not** “fork a consumer distro and install an agent.” Prefer:

```text
Linux kernel (hardware profiles; CRIU-capable / C-R integrated)
  → btrfs (subvols for agents, workspaces, images)
  → small multicall core (BusyBox-class …)
  → runtime ABI: linker/loader policy; glibc and/or Cosmopolitan
  → systemd-class units + supervisor (one unit / user per agent)
  → CRIU first-class (process freeze) + btrfs snapshots (data freeze)
  → shell (CLI + plain language) + Wayland modal surface
  → capabilities / optional fuller tools
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

### Cosmopolitan as libc alternative (+ APE)

[Cosmopolitan Libc](https://github.com/jart/cosmopolitan) is a first-class **libc alternative** to explore—not only a bag of portable helpers:

- Link agent helpers, capabilities, and selected tools against Cosmo; reduce host DSO drift that breaks CRIU restores.  
- APE blobs for bootstrap, recovery, and cross-host skills.  
- Complements multicall core and explicit **linker/loader policy** ([LINKERS_LOADERS.md](./LINKERS_LOADERS.md)).  
- Does **not** mean the whole Wayland desktop must be Cosmo-linked on day one. Declare runtime per tree.

### What we still optimize

Even with a tiny core, **speed of real work** matters: allocators, compiler/runtime choices, kernel config, hot-path patches. Multicall + portable tools are about **shape and surface**; optimizations are about **measured performance** on real hardware.

### Hardware profiles (not QEMU-first)

QEMU is CI smoke. Product and performance work target **real machines**. Full write-up: [HARDWARE_PROFILES.md](./HARDWARE_PROFILES.md).

**Shape of prior art (examples, not hard deps):**

- [linux-rg](https://github.com/HaoZeke/linux-rg) — multi-profile kernel: shared base + per-machine overlays, documented targets, patches tracked per profile.  
- [hzArchiso](https://github.com/HaoZeke/hzArchiso) — install/image where package and driver set is intentional (what lands and why).

| Concern | Direction |
|---------|-----------|
| Drivers | Real hardware first; prefer userspace / least privilege when third-party code is risky |
| Kernel config | Per-profile overlays; measure; keep diffs small |
| “Agentic” kernel patches | Speculative (audio `-rt` analogy). Unlikely early; do not block userspace |
| Personalization | Different machines/users → different layers; product shape stays one |
| Install set | Profile-driven; not one giant generic rootfs |

### Linkers, loaders, runtime identity

Hit-or-miss agent restore is often a **linker/loader/libc** problem (wrong layer: site package stacks). Fix the runtime ABI of agent trees; pair with kernel-deep CRIU. Full write-up: [LINKERS_LOADERS.md](./LINKERS_LOADERS.md).

## Desktop and shell

- **Wayland** as the display path. No X11-first design.  
- **Modal UI:** work mode and agent interaction are the center. Extra windows and tabs exist for workflows that need them; they are not the default layout.  
- **Shell is first-class:** ordinary command line remains (can be a real shell applet or a dedicated shell binary). Plain-language goals hit the same supervised agent stack (not a second, unconstrained agent).  
- Surfaces (editor, terminal, files, embedded web) share project/mode context.  
- Heavier tools (full browser, IDE-class editor) are **opt-in layers**, not the default rootfs bulk.

## Agent supervisor + systemd

Think **init/units for agents**, not “spawn a chat process and hope.”

**systemd** (or a compatible replacement) is a practical fit: `User=`, cgroups, sandbox directives, restart policy, journal hooks. Isolation features already exist; use them so agents cannot touch what they should not. A thin **policy supervisor** can sit above units (goal routing, confirm UI, inter-agent channels) without reinventing service management.

| Concern | Direction |
|---------|-----------|
| Lifecycle | Unit start/stop/restart; idle park; explicit, inspectable |
| Identity | Each agent: **own Unix user** + unit + home/memory subvol |
| Rights | ACLs + systemd sandbox + capability grants; human is not the agent’s effective uid |
| Memories / data | Durable paths on btrfs (not only process heap); concrete store per agent |
| Intercommunicate | Named channels / IPC the supervisor allows; no free-for-all |
| Policy | Confirm hooks; shared action log; stop always works |
| Multi-agent | Multiple units concurrent; one visible work mode for the human |

Which library implements the agent *loop* is an engineering choice. **Units + identity + ACLs + durable storage are product shape.**

## Checkpointing: CRIU + btrfs

Two layers, both in:

| Tool | Role |
|------|------|
| **btrfs** | Subvols and snapshots for agent homes, workspaces, capability store, CRIU image dirs; seamless backups / send |
| **CRIU** | Freeze/restore **running** agent process trees (warm state, mid-loop) |

Compose: dump process → images on subvol → snapshot subvol. CRIU fail-soft still leaves btrfs truth. Details: [../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md).

**Not v1 product goals:** full Wayland DE CRIU, cross-kernel live migration as the pitch. **Yes:** btrfs default durability; **first-class CRIU** deeply tied to the **kernel we ship** (config, features, and any needed integration—not a random distro package bolted on); supervisor dump/restore of the canonical agent tree + tested restore; dump-friendly link/load policy; unit-per-agent.

CRIU is GPLv2. Checkpoint images and secret-bearing snaps need swap-class care.

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
| Kernel | Linux + machine profile overlays; CRIU-capable | C/R integrated with the kernel we ship |
| Core applets | BusyBox, toybox, custom multicall | Mount, net, basic utils |
| Init / agent control | systemd units + thin policy supervisor | One unit/user per agent; sandbox flags |
| Filesystem | btrfs | Agent/workspace/image subvols + snapshots |
| Checkpoint | CRIU first-class + btrfs snaps | Kernel-deep process C/R + data |
| Runtime ABI | Linker/loader policy; glibc and/or Cosmo | Dump-friendly agent trees |
| Shell | Real shell + plain-language front | Same agent stack |
| Desktop | Wayland compositor + modal shell UI | Windows optional |
| Cosmopolitan | Libc alternative + APE | Helpers, capabilities, experiments |
| Heavy tools | Explicit layers | Editor, browser engine, compilers |

## v1 image

x86_64 first. Prefer install/boot on **one documented real-machine profile**. CI may use QEMU for smoke only. Image should be **explainable**: small core + **kernel with CRIU path** + agent stack + **documented linker/loader/libc policy** + enough for the v1 real-work paths. More architectures later (not v1 program).
