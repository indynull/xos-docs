# Hardware profiles and performance

Parent: [../VISION.md](../VISION.md) · See: [TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md) · [LINKERS_LOADERS.md](./LINKERS_LOADERS.md)

> **Exploration / candidate notes — not committed product law.**  
> QEMU-first bring-up is fine for the harness. Profile machinery is direction, not a v1 spine gate.

## Problem

Team default for early work: **QEMU first** for bring-up and harness iteration—**and do not ignore real hardware**.

| Target | Role |
|--------|------|
| **QEMU** | First path for CI and early agent harness; fast loop |
| **Real hardware** | Drivers, performance, and especially **local LLM** (virtio-gpu is not enough for serious local inference) |
| **Product fail** | Only a QEMU boot screenshot with no harness path |

Drivers, firmware, power, GPU, Wi‑Fi, storage, and scheduler behavior only show up on real machines. Install contents and kernel knobs should be **deliberate per machine class**.

Personalization matters (different users, different hardware), but the **product shape** stays one: goals, modes, supervised agents, capabilities. What changes is the **base profile**—kernel, drivers, firmware, and which layers land on disk.

See also model stack vs hardware: [MODEL_STACK.md](./MODEL_STACK.md).

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
| “Agentic” kernel patches | Speculative—only if something as clear as audio’s `-rt` stack emerges. **Unlikely early** for generic agent UX. **CRIU integration is different:** C/R is first-class and should be designed **with the kernel we ship** (config, required features, deep integration)—not a random userspace package on an arbitrary kernel |
| CRIU + kernel | Primary profiles enable and validate the kernel surface CRIU needs; treat breakages as base bugs |

## Performance

- Optimize **measured** paths on the target profile (scheduler, network, memory reclaim, storage, compositor).  
- Toolchain and libc opts remain in scope ([TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md)).  
- Agent traffic (many units, snapshots, CRIU images) stresses FS and MM—profile testing should include agent load, not only idle desktop.  
- Personalization of layers is expected; chaos is not. Profiles are named, documented, and testable.

## What is out of scope as a product story

- Shipping one QEMU image as the **only** product story forever  
- Claiming multi-arch + every laptop without profiles  
- Kernel patch tourism without a named profile and a measured win  
- Forcing every user onto one hardware personality  
- Serious local LLM as a v1 requirement on virtio-gpu or $180-class machines

## v1 expectation

- **QEMU-first** path works for harness bring-up and CI.  
- At least **one real-machine profile** path documented (even if rough) for CRIU and anything that needs real devices/GPU.  
- Expand profiles as hardware and people join—not a v1 multi-SKU program.
