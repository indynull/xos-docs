# xOS — brief

Share this first. More detail: [VISION.md](./VISION.md), then [README.md](./README.md).

---

xOS is a Linux desktop built around **what you are trying to do**, not around apps and browser tabs. Today, real work means jumping between editors, tickets, logs, docs, internal tools, and websites, re-finding context each time. xOS starts from your goal. If the system already knows how to do that kind of thing, it does it and shows the result in a focused workspace. If it does not, an agent helps you wire it up (with clear permissions), saves that skill, and next time you do not repeat the click path.

The shell is first-class: a normal command line and a plain-language path into the same agent stack. The desktop is Wayland-native with a **modal** focus (work mode + agent chat as the center, not window soup). Agents are not one chat process with your full rights: a supervisor starts and stops them, they talk through controlled channels, and each agent runs as its own user with ACLs. Soft stop can **checkpoint** an agent tree with CRIU (freeze/restore), not only kill it. The base is radical and small when it can be: Linux plus a **BusyBox-class multicall core** (not a consumer distro with packages deleted), optional **Cosmopolitan/APE-style portable tools** for capabilities and bootstrap, and real optimizations for hard workloads—not a stock distro plus a chat box, and not “boots in QEMU” as the product story.

This is aimed at people who do hard jobs on computers all day: engineering, ops, research, design, writing. Version 1 will not cover every workflow. It only needs to prove the idea on a few real tasks: actual coding with editor and terminal, a real investigation or write-up with sources, and adding one non-trivial skill then using it again. A pretty Linux image with a chat box and a few toy demos is not enough. When an agent acts, you should see what it is doing, stop it, and approve anything risky.
