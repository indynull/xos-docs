# Linkers, loaders, and runtime identity

Parent: [../VISION.md](../VISION.md) · See: [TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md) · [../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md) · [HARDWARE_PROFILES.md](./HARDWARE_PROFILES.md)

## Wrong layer vs right layer

Hit-or-miss **agent restore** is often blamed on “the package stack.” That is usually the **wrong layer**.

The process CRIU freezes is defined by **how it was linked and how the dynamic loader resolves it**: libc identity, ELF interpreter, `rpath`/`runpath`, symbol versions, open DSOs, TLS, vDSO, and whether the tree is mostly static or a web of shared libs. If those move under the agent between dump and restore, restore fails—no amount of module-tree pinning at the *wrong* abstraction fixes that cleanly.

```text
wrong layer:  “same module stack on every site” (site package narrative)
right layer:  linker + loader + libc/runtime identity for agent trees
```

## What we care about

| Concern | Direction |
|---------|-----------|
| **Loader identity** | Known ELF interpreter and loader policy for agent and capability binaries |
| **Libc path** | Explicit: system glibc **and/or** [Cosmopolitan](https://github.com/jart/cosmopolitan) as a **libc alternative** for chosen trees—not accidental mix |
| **Static vs dynamic** | Prefer dump-friendly layouts: fewer floating DSOs on the agent happy path; static or APE where it pays |
| **Symbol / ABI stability** | Agent trees pin *runtime* ABI (sonames, symbol versions), not a vague “stack id” from a foreign site |
| **Restore contract** | Dump and restore see the same loader-visible graph (paths, inodes of DSOs, or static blob) |
| **Pluggable allocators** | Fits base toolchain work (malloc choice is a link/load decision too) |

## Cosmopolitan as libc alternative

[Cosmopolitan Libc](https://github.com/jart/cosmopolitan) / APE is not only “portable helpers.” It is an interesting **libc alternative** for xOS:

| Use | Why |
|-----|-----|
| Agent helpers and capabilities | One blob; fewer host DSO surprises at restore |
| Bootstrap / recovery tools | Run without a full glibc root |
| Experiments | Compare Cosmo-linked agent trees vs glibc for CRIU reliability and size |
| Not default for everything | Compositor, full Develop IDE, and heavy desktop stacks may stay on system libc initially |

Policy: **declare** which libc/runtime a binary uses. Supervisor and CRIU paths should not guess.

## CRIU and the linker/loader story

| Restore failure class | Linker/loader response |
|----------------------|-------------------------|
| Missing `.so` after upgrade | Static or APE agent cores; or freeze DSO set with the agent subvol |
| Different libc | Same libc build on dump host; Cosmo trees avoid host libc drift |
| Wrong ELF interpreter | Image and agent install share one interpreter policy |
| TLS / vDSO / kernel coupling | Hardware/kernel profile must match CRIU needs ([HARDWARE_PROFILES.md](./HARDWARE_PROFILES.md); deep CRIU+kernel integration) |

CRIU remains **first-class** and **kernel-deep** ([../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md)). Linkers/loaders make the *userspace half* of restore boring.

## What this is not

- Not a multi-site scientific software distribution product.  
- Not “install Spack/EasyBuild/EESSI and call it done.”  
- Not forcing every binary through Cosmopolitan on day one.

## v1 expectation

- Document **runtime policy** for the canonical agent tree: libc (glibc and/or Cosmo), static vs dynamic, interpreter.  
- CRIU dump→restore test uses that policy on the primary hardware profile.  
- Cosmopolitan path demonstrated for at least one capability or agent helper (stretch OK if kernel/CRIU path is higher priority).  
- Toolchain work (pluggable mallocs, clang opts) stays aligned with how we *link* what we ship.
