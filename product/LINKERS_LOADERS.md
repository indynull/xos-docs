# Linkers, loaders, formats, and runtime identity

Parent: [../VISION.md](../VISION.md) · See: [TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md) · [../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md) · [HARDWARE_PROFILES.md](./HARDWARE_PROFILES.md)

## Right layer for agent restore

Hit-or-miss **agent restore** is not fixed by a shared site package narrative. CRIU freezes a process defined by:

- how it was **linked**  
- how the **dynamic loader** resolves it  
- which **executable / library format** it uses  
- what **debug symbols** and build-ids attach when something goes wrong  

```text
right layer:  linker + loader + formats + debug identity + system libc
wrong layer:  multi-site “same modules everywhere” product stories
```

If ELF interpreter, sonames, symbol versions, open DSOs, TLS, or vDSO coupling drift between dump and restore, CRIU fails. Design that graph; do not paper over it with distribution packaging slogans.

## What “OS” means here (runtime)

When we say OS base, we also mean:

| Piece | Why it matters |
|-------|----------------|
| **Executable format** | ELF (and any deliberate alternatives); load address, interp, notes |
| **Library format** | Shared objects, versioning, how agents open them |
| **Linker** | How agent and capability binaries are produced (static vs dynamic, rpath) |
| **Loader / dynamic linker** | Policy for the agent happy path; one story, not accidental mix |
| **Debug symbols / build-id** | Restore failures and hung agents must be diagnosable; symbols are part of the base story |
| **System libc** | Real system libc (typically glibc, or another **true** system libc if the team chooses)—not a demo single-binary libc |

## System libc (default)

| Direction | Notes |
|-----------|--------|
| **System libc is real** | Agents, desktop, and CRIU-facing trees run on a normal system libc path we control and optimize (pluggable mallocs, etc.) |
| **One policy** | Document which libc, which loader, static vs dynamic defaults for the **canonical agent tree** |
| **Dump-friendly** | Prefer fewer floating DSOs on the agent happy path; freeze DSO set with agent subvol when dynamic |
| **Restore contract** | Dump and restore see the same loader-visible graph |

### Speculative later (not v1 product)

| Idea | Status |
|------|--------|
| Replace system libc with a **Rust-implemented** runtime + shims (e.g. via FFI) | Research-grade; would need heavy work; interesting because Grok can help extend it—not a v1 commitment |
| Custom executable or library formats beyond ELF | Only if measured need; ELF first |

Do not block shell, agents, or CRIU on speculative libc replacement.

## Cosmopolitan / APE (interesting, not system libc)

[Cosmopolitan](https://github.com/jart/cosmopolitan) / APE is **interesting for single-binary, run-on-many-hosts demos**—not a realistic **system libc** for this OS.

| Use | Fit |
|-----|-----|
| Show-off / portable one-shot tools | Yes, optional |
| Bootstrap experiment | Maybe |
| System libc for agents, desktop, CRIU core | **No** — not usable in that role for this project |
| Framing as “build once run anywhere like Java” | Avoid; wrong product story |

Keep Cosmopolitan off the critical path. System identity is ELF + real system libc + our linker/loader policy.

## CRIU and this layer

| Restore failure class | Response |
|----------------------|----------|
| Missing `.so` after upgrade | Static agent cores or freeze DSO set on agent subvol |
| Different libc build | Same system libc on dump and restore; kernel/profile we ship |
| Wrong ELF interpreter | One interpreter policy on the image |
| Undebuggable dump failure | build-id + symbols policy for agent trees |
| TLS / vDSO / kernel coupling | CRIU **kernel-deep** with the kernel we ship ([../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md)) |

**CRIU is first-class** ([criu.org](https://criu.org/)): on-image, supervisor-owned, tested dump→restore, integrated with our kernel—not a hit-or-miss optional package.

## What this is not

- Not a multi-site scientific software distribution product.  
- Not Cosmopolitan-as-system-libc.  
- Not “rewrite libc in Rust” as a v1 gate.  
- Not ignoring formats and debug info.

## v1 expectation

- Document runtime policy for the **canonical agent tree**: system libc, loader, static vs dynamic, symbol/debug expectations.  
- CRIU dump→restore test on primary hardware profile uses that policy and our kernel.  
- Cosmopolitan: optional curiosity only; not required for v1.  
- Toolchain opts (malloc, clang) stay aligned with how we **link** what we ship.
