# Changelog

## 2026-07-23 (drop EESSI; linkers/loaders + Cosmo libc + kernel CRIU)

- **Removed EESSI** as wrong layer for agent restore reliability.
- **Right layer:** linkers, loaders, libc identity ([product/LINKERS_LOADERS.md](../product/LINKERS_LOADERS.md)); Cosmopolitan as **libc alternative**.
- **CRIU:** first-class and **kernel-deep**—integrated with the kernel/profile we ship, not a random package bolt-on.
- Deleted `product/BUILD_DEPLOY.md`; rewired brief, vision, technical shape, v1, success, capabilities, principles, decision, governance, alternative tracks, README, checkpointing, hardware profiles.

## 2026-07-23 (profiles, first-class CRIU, EESSI-class stack — superseded)

- Hardware profiles + first-class CRIU; EESSI-class stack later replaced by linkers/loaders (see above).

## 2026-07-23 (team thread — systemd, btrfs, Archy/Enso, hardware)

- **Archy / Enso** as UX lineage (command nucleus, persistence)—not clones ([concepts/UX_LINEAGE.md](../concepts/UX_LINEAGE.md)).
- **systemd-class units** per agent + ACLs; durable agent memory/data paths.
- **btrfs** snapshots compose with **CRIU** (data vs process); both in product shape.
- **Hardware install/kernel profiles**; QEMU not product; agentic kernel patches speculative only.
- Updated checkpointing, technical shape, agents, security, v1, brief/vision, principles, decision, governance, success criteria, alternative tracks, README.

## 2026-07-23 (Rohit pass — CRIU checkpointing)

- **CRIU** (Checkpoint/Restore In Userspace; often “ciru”): supervisor-owned freeze/restore of agent trees; session snapshots; warm start; hung-agent dump.
- New [concepts/CHECKPOINTING.md](../concepts/CHECKPOINTING.md); wired into technical shape, agents, security, v1, brief, vision, decision, governance, alternative tracks, README.
- v1: optional dump/restore of one simple agent tree; not full DE C/R or live migration as the demo.

## 2026-07-23 (Rohit pass — multicall / Cosmopolitan base)

- **Base architecture:** think larger than “trimmed distro”—Linux + BusyBox-class multicall core, optional Cosmopolitan/APE portable tools for capabilities/bootstrap, heavier tools as layers.
- Edge *product* stays out of v1; multicall technique is in-scope as the base strategy.
- Cosmopolitan is not the whole desktop; busybox-only with no goal path is not enough.
- Updated technical shape, brief, vision, principles, non-goals, v1, decision, governance, problem, alternative tracks, README.

## 2026-07-23 (Rohit pass — shell, supervisor, isolation, optimizations)

- **Direction:** real goals over QEMU-first demos; base optimizations as product shape, not marketing “minimal.”
- **Desktop:** Wayland + modal focus; many windows allowed, not default.
- **Shell:** first-class CLI + plain-language path into the same agent stack.
- **Agents:** supervisord-style lifecycle; inter-agent channels controlled; each agent own Unix user + ACLs.
- Updated brief, one-pager, principles, non-goals, agents, work modes, technical shape, security, v1 scope, success criteria, governance, decision, problem, alternative tracks, README.

## 2026-07-23

- Plain language pass: less marketing wording across all docs.
- Minimal set: brief → one-pager → details. Demo and extra notes removed earlier.
