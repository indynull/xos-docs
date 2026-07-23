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
| **Agent** | Built into the OS: look things up, take actions, and help create new capabilities when missing |
| **Base system** | Thin Linux. Real tools underneath (editor, terminal, browser engine). Cut unused packages |
| **Fallback** | Full browser or normal app when the short path does not fit |

**Reuse:** the first time you connect a painful system costs effort and review. Later times should be a short goal, not the same UI maze.

---

## Who and why

For people whose work spans many tools and systems—not for theme hobbyists.

**Problem:** desktops are organized around apps. You become the glue. A chat widget on a normal desktop does not fix that. Most Linux desktops also ship a lot of junk.

**Aim:** a computer that shortens the path from goal to a result you can trust, for serious work (complex engineering, ops, research, and similar). Small examples can show the idea; they are not the product.

---

## What “good” looks like

| Bad default | Better |
|-------------|--------|
| Many windows and tabs | One clear mode; shared project context |
| Re-learning the same internal site | Saved capability; ask once next time |
| Huge default install | Small, understandable system |
| Agent as magic chat | Agent with tools, log, and stop |
| Pretty distro as the story | Getting real work done |

---

## Not goals

Wallpaper/theme culture as the product · app store as the core · “AI on a normal desktop” as the end state · toy demos as the identity · multi-arch distro project as v1 · agent with silent root power · shipping default junk “just in case.”

Full list: [principles/NON_GOALS.md](./principles/NON_GOALS.md).

---

## Rules (short)

1. Work first, decoration last  
2. Goals over app-hunting  
3. Saved capabilities over one-off clicking  
4. Save what you build  
5. Security matters (log, stop, ask before dangerous steps)  
6. Stay small and clear  
7. Thin, solid Linux base  
8. Be honest about v1 scope  
9. Agent actions must be visible  
10. Work mode always shown  

→ [principles/PRINCIPLES.md](./principles/PRINCIPLES.md)

---

## Version 1

Show the idea works on **real tasks**, not toys.

Bootable image (QEMU first): goal → agent → capability → mode for a few paths, including:

- real develop session (edit + terminal + agent)  
- real investigation or write-up with sources  
- create one non-trivial capability and use it again  

**Fail if:** pretty ISO + chat on a normal desktop, or only gimmick demos.

→ [product/V1_SCOPE.md](./product/V1_SCOPE.md) · [product/SUCCESS_CRITERIA.md](./product/SUCCESS_CRITERIA.md)

---

## Team decision

One product: goal-first desktop, capabilities, OS agent, thin Linux. Prove it before calling it an OS. Other ideas (stores, mobile, pure distro polish) stay separate.

→ [product/DECISION.md](./product/DECISION.md)
