# xOS

Design notes for a Linux desktop that starts from **what you want to do**, not from which app to open.

```text
goal  →  capability  →  result   (in a work mode)
```

**Product law (spine):** OS agent harness · dual path (instant + goals) · teach→sandboxed capabilities · supervised multi-agent identity · durable agent data (btrfs preferred) · lean multicall-leaning base · not just a DE fork or chat-on-Ubuntu.

**Exploration (not law):** CRIU timing, display stack, hardware profiles, linker/libc, dual model details—see product tech notes, labeled as candidates.

---

## Read in order

1. [BRIEF.md](./BRIEF.md) — two paragraphs  
2. [VISION.md](./VISION.md) — one page  
3. [product/WEDGE.md](./product/WEDGE.md) — what only we ship  
4. Details below — only if you need them  

If a detail file disagrees with the brief or one-pager, fix the detail file **or** mark the detail as exploration.

---

## Detail files

| Area | Files |
|------|--------|
| Rules | [principles/PRINCIPLES.md](./principles/PRINCIPLES.md) · [principles/NON_GOALS.md](./principles/NON_GOALS.md) |
| Ideas | [concepts/WORK_MODES.md](./concepts/WORK_MODES.md) · [concepts/CAPABILITIES.md](./concepts/CAPABILITIES.md) · [concepts/AGENTS.md](./concepts/AGENTS.md) |
| Product | [product/WEDGE.md](./product/WEDGE.md) · [product/DECISION.md](./product/DECISION.md) · [product/V1_SCOPE.md](./product/V1_SCOPE.md) · [product/SUCCESS_CRITERIA.md](./product/SUCCESS_CRITERIA.md) · [product/OS_VS_DE.md](./product/OS_VS_DE.md) · [product/SECURITY.md](./product/SECURITY.md) · [product/GOVERNANCE.md](./product/GOVERNANCE.md) |
| Exploration (candidates) | [product/TECHNICAL_SHAPE.md](./product/TECHNICAL_SHAPE.md) · [product/MODEL_STACK.md](./product/MODEL_STACK.md) · [product/HARDWARE_PROFILES.md](./product/HARDWARE_PROFILES.md) · [product/LINKERS_LOADERS.md](./product/LINKERS_LOADERS.md) · [concepts/CHECKPOINTING.md](./concepts/CHECKPOINTING.md) · [concepts/UX_LINEAGE.md](./concepts/UX_LINEAGE.md) *(optional)* |
| Background | [context/PROBLEM.md](./context/PROBLEM.md) · [context/ALTERNATIVE_TRACKS.md](./context/ALTERNATIVE_TRACKS.md) · [context/READING.md](./context/READING.md) · [context/REFERENCES.md](./context/REFERENCES.md) · [context/NOVELTY.md](./context/NOVELTY.md) · [context/HCI.md](./context/HCI.md) |
| Bibliography | [refs/xos-docs.bib](./refs/xos-docs.bib) (ookcite collection `xos-docs`) |
| Planning (implementation roadmap) | [planning/GROKOS_FULL_ROADMAP.md](./planning/GROKOS_FULL_ROADMAP.md) · [architecture SVG](./planning/grokos-architecture.svg) / [PNG](./planning/grokos-architecture.png) — phased P0–P9 roadmap, WBS, risk register; **not product law** (see spine above) |
| Process | [meta/HOW_TO_ITERATE.md](./meta/HOW_TO_ITERATE.md) · [meta/CHANGELOG.md](./meta/CHANGELOG.md) |

**Status:** design notes only. Not implemented. Working name: xOS.
