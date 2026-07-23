# Changelog

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
