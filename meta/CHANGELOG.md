# Changelog

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
