# Hardware profiles and performance

Parent: [../VISION.md](../VISION.md) · See: [TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md) · [BUILD_DEPLOY.md](./BUILD_DEPLOY.md)

## Problem

QEMU is a fine **CI smoke** target. It is a bad **product and performance** target.

Drivers, firmware, power, GPU, Wi‑Fi, storage, and scheduler behavior only show up on real machines. An agent-first OS that only “works in the guest” does not prove the idea. Install contents and kernel knobs should be **deliberate per machine class**, not one generic image that hopes for the best.

Personalization matters (different users, different hardware), but the **product shape** stays one: goals, modes, supervised agents, capabilities. What changes is the **base profile**—kernel, drivers, firmware, and which layers land on disk.

## Profile model

```text
product shape (same for everyone)
  ×  machine profile (kernel overlay, drivers, firmware, validated devices)
  ×  install profile (packages/layers for that job: develop laptop, lab desktop, …)
  →  bootable system you measure on that metal
```

| Piece | Meaning |
|-------|---------|
| **Machine profile** | CPU/GPU/platform, kernel config overlay, firmware, known-good modules, boot contracts |
| **Install profile** | What gets installed and why (minimal core + needed layers; not “entire universe”) |
| **Shared product** | Shell, modes, agents, CRIU/btrfs, capabilities—unchanged story |

### Prior art (shape, not a required dependency)

These are **examples of the profile discipline**, not mandates to fork Arch or ship those trees as xOS:

| Example | What to steal |
|---------|----------------|
| [linux-rg](https://github.com/HaoZeke/linux-rg) | Multi-profile kernel package: shared base + **per-machine config overlay**, documented hardware targets, patch applicability tracked per profile, validation on real boxes (laptop vs desktop) |
| [hzArchiso](https://github.com/HaoZeke/hzArchiso) | Install media / image where **package set and drivers are intentional** (including size discipline); offline-capable build; profile of what lands and why |

xOS may use different packaging (not necessarily Arch). The rule is the same: **profile-first, measure on metal.**

## Drivers

| Direction | Notes |
|-----------|--------|
| Real hardware first | CI in QEMU does not count as driver proof |
| Prefer userspace / least privilege where safe | Third-party code in the kernel is a long-term liability (printer-class lessons on other OSes); keep dangerous blobs out of kernel when alternatives exist |
| Firmware inventory | Profiles list required firmware; fail closed with a clear message when missing |
| GPU / media | Profile-specific; agent GPU CRIU is later and must not break base laptop/desktop profiles |
| “Agentic” kernel patches | Speculative—only if something as clear as audio’s `-rt` stack emerges. **Unlikely early.** Do not block userspace (agents, CRIU, shell) on a fantasy patch set |

## Performance

- Optimize **measured** paths on the target profile (scheduler, network, memory reclaim, storage, compositor).  
- Toolchain and libc opts remain in scope ([TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md)).  
- Agent traffic (many units, snapshots, CRIU images) stresses FS and MM—profile testing should include agent load, not only idle desktop.  
- Personalization of layers is expected; chaos is not. Profiles are named, documented, and testable.

## What is out of scope as a product story

- Shipping one QEMU image as the only proof  
- Claiming multi-arch + every laptop without profiles  
- Kernel patch tourism without a named profile and a measured win  
- Forcing every user onto one hardware personality

## v1 expectation

- At least **one real-machine profile** path documented (even if rough): what kernel/drivers/install set, how to build, how to smoke-test agents + CRIU/btrfs on that box.  
- QEMU remains allowed for CI only.  
- Expand profiles as hardware and people join—not a v1 multi-SKU program.
