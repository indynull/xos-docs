# Novelty (honest)

Parent: [../VISION.md](../VISION.md) · Grounding: [REFERENCES.md](./REFERENCES.md)

This is **not** a claim that “nobody has done agents” or “nobody has done CRIU.” Most pieces of the stack are **prior art**. Novelty, if any, is in **composition and product constraints** for an OS-level harness—and that is a **hypothesis** until built and compared.

## Prior art (grounded)

| Area | What already exists | Sources |
|------|---------------------|---------|
| Goal / command nucleus HCI | Raskin’s humane interface program; Archy/Enso-class command UIs | Raskin 2000 (ISBN 0-201-37937-6); review DOI 10.1145/967135.967153; Archy overview (secondary) |
| Process C/R on Linux | CRIU project and scientific/container use | criu.org; DOI 10.1051/epjconf/202429507046 |
| GPU + CRIU | CRIUgpu and vendor CUDA checkpoint paths | DOI 10.48550/arXiv.2502.16631; NVIDIA blog (URL in REFERENCES) |
| FS snapshots / COW trees | Btrfs design and COW B-tree ancestry | DOI 10.1145/2501620.2501623; DOI 10.1145/1326542.1326544 |
| Fast concurrent allocators | snmalloc (message-passing allocator) | DOI 10.1145/3315573.3329980; microsoft/snmalloc |
| Tool protocols for LLMs | MCP and many agent CLIs (opencode, hermes-class, etc.) | modelcontextprotocol.io (spec URL) |
| Multicall small userspace | BusyBox and relatives | busybox.net |
| Portable single-binary libc experiments | Cosmopolitan / APE | github.com/jart/cosmopolitan |
| Local LLM runtimes | llama.cpp and peers | github.com/ggerganov/llama.cpp |
| Profiled / custom kernels | Distro practice; team linux-rg | github.com/HaoZeke/linux-rg |

## What is **not** novel (do not market as invention)

- “AI on a desktop”  
- Using systemd units, Unix users, or ACLs for isolation  
- Preferring a thin userspace  
- Preferring btrfs snapshots for rollbacks  
- Dual local+remote models in the abstract  
- Shell + natural language in the abstract  

## Product hypothesis (where novelty might live)

| Hypothesis | Why it might be “can’t get this from Ubuntu + chat CLI” | Falsifiable if… |
|------------|--------------------------------------------------------|-----------------|
| **OS-level agent harness** | Agents are first-class OS objects: identity, unit isolation, durable state, teach→capability, dual path—not an app bolted on | A stock DE + agent CLI matches isolation, durability, and teach-path UX without custom OS integration |
| **Capability as OS object** | Patterns learned from normal use persist under sandbox policy at OS scope | Same persistence works with only project-scoped agent tools and no OS identity story |
| **Process C/R *integrated* with that harness** | CRIU (candidate) + btrfs + per-agent paths as one lifecycle—not optional glue | Users already get reliable agent freeze/resume without OS integration |

These are **design claims**, not published results. Cite prior art for components; cite **measurements** when claiming wins (latency, restore success rate, size).

## Allocator / “15% faster” note

snmalloc is a real research allocator (DOI above). Any specific “15% compile speedup” or “Chromium crashes under snmalloc-as-glibc-malloc” line in discussion is **team anecdote** until reproduced and written up. It supports the *policy* “gate allocator swaps with an app matrix,” not a novelty claim for xOS.

## CRIU assignment (ownership)

Process checkpoint/restore for agents (CRIU candidate, kernel integration, dump-friendly agent trees) is **interesting ownership** for R. Goswami / CRIU track—**direction**, not a v1 pass/fail gate (see design review demotion). Track as vault issue when project scaffold exists.

## Citation hygiene

- Every factual “paper says X” needs a DOI or primary URL from [REFERENCES.md](./REFERENCES.md).  
- Prefer ookcite `validate_doi` / `verify_references` before adding new DOIs.  
- Free-text reverse lookup can return **wrong** papers; check title match.
