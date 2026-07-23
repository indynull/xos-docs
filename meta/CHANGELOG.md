# Changelog

## 2026-07-23 (HCI research, ookcite collection, bib export, voice)

- Ookcite collection **`xos-docs`**; curated export [refs/xos-docs.bib](../refs/xos-docs.bib).
- HCI notes grounded in Amershi, Horvitz, Shneiderman, Raskin, DirectGPT, explainability survey ([context/HCI.md](../context/HCI.md)).
- BRIEF rewritten denser (mechanism-first, cite keys); novelty/refs point at collection + bib.
- proseguard `prose-human` run on spine prose (see gate log in commit if present).

## 2026-07-23 (ground references + honest novelty)

- Add [context/REFERENCES.md](../context/REFERENCES.md): ookcite-validated DOIs (CRIU, CRIUgpu, Btrfs, snmalloc, Humane Interface review) + primary URLs (criu.org, MCP spec, Cosmopolitan, BusyBox, llama.cpp, team kernel/archiso).
- Add [context/NOVELTY.md](../context/NOVELTY.md): prior art table; product hypothesis only where composition might differ; ban marketing ungrounded novelty.
- Label snmalloc/Chromium “15%” as **team anecdote**, not literature.
- Wire REFERENCES/NOVELTY into README, VISION, CHECKPOINTING, UX_LINEAGE, LINKERS_LOADERS, TECHNICAL_SHAPE, CAPABILITIES.

## 2026-07-23 (design review: demote tech locks from product law)

- Response to PR design review (changes requested): keep product spine; demote implementation bets.
- **Law stays:** OS harness, dual path, teach→capability, supervised multi-agent identity, durable agent data/btrfs preferred, multicall-leaning base, wedge, not just DE.
- **Demoted from BRIEF/VISION/principles/v1 pass-fail:** CRIU dump→restore gate, hardware profile machinery, linker/loader/libc as law, Wayland/modal as locked, Archy/Enso as identity.
- Tech files (`CHECKPOINTING`, `LINKERS_LOADERS`, `HARDWARE_PROFILES`, `MODEL_STACK`, heavy `TECHNICAL_SHAPE`) labeled **exploration / candidate**.
- `UX_LINEAGE` optional, off main read path.

## 2026-07-23 (wedge, dual model, QEMU-first bring-up)

- **Wedge:** OS-level agent harness is the “can’t do without us” product ([product/WEDGE.md](../product/WEDGE.md)); defer boiling every layer.
- **Dual model stack:** remote + local; OS budgets; virtio-gpu weak for local LLM ([product/MODEL_STACK.md](../product/MODEL_STACK.md)).
- **QEMU-first** bring-up agreed; real HW still required for local LLM/drivers.
- Web middle path (structured sources); hostile-web farm out of v1; NM/iwd now, replace later.
- Addresses “biting off too much / no solid unique product” risk.

## 2026-07-23 (dual path, teach→capability, lean chrome)

- **Not a turn against OS-adjacent work:** DE surface is big; base still optimized (`OS_VS_DE.md`).
- **Dual path:** instant normal tools always; agent for multi-step; model lag is first-class.
- **Teach → sandboxed capability:** operator does it normally until pattern is clear, then persist.
- Progressive app replacement; strip ricing/admin chrome by default.

## 2026-07-23 (stay near libc; OS vs DE; malloc caution)

- **v1:** use existing system libc; alternate libc / Rust+shim is **future** (browser-on-new-libc is proposal-sized).
- Apps depend on libc **implementation details and bugs**; pluggable malloc (e.g. snmalloc) needs app-matrix gates (Chromium-class crash class).
- **Not just a Sway/DE fork:** [product/OS_VS_DE.md](../product/OS_VS_DE.md)—goal-first surface + OS-adjacent base; browser is app layer.
- Ali/Bernhard consensus: stay closer to libc when departure is not necessary.

## 2026-07-23 (Cosmo not system libc; formats/debug; wipe site-stack slogans)

- Cosmopolitan: optional portable single-binary only—**not** system libc (team consensus).
- Runtime ABI expanded: executable/lib **formats**, linkers, loaders, **debug symbols/build-id**; speculative Rust libc noted as non-v1.
- CRIU remains first-class + kernel-deep ([criu.org](https://criu.org/)).
- Removed all remaining site-module-stack product framing from live docs.

## 2026-07-23 (linkers/loaders + kernel-deep CRIU)

- Right layer for hit-or-miss agent restore: linkers/loaders/runtime identity ([product/LINKERS_LOADERS.md](../product/LINKERS_LOADERS.md)).
- CRIU first-class and integrated with the kernel/profile we ship.
- Dropped multi-site package-stack product shape for restore reliability.

## 2026-07-23 (profiles, first-class CRIU)

- Hardware profiles + first-class CRIU; later refined by linkers/loaders pass.

## 2026-07-23 (team thread — systemd, btrfs, Archy/Enso, hardware)

- **Archy / Enso** as UX lineage (command nucleus, persistence)—not clones ([concepts/UX_LINEAGE.md](../concepts/UX_LINEAGE.md)).
- **systemd-class units** per agent + ACLs; durable agent memory/data paths.
- **btrfs** snapshots compose with **CRIU** (data vs process); both in product shape.
- **Hardware install/kernel profiles**; QEMU not product; agentic kernel patches speculative only.
- Updated checkpointing, technical shape, agents, security, v1, brief/vision, principles, decision, governance, success criteria, alternative tracks, README.

## 2026-07-23 (Rohit pass — CRIU checkpointing)

- **CRIU** (Checkpoint/Restore In Userspace; often “ciru”): supervisor-owned freeze/restore of agent trees; session snapshots; warm start; hung-agent dump.
- New [concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md); wired into technical shape, agents, security, v1, brief, vision, decision, governance, alternative tracks, README.

## 2026-07-23 (Rohit pass — multicall base)

- Base architecture: Linux + BusyBox-class multicall core; heavier tools as layers.
- Edge *product* stays out of v1; multicall technique is in-scope as the base strategy.

## 2026-07-23 (Rohit pass — shell, supervisor, isolation, optimizations)

- **Direction:** real goals over QEMU-first demos; base optimizations as product shape, not marketing “minimal.”
- **Desktop:** Wayland + modal focus; many windows allowed, not default.
- **Shell:** first-class CLI + plain-language path into the same agent stack.
- **Agents:** supervisord-style lifecycle; inter-agent channels controlled; each agent own Unix user + ACLs.
- Updated brief, one-pager, principles, non-goals, agents, work modes, technical shape, security, v1 scope, success criteria, governance, decision, problem, alternative tracks, README.

## 2026-07-23

- Plain language pass: less marketing wording across all docs.
- Minimal set: brief → one-pager → details. Demo and extra notes removed earlier.
