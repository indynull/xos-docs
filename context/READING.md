# Guided reading list and precis

Parent: [../VISION.md](../VISION.md) · Bib: [../refs/xos-docs.bib](../refs/xos-docs.bib) · Collection: ookcite `xos-docs` · Index: [REFERENCES.md](./REFERENCES.md)

Read this when a design note cites a key and you need the *actual* claim, not a marketing gloss. Product law is still BRIEF/VISION; this file is homework.

**Order.** HCI and Human-AI first (why dual path and teach-to-capability). Systems second (btrfs, CRIU, allocators). Specs and team repos last.

**Keys** match `refs/xos-docs.bib`. DOIs were ookcite-validated unless marked *primary URL only*.

---

## Track A -- HCI and Human-AI (why the product surface looks like it does)

### A1. Horvitz 1999 -- mixed initiative

- **Key:** `horvitzPrinciplesMixedInitiative1999`
- **Cite:** Eric Horvitz. "Principles of mixed-initiative user interfaces." CHI 1999. doi:[10.1145/302979.303030](https://doi.org/10.1145/302979.303030)
- **What it is:** Classic CHI paper on when automation should act vs when the user should keep the wheel.
- **Precis:** Automation that always drives is not clever; it is rude. Horvitz treats initiative as a resource to share. Systems should act under uncertainty only when expected utility beats the cost of interrupting the user, and they should leave cheap paths for the human to take back control. Timing, value of information, and dialogue structure matter more than raw model quality.
- **Why xOS cites it:** Dual path is not taste. Waiting on a model to open a terminal is unilateral automation. Instant tools keep user initiative for high-certainty actions; goal path covers multi-step work where automation can pay.
- **Does not buy:** A specific shell UI, compositor, or "always local LLM" story.
- **Read if:** You want the theoretical spine for dual path before arguing UI chrome.

### A2. Amershi et al. 2019 -- guidelines for Human-AI interaction

- **Key:** `amershiGuidelinesHumanAIInteraction2019`
- **Cite:** Saleema Amershi et al. "Guidelines for Human-AI Interaction." CHI 2019. doi:[10.1145/3290605.3300233](https://doi.org/10.1145/3290605.3300233)
- **What it is:** Eighteen design guidelines distilled from industry practice and validated with practitioners, covering initially, during interaction, when wrong, and over time.
- **Precis:** The paper turns "AI UX" from vibes into a checklist class: make clear what the system can do; show how well it can do it; time services based on context; support efficient dismissal and correction; scope services when confused; learn from user behavior carefully; mitigate social biases; remember recent context; notify of updates. Empirical part: designers find the guidelines applicable and useful when reviewing AI products.
- **Why xOS cites it:** Status, stop, action log, teach-to-capability (correction that sticks), and "do not freestyle after pattern is known" map to these guidelines without inventing a new HCI theory.
- **Does not buy:** Proof that OS integration is required. The guidelines apply to apps too; our wedge is *where* we enforce them.
- **Read if:** You review agent UX and need a standard vocabulary for "this is bad agency."

### A3. Shneiderman 1997 -- direct manipulation

- **Key:** `shneidermanDirectManipulation1997`
- **Cite:** Ben Shneiderman. "Direct manipulation for comprehensible, predictable and controllable user interfaces." IUI 1997. doi:[10.1145/238218.238281](https://doi.org/10.1145/238218.238281)
- **What it is:** Compact restatement of the direct-manipulation program: continuous representation of objects, physical actions instead of opaque syntax, rapid reversible operations with immediate feedback.
- **Precis:** Interfaces earn trust when the user can see the object of interest, act on it, and undo. Opacity and delayed feedback kill controllability. Shneiderman positions DM as a path to systems people can understand and dominate, not systems that dominate people.
- **Why xOS cites it:** Freeform agents that mutate state without a durable, inspectable capability object fail predictability. Capabilities and action logs are the DM-shaped answer in an agent world, not pure chat.
- **Does not buy:** That natural language is forbidden. NL is fine when it bottoms out in visible, reversible structure.
- **Read if:** Someone claims "agents replace UI so nothing needs to be inspectable."

### A4. Masson et al. 2024 -- DirectGPT

- **Key:** `massonDirectGPT2024`
- **Cite:** Damien Masson et al. "DirectGPT: A Direct Manipulation Interface to Interact with Large Language Models." CHI 2024. doi:[10.1145/3613904.3642462](https://doi.org/10.1145/3613904.3642462)
- **What it is:** Empirical / system paper on combining direct manipulation with LLM interaction rather than chat alone.
- **Precis:** Pure chat is a weak control surface for many editing and construction tasks. DirectGPT shows gains when users can select, point, and manipulate while still using language. The result is not "DM or LLM" but a hybrid: language for intent, DM for scope and precision.
- **Why xOS cites it:** Supports goal path + structural surfaces (modes, shared project context, selection) over chat-only OS fantasies. Does **not** prove our modal shell is optimal; it proves chat-only is incomplete.
- **Does not buy:** A finished OS UI kit.
- **Read if:** Designing the Develop mode surfaces next to the agent.

### A5. Abdul et al. 2018 -- explainable / accountable systems map

- **Key:** `abdulTrendsTrajectoriesExplainable2018`
- **Cite:** Ashraf Abdul et al. "Trends and Trajectories for Explainable, Accountable and Intelligible Systems." CHI 2018. doi:[10.1145/3173574.3174156](https://doi.org/10.1145/3173574.3174156)
- **What it is:** Large literature mapping exercise (hundreds of core papers, thousands of citing works) on explanations and intelligible systems across HCI, ML, and neighboring fields.
- **Precis:** Intelligibility and accountability are multi-community problems. Topic models and network analysis show which research clusters talk to each other and which are isolated. The HCI takeaway: powerful autonomous systems need intelligible interfaces designed in, not bolted on after deployment.
- **Why xOS cites it:** Action logs and visible agent identity are not "ops chrome"; they sit in a long accountability literature. Also a warning against isolated reinvented "explainability" features.
- **Does not buy:** A specific logging format or that logging alone yields trust.
- **Read if:** You need citations when someone calls status/stop "over-engineering."

### A6. Raskin 2000 -- The Humane Interface

- **Key:** `raskinHumaneInterface2000`
- **Cite:** Jef Raskin. *The Humane Interface: New Directions for Designing Interactive Systems.* Addison-Wesley, 2000. ISBN 0-201-37937-6.
- **What it is:** Book-length HCI program from the Macintosh / Canon Cat lineage: modelessness ideals, content persistence, quantification of interface costs, search and zooming ideas later reflected in Archy.
- **Precis:** Interfaces should minimize cognitive tax. Apps-as-worlds and modal traps (in Raskin's sense) cost attention. Persistent content and command-oriented work beat constant re-finding. Many concrete proposals (LEAP search, zooming) are historical; the durable claim is humane defaults and persistence over decoration.
- **Why xOS cites it:** Inspiration for goals/capabilities over app hunting, and for persistence of work products. Explicitly **not** a mandate to rebuild Archy or a full ZUI.
- **Does not buy:** Work-mode "modals" in our sense (task context chrome). Different use of "mode."
- **Read if:** UX lineage discussions; pair with Brown's short review.

### A7. Brown 2002 -- review of Humane Interface

- **Key:** `brownReviewHumaneInterface2002`
- **Cite:** Dan Brown. "Review of The humane interface." *ACM SIGCHI Bulletin* supplement, 2002. doi:[10.1145/967135.967153](https://doi.org/10.1145/967135.967153)
- **What it is:** Contemporary review of Raskin's book.
- **Precis:** Secondary pointer for readers who want a short entry before the full book. Use for orientation, not as a substitute for Raskin's arguments.
- **Why xOS cites it:** Lightweight citation when the book is too heavy for a footnote.
- **Read if:** Skimming only.

---

## Track B -- Systems: durable state and process C/R

### B1. Rodeh, Bacik, Mason 2013 -- Btrfs

- **Key:** `rodehBtrfs2013`
- **Cite:** Ohad Rodeh, Josef Bacik, Chris Mason. "BTRFS." *ACM Transactions on Storage* 9(3), 2013. doi:[10.1145/2501620.2501623](https://doi.org/10.1145/2501620.2501623)
- **What it is:** Authoritative design paper for Btrfs: COW, B-trees, snapshots/clones, scaling and integrity goals.
- **Precis:** Btrfs targets general-purpose Linux storage with copy-on-write, efficient snapshots and clones, checksums, and even performance as the filesystem ages across diverse disks and workloads. Snapshots are a first-class design product, not an afterthought. The paper walks core data structures and algorithms and the tradeoffs forced by COW + snapshots (e.g. defragmentation tension).
- **Why xOS cites it:** Preferred durability layer for agent homes/workspaces is not a vibe; COW snapshots match high agent write traffic and rollback needs. Preferred, not sacred -- product law allows equal alternatives.
- **Does not buy:** That Btrfs is always faster than XFS/ZFS for every laptop, or that snapshots replace process C/R.
- **Read if:** Arguing filesystem choice for agent data.

### B2. Rodeh 2008 -- B-trees, shadowing, and clones

- **Key:** `rodehBtreesShadowingClones2008`
- **Cite:** Ohad Rodeh. "B-trees, shadowing, and clones." *ACM Trans. Storage* 3(4), 2008. doi:[10.1145/1326542.1326544](https://doi.org/10.1145/1326542.1326544)
- **What it is:** Algorithms for B-trees under shadowing/COW with efficient cloning (writeable snapshots).
- **Precis:** Classical B-trees fight pure shadowing filesystems. Rodeh gives concurrency-friendly B-tree algorithms that respect COW and implement cheap clones. This is intellectual ancestry for Btrfs-style snapshot systems (WAFL/ZFS-class ideas included).
- **Why xOS cites it:** Explains *why* snapshot-friendly trees matter when many agent subvolumes fork and roll back.
- **Does not buy:** Kernel patch recipes for 2026 mainline.
- **Read if:** You need depth under B1.

### B3. Andrijauskas et al. 2024 -- CRIU for scientific / HTC jobs

- **Key:** `andrijauskasCriuCheckpointRestore2024`
- **Cite:** Fabio Andrijauskas et al. "CRIU - Checkpoint Restore in Userspace for computational simulations and scientific applications." *EPJ Web Conf.* 295:07046, 2024. doi:[10.1051/epjconf/202429507046](https://doi.org/10.1051/epjconf/202429507046) (also arXiv:2402.05244)
- **What it is:** Empirical feasibility study of CRIU in OSG OSPool / HTCondor-like scientific settings.
- **Precis:** Long jobs want preemption without waste. CRIU dumps process state to disk (files, some network cases, containers with caveats). Authors test serial codes, pthreads/forks, open files, containers, GPU, network, NFS/CVMFS, MPI. Results are mixed: simple serial and thread/fork cases work; GPU and MPI fail in their setup; container checkpointing depends on runtime integration; restore often wants matching CPU family and path layout. Conclusion: adequate for many HTC scenarios, not universal.
- **Why xOS cites it:** Grounds CRIU as real and limited. "Hit or miss" is not only folklore; the paper tables failure classes (GPU, MPI, some container paths). Supports dump-friendly agent trees and honest fail-soft.
- **Does not buy:** That CRIU is a free v1 demo checkbox, or that agent C/R is solved.
- **Read if:** Owning `xos-yl4d` (CRIU track).

### B4. Stoyanov et al. 2025 -- CRIUgpu

- **Key:** `stoyanovCriugpuTransparentCheckpointing2025`
- **Cite:** Radostin Stoyanov et al. "CRIUgpu: Transparent Checkpointing of GPU-Accelerated Workloads." arXiv:2502.16631, 2025. doi:[10.48550/arXiv.2502.16631](https://doi.org/10.48550/arXiv.2502.16631)
- **What it is:** Transparent GPU checkpointing via driver capabilities, integrated with CRIU-class workflows for CUDA and ROCm, multi-GPU DL/HPC evaluation.
- **Precis:** GPU C/R is hard (memory model, sync, dynamic parallelism). Prior intercept/log/replay of device APIs adds steady-state overhead and vendor-specific glue. CRIUgpu uses newer driver snapshot facilities, claims zero steady-state overhead and faster recovery than intercept-based schemes across multi-GPU workloads.
- **Why xOS cites it:** Local LLM / GPU agent work is later, but when dual-model local inference matters, GPU C/R is the literature, not handwaving. Pair with NVIDIA CUDA-checkpoint + CRIU blog (vendor primary URL).
- **Does not buy:** v1 local LLM on virtio-gpu or $180 laptops.
- **Read if:** Hardware profiles with real GPUs; later agent freeze on GPU workers.

### B5. Liétar et al. 2019 -- snmalloc

- **Key:** `lietarSnmallocMessagePassing2019`
- **Cite:** Paul Liétar et al. "snmalloc: a message passing allocator." ISMM 2019. doi:[10.1145/3315573.3329980](https://doi.org/10.1145/3315573.3329980)
- **What it is:** High-performance allocator for producer/consumer multi-threaded workloads; message-passing remote frees without locks; compact slab metadata.
- **Precis:** Many allocators hurt when free happens on a different thread than alloc. snmalloc batches remote frees back to the owning allocator via message passing and uses a bump/free-list structure with tiny per-slab metadata. Wins show up on producer/consumer benchmarks vs then-current allocators. Code: Microsoft snmalloc.
- **Why xOS cites it:** Real research backing "pluggable malloc" discussions. Team Slack numbers (compile % and Chromium crashes under glibc malloc swap) are **anecdotes**, not this paper's claims. Paper motivates careful allocator choice; app-matrix policy is operational hygiene.
- **Does not buy:** That snmalloc is safe as default system malloc for Chromium-class apps.
- **Read if:** Toolchain / base optimization track.

---

## Track C -- Specs and primaries (no journal DOI required)

### C1. CRIU project

- **Primary:** https://criu.org/ · https://github.com/checkpoint-restore/criu
- **Precis:** Userspace checkpoint/restore for Linux tasks: freeze a tree, image to disk, restore later. Uses kernel facilities (ptrace, namespaces, TCP repair, etc.). Integrated into container ecosystems; license GPLv2 (see tree). Project docs list what cannot be checkpointed and failure modes.
- **Why xOS cites it:** Canonical definition of the CRIU candidate stack. Papers evaluate; the project is the source of truth for flags and limits.
- **Read if:** Implementing dump/restore.

### C2. Model Context Protocol (MCP)

- **Primary:** https://modelcontextprotocol.io/specification/2025-11-25 (and `/docs`)
- **Precis:** Open protocol for connecting LLM apps to tools and context (resources, tools, prompts). Not peer-reviewed novelty for xOS; it is a wire format / ecosystem standard. Capabilities in product language may *use* MCP-shaped tools; they are not the same as OS capabilities (identity, ACLs, durable teach path).
- **Why xOS cites it:** Honest name for tool protocols instead of inventing "our unique tool bus" without acknowledging the standard.
- **Read if:** Wiring agent tools.

### C3. Cosmopolitan Libc / APE

- **Primary:** https://github.com/jart/cosmopolitan
- **Precis:** Libc and build system aimed at actually-portable executables across OSes. Strong for single-binary demos and bootstrap tools. Project materials do not position it as a drop-in system libc for a full Linux desktop + browser stack; team consensus matches that limit.
- **Why xOS cites it:** Optional portable helpers only. Not system libc product law.
- **Read if:** Bootstrap / recovery tools.

### C4. BusyBox

- **Primary:** https://www.busybox.net/
- **Precis:** Multicall binary: many common Unix utilities in one image. Standard pattern for small embedded and appliance userspace.
- **Why xOS cites it:** Multicall-leaning base is prior art, not invention.
- **Read if:** Base image construction.

### C5. llama.cpp

- **Primary:** https://github.com/ggerganov/llama.cpp
- **Precis:** Widely used local LLM runtime. Dual-model stack direction may call it or peers; not an xOS deliverable by itself.
- **Why xOS cites it:** Concrete example of local tier for MODEL_STACK exploration.
- **Read if:** Local inference experiments on real HW.

### C6. NVIDIA -- checkpointing CUDA with CRIU

- **Primary:** https://developer.nvidia.com/blog/checkpointing-cuda-applications-with-criu/
- **Precis:** Vendor walkthrough of CUDA checkpoint utilities cooperating with CRIU for process trees. Complementary to CRIUgpu paper; blog, not journal.
- **Why xOS cites it:** Practical GPU C/R entry point next to B4.

### C7. Team prior art -- linux-rg, hzArchiso

- **Primary:** https://github.com/HaoZeke/linux-rg · https://github.com/HaoZeke/hzArchiso
- **Precis:** Multi-profile kernel packaging and intentional Archiso-style install sets. Operational prior art for hardware-aware images, not peer-reviewed claims.
- **Why xOS cites it:** Profile discipline examples when exploration notes discuss metal.

---

## Suggested reading paths

| Goal | Order |
|------|--------|
| Argue dual path in a design review | A1 -> A2 -> A3 -> A4 |
| Argue durable agent data | B1 -> B2 |
| Own CRIU issue `xos-yl4d` | C1 -> B3 -> B4 -> C6 |
| Stop overclaiming novelty | [NOVELTY.md](./NOVELTY.md) after A2 and B3 |
| Allocator debates | B5 + team anecdote label in REFERENCES |

## What this list deliberately omits

- Unrelated CHI hits from bad free-text ookcite queries (avatars, random CSCW demos) -- not in curated bib.
- Full OS textbooks and every checkpointing system (DMTCP, BLCR, etc.) -- add when someone owns that comparison with DOIs.
- Marketing posts without technical content.

When you add a citation to design notes, add the key to `refs/xos-docs.bib`, the DOI to ookcite collection `xos-docs`, and a precis section here in the same PR.
