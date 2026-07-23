# Governance

Parent: [../VISION.md](../VISION.md) · See: [DECISION.md](./DECISION.md)

## Aim

One clear product. Change the idea in the brief/one-pager first. People rotate; roles and docs stay.

## Who decides the product

A small group owns: what is in/out of the idea, v1 cuts, core vs other project. Engineering can spread out; the product idea cannot drift in chat only. Prefer a few area leads over a large committee (Debian-shaped process is an anti-pattern for this project).

## Roles (not names)

| Role | Owns |
|------|------|
| Product | Brief, one-pager, modes, capabilities UX, non-goals, v1 bar |
| Base system | Thin optimized Linux, images, package cuts, toolchain/kernel knobs that earn keep |
| Shell / desktop | Wayland modal UI, CLI + plain-language goal entry |
| Agent runtime | Supervisor, lifecycle, inter-agent channels, tool loop |
| Security | Policy, per-agent users/ACLs, capability classes |
| Infra | Build, CI smoke (QEMU OK), real-hardware artifacts when available |

If product has no owner, pause idea-changing work.

## Rules

1. Idea conflict → edit [../BRIEF.md](../BRIEF.md) / [../VISION.md](../VISION.md) first.  
2. New product idea → update non-goals or [../context/ALTERNATIVE_TRACKS.md](../context/ALTERNATIVE_TRACKS.md).  
3. “While we’re at it” → no for v1 by default.  
4. Security exceptions → write in [SECURITY.md](./SECURITY.md).  
5. QEMU/CI convenience must not rewrite the product story.
