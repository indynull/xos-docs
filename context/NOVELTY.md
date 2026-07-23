# Novelty (honest)

Parent: [../VISION.md](../VISION.md) · [REFERENCES.md](./REFERENCES.md) · [HCI.md](./HCI.md) · Bib: [../refs/xos-docs.bib](../refs/xos-docs.bib)

## Prior art (do not market as invention)

| Area | Exists | Bib / URL |
|------|--------|-----------|
| Humane / command-oriented HCI | Raskin 2000; Archy/Enso-class UIs | `raskinHumaneInterface2000`, `brownReviewHumaneInterface2002` |
| Mixed-initiative UI | Horvitz 1999 | `horvitzPrinciplesMixedInitiative1999` |
| Human-AI guidelines | Amershi et al. CHI 2019 | `amershiGuidelinesHumanAIInteraction2019` |
| Direct manipulation | Shneiderman line | `shneidermanDirectManipulation1997` |
| LLM + DM hybrids | DirectGPT CHI 2024 | `massonDirectGPT2024` |
| Explainability / control surveys | Abdul et al. 2018 | `abdulTrendsTrajectoriesExplainable2018` |
| Linux process C/R | CRIU project + papers | `andrijauskasCriuCheckpointRestore2024`, criu.org |
| GPU C/R | CRIUgpu | `stoyanovCriugpuTransparentCheckpointing2025` |
| FS snapshots | Btrfs | `rodehBtrfs2013`, `rodehBtreesShadowingClones2008` |
| Allocators | snmalloc | `lietarSnmallocMessagePassing2019` |
| Tool protocols for LLMs | MCP | modelcontextprotocol.io |
| Small multicall userspace | BusyBox | busybox.net |
| Portable single-binary libc experiments | Cosmopolitan | jart/cosmopolitan |
| Local LLM runtimes | llama.cpp | ggerganov/llama.cpp |

## Not novel

"AI on a desktop." systemd units. Unix users. ACLs. Preferring thin userspace. Dual local+remote models in the abstract. Natural language shells in the abstract.

## Product hypothesis (composition only)

| Hypothesis | Falsifiable if |
|------------|----------------|
| OS-level agent harness (identity + dual path + teach-to-capability + durable state) is not reproducible by stock DE + agent CLI | A stock DE + agent CLI matches isolation, durability, and teach-path UX without OS integration |
| Process C/R *integrated* with that harness beats optional glue | Users already get reliable agent freeze/resume without OS work |

These are design claims. Cite literature for components. Cite **measurements** for wins.

## CRIU ownership

Track: vault `xos-yl4d` (OWNER rgoswami). Direction, not v1 pass/fail. When shipped: kernel-deep + tested restore, numbers not anecdotes.

## Hygiene

Collection `xos-docs` + curated `refs/xos-docs.bib` are authoritative. Free-text ookcite can resolve wrong CHI papers; title-check every add.
