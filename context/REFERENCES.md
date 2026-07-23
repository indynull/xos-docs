# References (grounded)

Parent: [../VISION.md](../VISION.md) · HCI notes: [HCI.md](./HCI.md) · Novelty: [NOVELTY.md](./NOVELTY.md)

## Ookcite collection and BibTeX export

| Item | Value |
|------|--------|
| Collection | **`xos-docs`** (ookcite) |
| Curated BibTeX | [../refs/xos-docs.bib](../refs/xos-docs.bib) |
| Hygiene | `verify_references` on DOIs below; free-text reverse lookup can return wrong CHI papers -- check titles |

Re-export:

```bash
# via MCP: ookcite export_collection collection=xos-docs
# then replace or diff against refs/xos-docs.bib (curated subset)
```

Add DOIs with:

```text
ookcite batch_add_to_collection collection=xos-docs use_live_queries=true
```

Prefer DOIs over free text. Import large dumps with `import_bibliography` when needed.

## BibTeX keys in refs/xos-docs.bib

| Key | Topic | DOI / ID |
|-----|--------|----------|
| `andrijauskasCriuCheckpointRestore2024` | CRIU scientific use | 10.1051/epjconf/202429507046 |
| `stoyanovCriugpuTransparentCheckpointing2025` | CRIU + GPU | 10.48550/arXiv.2502.16631 |
| `lietarSnmallocMessagePassing2019` | snmalloc | 10.1145/3315573.3329980 |
| `rodehBtrfs2013` | Btrfs | 10.1145/2501620.2501623 |
| `rodehBtreesShadowingClones2008` | COW B-trees | 10.1145/1326542.1326544 |
| `raskinHumaneInterface2000` | Humane Interface (book) | ISBN 0-201-37937-6 |
| `brownReviewHumaneInterface2002` | Review of Raskin | 10.1145/967135.967153 |
| `amershiGuidelinesHumanAIInteraction2019` | Human-AI guidelines | 10.1145/3290605.3300233 |
| `horvitzPrinciplesMixedInitiative1999` | Mixed-initiative UI | 10.1145/302979.303030 |
| `shneidermanDirectManipulation1997` | Direct manipulation | 10.1145/238218.238281 |
| `abdulTrendsTrajectoriesExplainable2018` | Explainable systems survey | 10.1145/3173574.3174156 |
| `massonDirectGPT2024` | DM + LLM UI | 10.1145/3613904.3642462 |

## Project / spec primaries (URL)

| Topic | URL |
|-------|-----|
| CRIU | https://criu.org/ · https://github.com/checkpoint-restore/criu |
| MCP | https://modelcontextprotocol.io/specification/2025-11-25 |
| Cosmopolitan | https://github.com/jart/cosmopolitan |
| BusyBox | https://www.busybox.net/ |
| llama.cpp | https://github.com/ggerganov/llama.cpp |
| CUDA + CRIU (vendor) | https://developer.nvidia.com/blog/checkpointing-cuda-applications-with-criu/ |
| linux-rg | https://github.com/HaoZeke/linux-rg |
| hzArchiso | https://github.com/HaoZeke/hzArchiso |

## Team reports (anecdote, not literature)

| Claim | Status |
|-------|--------|
| snmalloc under glibc ~15% faster compiles; Chromium crashes on malloc quirks | Team report (B. Rosenkränzer). Motivates app-matrix policy. Not a paper result. |
| Agent restore hit-or-miss in practice | Operator report (R. Goswami). Hypothesis fuel for CRIU track, not a survey. |

## Hygiene

- Never invent DOIs. Run ookcite `verify_references` before adding rows.
- Collection may contain noisy free-text hits; **curated truth is `refs/xos-docs.bib`**.
- Cite with keys from that file in design notes.
