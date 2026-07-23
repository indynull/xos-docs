# xOS

Design notes for a Linux desktop that starts from **what you want to do**, not from which app to open.

```text
goal  →  capability  →  result   (in a work mode)
```

**Spine (product shape):** **OS agent harness** (the wedge) · dual path · capabilities · CRIU + identities · dual model stack · goal-first surface · lean base near system libc. QEMU-first bring-up OK; real HW when virt is not enough.

---

## Read in order

1. [BRIEF.md](./BRIEF.md) — two paragraphs  
2. [VISION.md](./VISION.md) — one page  
3. Details below — only if you need them  

If a detail file disagrees with the brief or one-pager, fix the detail file.

---

## Detail files

| Area | Files |
|------|--------|
| Rules | [principles/PRINCIPLES.md](./principles/PRINCIPLES.md) · [principles/NON_GOALS.md](./principles/NON_GOALS.md) |
| Ideas | [concepts/WORK_MODES.md](./concepts/WORK_MODES.md) · [concepts/CAPABILITIES.md](./concepts/CAPABILITIES.md) · [concepts/AGENTS.md](./concepts/AGENTS.md) · [concepts/CHECKPOINTING.md](./concepts/CHECKPOINTING.md) · [concepts/UX_LINEAGE.md](./concepts/UX_LINEAGE.md) |
| Product | [product/WEDGE.md](./product/WEDGE.md) · [product/DECISION.md](./product/DECISION.md) · [product/V1_SCOPE.md](./product/V1_SCOPE.md) · [product/SUCCESS_CRITERIA.md](./product/SUCCESS_CRITERIA.md) · [product/TECHNICAL_SHAPE.md](./product/TECHNICAL_SHAPE.md) · [product/OS_VS_DE.md](./product/OS_VS_DE.md) · [product/MODEL_STACK.md](./product/MODEL_STACK.md) · [product/HARDWARE_PROFILES.md](./product/HARDWARE_PROFILES.md) · [product/LINKERS_LOADERS.md](./product/LINKERS_LOADERS.md) · [product/SECURITY.md](./product/SECURITY.md) · [product/GOVERNANCE.md](./product/GOVERNANCE.md) |
| Background | [context/PROBLEM.md](./context/PROBLEM.md) · [context/ALTERNATIVE_TRACKS.md](./context/ALTERNATIVE_TRACKS.md) |
| Process | [meta/HOW_TO_ITERATE.md](./meta/HOW_TO_ITERATE.md) · [meta/CHANGELOG.md](./meta/CHANGELOG.md) |

**Status:** design notes only. Not implemented. Working name: xOS.
