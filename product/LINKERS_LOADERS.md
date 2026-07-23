# Linkers, loaders, formats, and runtime identity

Parent: [../VISION.md](../VISION.md) · See: [TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md) · [../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md) · [HARDWARE_PROFILES.md](./HARDWARE_PROFILES.md)

> **Exploration / candidate notes — not committed product law.**  
> Spine docs stay near “real system libc by default.” Detail here is for implementers later.

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

If ELF interpreter, sonames, symbol versions, open DSOs, TLS, or vDSO coupling drift between dump and restore, CRIU fails. Design that graph deliberately.

## What “OS” means here (runtime)

When we say OS base, we also mean:

| Piece | Why it matters |
|-------|----------------|
| **Executable format** | ELF first; load address, interp, notes |
| **Library format** | Shared objects, versioning, how agents open them |
| **Linker** | How agent and capability binaries are produced (static vs dynamic, rpath) |
| **Loader / dynamic linker** | Policy for the agent happy path; one story, not accidental mix |
| **Debug symbols / build-id** | Restore failures and hung agents must be diagnosable |
| **System libc** | Real system libc (typically **glibc**)—what already runs browsers and toolchains |

Formats and debug identity are part of thinking about an OS. They do not require inventing a new libc on day one.

## v1 rule: stay close to what works

**Consensus for getting something running:** use **what is already there** (glibc-class system libc, normal ELF toolchain). Stay close to libc unless a departure is necessary and measured.

| Why | |
|-----|--|
| Real apps depend on **implementation details and even bugs** in core libraries | A full libc replacement is not a side quest |
| Getting a browser onto an all-new libc is on the order of **a whole project proposal by itself** | Do not put that on the v1 critical path |
| Development is agent-assisted (Grok) | Smaller surface vs reality is safer than a novel runtime for DX fashion |
| Rust/Go as *languages* are fine for our own tools | That is not the same as replacing system libc |

### Pluggable mallocs (optimize carefully)

Base toolchain work can include **pluggable mallocs** (e.g. snmalloc under glibc) with real wins—and real breakage.

Documented class of failure: a booting system can compile **~15% faster** with snmalloc replacing glibc malloc, while **Chromium-class browsers crash** because they rely on particular glibc malloc behavior (including over-allocation quirks). Treat that as the rule: **measure, and gate malloc swaps with app matrix tests**—especially browsers and other app-layer beasts—not only compile benchmarks.

| v1 | Later |
|----|--------|
| Prefer stock or well-known-compatible allocator defaults | Broader pluggable malloc story if the matrix is green |
| Document which apps must pass before flipping defaults | Profile-specific malloc if needed |

### Speculative later (future version—not v1)

| Idea | Status |
|------|--------|
| Replace system libc with a **Rust** (or other) runtime + shims (e.g. FFI) | Future research; heavy; interesting for long-horizon Grok extension—not how we ship v1 |
| Custom executable/library formats beyond ELF | Only if measured need |
| Full alternate-libc app ecosystem | Separate mega-effort (browser alone is huge) |

Bernhard’s bar: think about this for a **future version**; to get something up, use what exists.

## Cosmopolitan / APE (optional, not system libc)

[Cosmopolitan](https://github.com/jart/cosmopolitan) / APE: interesting for **single-binary portable demos**—not a usable **system libc** for agents + desktop + CRIU.

| Use | Fit |
|-----|-----|
| One-shot portable tools | Optional |
| System libc | **No** |
| “Build once run anywhere” product identity | **No** |

## CRIU and this layer

| Restore failure class | Response |
|----------------------|----------|
| Missing `.so` after upgrade | Static agent cores or freeze DSO set on agent subvol |
| Different libc build | Same system libc on dump and restore; kernel we ship |
| Wrong ELF interpreter | One interpreter policy on the image |
| Undebuggable dump failure | build-id + symbols policy for agent trees |
| TLS / vDSO / kernel coupling | CRIU **kernel-deep** ([../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md)) |

**CRIU is first-class** ([criu.org](https://criu.org/)): on-image, supervisor-owned, tested dump→restore, integrated with our kernel.

## Browser and app layer vs OS-adjacent

Browsers are **application layer**: they must work on our system libc, but they are not the product nucleus. A full alternate-libc browser port is not “just DE work”—it is enormous. Keep browsers on the known-good libc path.

## What this is not

- Not a multi-site package-stack product for restore.  
- Not Cosmopolitan-as-system-libc.  
- Not rewrite-libc-in-Rust for v1.  
- Not “malloc swap without a browser/app matrix.”  
- Not ignoring formats and debug info.

## v1 expectation

- **glibc-class** (or other true system libc) documented for the image and agent tree.  
- Linker/loader/static-vs-dynamic policy for the **canonical agent tree**.  
- Debug/build-id policy good enough to diagnose CRIU and agent failures.  
- CRIU dump→restore test on primary hardware profile.  
- Any malloc/pluggable-allocator experiment: compile win **and** browser/app smoke before default.  
- Cosmopolitan and Rust-libc: optional/future only.
