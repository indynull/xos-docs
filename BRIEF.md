# xOS — brief

Share this first. More detail: [VISION.md](./VISION.md), then [README.md](./README.md).

---

xOS is a Linux desktop built around **what you are trying to do**, not around apps and browser tabs. Today, real work means jumping between editors, tickets, logs, docs, internal tools, and websites, re-finding context each time. xOS starts from your goal. If the system already knows how to do that kind of thing, it does it and shows the result in a focused workspace. If it does not, an agent helps you wire it up (with clear permissions), saves that skill, and next time you do not repeat the click path.

The shell is first-class: a **fast normal path** (CLI, terminal, launcher-class actions with no model wait) **and** a plain-language goal path into the same supervised agent stack. When a pattern is clear, it is **saved as a sandboxed capability**—not freeform inventing every time. The desktop focuses on a **work mode** (task context always visible), with little chrome—not a ricing product and not “just another tiling DE.” Agents are not one chat process with your full rights: each has its own **identity** (user + unit-class isolation + ACLs) and **durable data** on disk (btrfs preferred for homes/workspaces/snapshots). Soft stop and lifecycle are supervised; **checkpoint/snapshot of agent state is product intent** (see tech notes for candidate stacks such as CRIU). The base stays **small and multicall-leaning**—build up, do not ship a consumer distro and delete packages. The product wedge is an **OS-level agent harness**, not “shell out to a chat CLI on Ubuntu.”

This is aimed at people who do hard jobs on computers all day. Version 1 proves the harness on a few real tasks: coding with editor and terminal, a real investigation or write-up, and create-or-teach one capability then reuse it—with instant tools always available. Pretty Linux plus a chat box is not enough. When an agent acts, you see what it is doing, can stop it, and approve anything risky.
