# xOS

Design notes for a Linux desktop that starts from **what you want to do**, not from which app to open.

```text
goal  →  capability  →  result   (in a work mode)
```

**Spine (product shape):** Wayland + modal shell (Archy/Enso-informed) · agents as user + unit + ACLs + durable data · first-class CRIU + btrfs · hardware profiles · EESSI-class pinned stack · multicall + optional Cosmopolitan/APE. QEMU is CI smoke, not the story.

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
| Product | [product/DECISION.md](./product/DECISION.md) · [product/V1_SCOPE.md](./product/V1_SCOPE.md) · [product/SUCCESS_CRITERIA.md](./product/SUCCESS_CRITERIA.md) · [product/TECHNICAL_SHAPE.md](./product/TECHNICAL_SHAPE.md) · [product/HARDWARE_PROFILES.md](./product/HARDWARE_PROFILES.md) · [product/BUILD_DEPLOY.md](./product/BUILD_DEPLOY.md) · [product/SECURITY.md](./product/SECURITY.md) · [product/GOVERNANCE.md](./product/GOVERNANCE.md) |
| Background | [context/PROBLEM.md](./context/PROBLEM.md) · [context/ALTERNATIVE_TRACKS.md](./context/ALTERNATIVE_TRACKS.md) |
| Process | [meta/HOW_TO_ITERATE.md](./meta/HOW_TO_ITERATE.md) · [meta/CHANGELOG.md](./meta/CHANGELOG.md) |

**Status:** design notes only. Not implemented. Working name: xOS.
