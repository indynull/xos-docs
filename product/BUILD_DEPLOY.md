# Build and deploy guarantees

Parent: [../VISION.md](../VISION.md) · See: [TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md) · [HARDWARE_PROFILES.md](./HARDWARE_PROFILES.md) · [../concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md)

Agent restore and day-to-day work both need a **boring, strong** software supply chain. Hit-or-miss agent state is partly a CRIU problem and partly a “which binary, which library, which path” problem. If the stack under the agent drifts, freeze/restore and capabilities rot.

## Goal

A build/deploy story with **reproducible, optimized, multi-host** software—same spirit as [EESSI](https://www.eessi.io/) (European Environment for Scientific Software Installations): a common stack you can mount or provision on laptop, workstation, CI, and cluster without rebuilding the world ad hoc each time.

We do **not** require adopting EESSI as a hard dependency on day one. We require the **guarantees** it points at.

## Guarantees we want

| Guarantee | Meaning |
|-----------|---------|
| **Reproducible installs** | Same profile + same stack revision → same tools on two machines (bit-identical where feasible; clearly versioned otherwise) |
| **Optimized for host class** | Builds (or selected binaries) match CPU/arch family; not lowest-common-denominator only |
| **Same stack on CI and desk** | Agent and capability tests run against the stack people actually use |
| **Explicit revision** | Every image and agent environment records stack id / commit / module set |
| **Layered, not opaque** | Core OS image separate from scientific/dev tool stack where that helps; both pinned |
| **Offline / cache friendly** | Serious work environments often need local or site caches (EESSI-like distribution) |

## Why this matters for agents

| Failure mode | Mitigation |
|--------------|------------|
| Agent restore “works on my laptop” only | Same CRIU + same libs + same paths via pinned stack |
| Capability built last month breaks | Capability declares required stack revision |
| Develop mode compiler ≠ CI compiler | One stack id for both |
| QEMU CI uses random distro packages | CI consumes the **same** stack artifact as the hardware profile (or a documented subset) |

CRIU images are host-bound; a stable userspace **under** the agent makes restores less hit-or-miss. btrfs snapshots of agent homes still need matching interpreters and shared libs when process restore is not used.

## Shape (options; pick one path and stick)

| Approach | Notes |
|----------|--------|
| **EESSI-compatible** | Consume or mirror EESSI for scientific/dev layers; OS stays thin |
| **EESSI-like private stack** | CVMFS or similar distribution of a project-owned optimized tree |
| **Prefix / module system** | Spack, EasyBuild, or prefix modules with a frozen “xOS stack” release |
| **Image-native freeze** | Entire rootfs content-addressed (more appliance-like); harder to personalize |

Multicall core + Cosmopolitan helpers remain for the **tiny OS and portable skills**. The **dev/science stack** is the large, versioned layer agents and Develop mode need.

## Relation to hardware profiles

- Hardware profile chooses **kernel, drivers, firmware**.  
- Build/deploy stack chooses **userspace tools and libs** (compilers, runtimes, scientific apps).  
- Both ids appear in `/etc` or equivalent and in agent action logs when a capability runs.

## v1 expectation

- Document the chosen stack mechanism (even if the full multi-site CVMFS story is later).  
- Pin a **stack revision** into the bootable image used for the real-work demo.  
- CI builds and agent tests reference that revision.  
- Full EESSI multi-site ops is not required for v1; the **pin + same-on-CI** bar is.
