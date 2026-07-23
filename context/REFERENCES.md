# References (grounded)

Parent: [../VISION.md](../VISION.md)

Primary sources and **ookcite-validated DOIs** used to ground design notes. Do not invent citations. Team Slack reports are labeled **anecdote**, not literature.

## How to use

- Prefer **DOI** or **canonical project URL** over second-hand summaries.  
- Product **law** (BRIEF/VISION/principles) should not claim novelty that is not argued in [NOVELTY.md](./NOVELTY.md).  
- Exploration docs may cite candidates; mark *unverified* if a DOI was not checked.

## Validated bibliographic records (ookcite)

| Topic | Citation (IEEE-style from ookcite) | DOI |
|-------|-------------------------------------|-----|
| CRIU (scientific use) | F. Andrijauskas et al., “CRIU - Checkpoint Restore in Userspace for computational simulations and scientific applications,” *EPJ Web Conf.*, vol. 295, p. 7046, 2024 | [10.1051/epjconf/202429507046](https://doi.org/10.1051/epjconf/202429507046) |
| CRIU GPU extension | R. Stoyanov et al., “CRIUgpu: Transparent Checkpointing of GPU-Accelerated Workloads,” arXiv, 2025 | [10.48550/arXiv.2502.16631](https://doi.org/10.48550/arXiv.2502.16631) |
| snmalloc | P. Liétar et al., “snmalloc: a message passing allocator,” *ISMM 2019* | [10.1145/3315573.3329980](https://doi.org/10.1145/3315573.3329980) |
| Btrfs | O. Rodeh, J. Bacik, and C. Mason, “BTRFS,” *ACM Trans. Storage*, vol. 9, no. 3, 2013 | [10.1145/2501620.2501623](https://doi.org/10.1145/2501620.2501623) |
| COW B-trees (Btrfs ancestry) | O. Rodeh, “B-trees, shadowing, and clones,” *ACM Trans. Storage*, vol. 3, no. 4, 2008 | [10.1145/1326542.1326544](https://doi.org/10.1145/1326542.1326544) |
| Humane Interface (review) | D. Brown, “Review of The humane interface,” *ACM SIGCHI Bulletin* supplement, 2002 | [10.1145/967135.967153](https://doi.org/10.1145/967135.967153) |

## Books / ISBN (primary; no solid DOI for full text)

| Topic | Primary | Identifier |
|-------|---------|------------|
| Raskin, *The Humane Interface* | Addison-Wesley, 2000 | ISBN **0-201-37937-6** / **978-0-201-37937-2** (see also Open Library / publisher records; review DOI above) |

## Project / specification primaries (URLs)

| Topic | Canonical source | Notes |
|-------|------------------|--------|
| CRIU | https://criu.org/ · https://github.com/checkpoint-restore/criu | Project site + source; license GPLv2 (see tree) |
| go-criu / P.Haul | https://github.com/checkpoint-restore/go-criu | Programmatic CRIU control |
| Model Context Protocol (MCP) | https://modelcontextprotocol.io/ · spec e.g. https://modelcontextprotocol.io/specification/2025-11-25 | Open protocol for tools/context; **not** a peer-reviewed novelty claim for xOS |
| Cosmopolitan Libc / APE | https://github.com/jart/cosmopolitan | Source of truth; **not** system-libc literature |
| BusyBox | https://www.busybox.net/ · https://git.busybox.net/busybox | Multicall userspace; Zenodo snapshots exist but treat project site as primary |
| llama.cpp | https://github.com/ggerganov/llama.cpp | Local inference candidate; no DOI required |
| NVIDIA CUDA checkpoint + CRIU | https://developer.nvidia.com/blog/checkpointing-cuda-applications-with-criu/ | Vendor blog; pair with CRIUgpu paper for GPU C/R |
| linux-rg (profile kernel practice) | https://github.com/HaoZeke/linux-rg | Team prior art for multi-profile kernels—not a paper |
| hzArchiso | https://github.com/HaoZeke/hzArchiso | Team prior art for intentional install sets |
| Archy (historical) | https://en.wikipedia.org/wiki/Archy_(software) | Secondary overview; primary ideas in Raskin 2000 |
| Enso / Humanized | Contemporary demos / Humanized Enso (historical product) | Treat as HCI prior art, not a formal standard |

## Team reports (anecdote — not literature)

| Claim | Status |
|-------|--------|
| glibc + snmalloc ~15% faster compiles; Chromium crashes on glibc malloc quirks | **Team report** (B. Rosenkränzer, project Slack). Consistent with *why* allocator swaps need app matrices; **do not** cite as measured xOS result or as snmalloc paper result. |
| Agent restore “hit or miss” in practice | **Operator experience** (R. Goswami); motivates CRIU + dump-friendly layouts as *hypothesis*, not a survey paper. |

## Rejected / confusable sources

ookcite free-text can resolve **wrong** papers (e.g. unrelated “MCP” or “capability” hits). Only DOIs listed under **Validated** above were batch-checked `VALID` via ookcite `verify_references` (2026-07-23 session). Re-verify before formal publication.
