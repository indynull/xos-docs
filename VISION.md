# xOS — one-pager

Working name: **xOS**. Design only; not built yet.

**Read order:** [BRIEF.md](./BRIEF.md) → this page → [README.md](./README.md) for the rest.

---

## Core idea

```text
goal  →  skill (capability)  →  result
```

…in a **work mode** that matches what you are doing (for example: develop, research, ops).

| Term | Plain meaning |
|------|----------------|
| **Goal** | What you want done—not which app to open |
| **Capability** | A saved, reusable way to talk to a system (API, tool, repo, file format, site). MCP-style tools are fine |
| **Work mode** | The desktop focuses on one kind of work at a time. You always see which mode you are in |
| **Shell** | Normal CLI plus plain-language goals into the agent stack; not a separate product bolted on later |
| **Agent** | Built into the OS: look things up, take actions, and help create new capabilities when missing. Not one privileged chat process |
| **Agent supervisor** | systemd-class units + policy: start/stop agents, isolation, restarts, inter-agent channels. Not ad-hoc background jobs |
| **Checkpointing** | **CRIU first-class** (live process) + **btrfs** snapshots (data)—compose; tested restores, not hit-or-miss |
| **Agent identity** | Each agent: Unix user + unit + ACLs + durable data paths. Rights are granted, not inherited wholesale from the human |
| **Desktop surface** | Wayland + modal UI: work mode and agent interaction at the center; many windows possible, not the default. Lineage: Archy/Enso-style command focus ([concepts/UX_LINEAGE.md](./concepts/UX_LINEAGE.md)) |
| **Base system** | Linux + **btrfs** + **small multicall core**, optional Cosmopolitan/APE tools, **hardware profiles**, real optimizations. Not “Ubuntu minus packages.” |
| **Software stack** | EESSI-class **pinned optimized** tools—same revision on desk, CI, and agents ([product/BUILD_DEPLOY.md](./product/BUILD_DEPLOY.md)) |
| **Fallback** | Full browser or normal app when the short path does not fit |

**Reuse:** the first time you connect a painful system costs effort and review. Later times should be a short goal, not the same UI maze.

**Proof target:** real machines and real workloads. QEMU is a CI convenience at most—not the product goal and not a substitute for optimization.

---

## Who and why

For people whose work spans many tools and systems—not for theme hobbyists.

**Problem:** desktops are organized around apps. You become the glue. A chat widget on a normal desktop does not fix that. Most Linux desktops also ship a lot of junk.

**Aim:** a computer that shortens the path from goal to a result you can trust, for serious work (complex engineering, ops, research, and similar). Small examples can show the idea; they are not the product.

---

## What “good” looks like

| Bad default | Better |
|-------------|--------|
| Many windows and tabs | One clear mode; shared project context; windows optional |
| Re-learning the same internal site | Saved capability; ask once next time |
| Huge default install | Multicall core + explicit layers; **fast**, understandable system |
| Agent as magic chat with your rights | Unit + user + ACLs; log and stop; CRIU + btrfs when useful |
| “Boots in QEMU” as the story | Real hardware profiles; measure the base |
| Hit-or-miss agent restore | First-class CRIU + pinned stack + dump-friendly agents |
| Random tools per machine | Shared stack revision (EESSI-class) |
| Pretty distro as the story | Getting real work done |

---

## Not goals

Wallpaper/theme culture as the product · app store as the core · “AI on a normal desktop” as the end state · toy demos as the identity · multi-arch distro project as v1 · agent with silent root power · shipping default junk “just in case” · QEMU-first as the product narrative · one shared agent process with the user’s full rights.

Full list: [principles/NON_GOALS.md](./principles/NON_GOALS.md).

---

## Rules (short)

1. Work first, decoration last  
2. Goals over app-hunting  
3. Saved capabilities over one-off clicking  
4. Save what you build  
5. Security matters (log, stop, ask before dangerous steps)  
6. Stay small and clear  
7. Thin multicall base (BusyBox-class), optimized; portable tools where they pay  
8. Be honest about v1 scope  
9. Agent actions must be visible  
10. Work mode always shown  
11. Agents are supervised and isolated (own user + unit + ACLs + data paths)  
12. Shell is native (CLI + plain language), not an afterthought  
13. Durable agent state on disk (btrfs); **first-class CRIU** with tested restores  
14. Hardware-aware base; QEMU is not the product  
15. Pinned software stack (EESSI-class); same revision on desk and CI  

→ [principles/PRINCIPLES.md](./principles/PRINCIPLES.md)

---

## Version 1

Show the idea works on **real tasks**, not toys.

Bootable image aimed at **real hardware** (CI may use QEMU; that is not the goal): Wayland modal shell, goal → supervised agent → capability → mode, for a few paths, including:

- real develop session (edit + terminal + agent; plain-language shell path works)  
- real investigation or write-up with sources  
- create one non-trivial capability and use it again  
- at least two agents under the supervisor with distinct users/ACLs  
- CRIU dump → restore of the canonical agent path on a real-hardware profile  
- image and CI share a pinned software stack revision  

**Fail if:** pretty ISO + chat on a normal desktop, only gimmick demos, “it boots in QEMU” with no real-work path, or agent restore is untested roulette.

→ [product/V1_SCOPE.md](./product/V1_SCOPE.md) · [product/SUCCESS_CRITERIA.md](./product/SUCCESS_CRITERIA.md)

---

## Team decision

One product: goal-first desktop (Archy/Enso-informed), capabilities, supervised agents (user + unit + ACLs + btrfs data), **first-class CRIU**, Wayland modal shell, hardware profiles, EESSI-class pinned stack. Prove it before calling it an OS. Other ideas (stores, mobile-as-product, pure distro polish, QEMU demos as the product) stay separate.

→ [product/DECISION.md](./product/DECISION.md)
