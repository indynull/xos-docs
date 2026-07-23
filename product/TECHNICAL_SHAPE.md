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
| Portable tools | Cosmopolitan Libc / APE-style **actually portable** binaries where they help (agent helpers, bootstrap tools, cross-host skills)—not required for every process |
| Limits | Process isolation; network/file limits; **per-agent Unix user + ACLs** |
| Init / units | **systemd** (or compatible replacement that keeps unit isolation): one unit per agent, sandbox features, restart policy |
| Agent supervisor | Orchestrates units + policy + channels; may be systemd itself plus thin policy layer, or a dedicated supervisor that *uses* units |
| Checkpoint / restore | **CRIU first-class** (process) + **btrfs** snapshots (data)—compose ([../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md)) |
| Software stack | EESSI-class **pinned optimized** tools for Develop/agents/CI ([BUILD_DEPLOY.md](./BUILD_DEPLOY.md)) |
| Capability store | Versioned tools; local first; lives on snapshotted subvols |
| Agents | One or more workers; each **user account** + unit + memory/data paths; reuse existing stacks if they fit |
| Shell | CLI + plain-language goal entry; modes; action log; editor/terminal surfaces |
| Desktop | Wayland compositor; **modal** focus (mode + agent), multi-window allowed not default |
| Web engine | Embed when needed—not the main UI |
| Packaging | Reproducible images; fast rebuilds; prefer real hardware for product proof |

## Think larger about the base

The default mental model is **not** “fork a consumer distro and install an agent.” Prefer:

```text
Linux kernel (hardware profiles / measured opts)
  → btrfs (subvols for agents, workspaces, images)
  → small multicall core (BusyBox-class …)
  → pinned software stack (EESSI-class revision)
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

### Cosmopolitan / APE-style portability

[Cosmopolitan Libc](https://github.com/jart/cosmopolitan) / Actually Portable Executable is a **tool and bootstrap** direction, not a second desktop stack:

- Ship agent helpers, fix-up tools, or capability binaries that run on many hosts without a rebuild matrix.  
- Useful when a skill must run on a coworker’s machine, in CI, or on a recovery environment with the same blob.  
- Complements multicall core: core stays small on the installed system; portable blobs travel with capabilities.  
- Does **not** replace Linux+Wayland for the interactive desktop. Does **not** mean “browser-only OS.”

Use Cosmopolitan where portability pays for itself. Do not force the compositor or the whole desktop through APE.

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

### Build / deploy stack (EESSI-class guarantees)

Agents and CRIU restores need a **pinned, optimized, multi-host** software stack—same spirit as [EESSI](https://www.eessi.io/). Hit-or-miss restores are often library and path drift, not only CRIU. Full write-up: [BUILD_DEPLOY.md](./BUILD_DEPLOY.md).

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

**Not v1 product goals:** full Wayland DE CRIU, cross-kernel live migration as the pitch. **Yes:** btrfs default durability; **first-class CRIU** on the image with supervisor dump/restore of the canonical agent tree + tested restore path; unit-per-agent.

CRIU is a **host** package (GPLv2). Checkpoint images and secret-bearing snaps need swap-class care.

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
| Kernel | Linux + machine profile overlays | e.g. linux-rg-style discipline |
| Core applets | BusyBox, toybox, custom multicall | Mount, net, basic utils |
| Init / agent control | systemd units + thin policy supervisor | One unit/user per agent; sandbox flags |
| Filesystem | btrfs | Agent/workspace/image subvols + snapshots |
| Checkpoint | CRIU first-class + btrfs snaps | Process + data; tested restore |
| Software stack | EESSI / EESSI-like / pinned modules | Same on desk and CI |
| Shell | Real shell + plain-language front | Same agent stack |
| Desktop | Wayland compositor + modal shell UI | Windows optional |
| Portable helpers | Cosmopolitan/APE builds | Capabilities, bootstrap, recovery |
| Heavy tools | Stack layer or image layer | Editor, browser engine, compilers |

## v1 image

x86_64 first. Prefer install/boot on **one documented real-machine profile**. CI may use QEMU for smoke only. Image should be **explainable**: small core + CRIU + agent stack + **pinned software stack revision** + enough for the v1 real-work paths. More architectures later (not v1 program).
