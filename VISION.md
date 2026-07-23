# xOS — one-pager

Working name: **xOS**. Design only; not built yet.

**Read order:** [BRIEF.md](./BRIEF.md) → this page → [README.md](./README.md) for the rest.  
**Tech stack bets** (CRIU timing, display server, linker/libc, hardware profiles) live under [product/](./product/) and [concepts/](./concepts/) as **exploration**, not product law—unless repeated here.  
**Grounding:** [context/READING.md](./context/READING.md) (guided precis) · [context/REFERENCES.md](./context/REFERENCES.md) · [refs/xos-docs.bib](./refs/xos-docs.bib) · [context/NOVELTY.md](./context/NOVELTY.md).

---

## Core idea

```text
goal  →  skill (capability)  →  result
```

…in a **work mode** that matches what you are doing (for example: develop, research, ops).

| Term | Plain meaning |
|------|----------------|
| **Goal** | What you want done—not which app to open |
| **Capability** | A saved, reusable, sandboxed way to talk to a system; teach-from-normal-use is first-class |
| **Work mode** | Desktop focuses on one kind of work; mode always visible |
| **Shell** | Instant normal CLI/tools **and** plain-language goals on the same supervised stack |
| **Agent** | Part of the OS: read, act, help build capabilities. Not one privileged chat process |
| **Agent supervisor** | Lifecycle, policy, channels—not ad-hoc background jobs |
| **Agent identity** | Per agent: own user + unit-class isolation + ACLs + durable data paths |
| **Durability** | Agent state on disk; **btrfs** preferred for homes/workspaces/snapshots |
| **Checkpointing (intent)** | Freeze/resume agent process state when possible; compose with filesystem snapshots. Candidate tech (e.g. CRIU) is direction—not a locked v1 demo gate |
| **Base system** | Thin, multicall-leaning Linux; not “consumer distro minus packages” |
| **Fallback** | Full browser or normal app when the short path does not fit |

**Reuse:** first connection costs review; later times are a short goal.

**Bring-up:** QEMU-first is fine for the harness. Real hardware matters for drivers and local models later—not a spine law that blocks the story.

---

## Who and why

For people whose work spans many tools—not theme hobbyists.

**Problem:** desktops are app- and tab-centric; you are the glue. Chat on a normal desktop does not fix that. Default Linux installs are bloated.

**Aim:** shorter path from goal to a trusted result, with agents that are supervised and limited, and instant tools that never wait on a model.

---

## What “good” looks like

| Bad default | Better |
|-------------|--------|
| Many windows and tabs | One clear mode; shared project context |
| Re-learning the same internal site | Saved capability; ask once next time |
| Huge default install | Small multicall-leaning system |
| Agent as magic chat with your rights | Per-agent identity, log, stop |
| Agent-only UI (wait for “thinking”) | Instant normal tools always |
| Full default DE chrome / ricing | Strip what agents + config can own |
| Shell-out chat as the “OS” | OS-level harness + capabilities |
| Pretty distro as the story | Getting real work done |

---

## Not goals

Wallpaper/ricing product · app store as core · chat on stock desktop as end state · toy demos as identity · agent with silent full user rights · default junk “just in case” · hostile-web scrape farm as v1 · boiling every subsystem at once.

Full list: [principles/NON_GOALS.md](./principles/NON_GOALS.md).

---

## Rules (product law)

1. Work first, decoration last  
2. Goals over app-hunting  
3. Saved capabilities over one-off clicking  
4. Save what you build (teach → sandbox)  
5. Security matters (log, stop, ask before dangerous steps)  
6. Stay small and clear  
7. Thin multicall-leaning base  
8. Honest v1: harness on real tasks, not every layer rewritten  
9. Agent actions must be visible  
10. Work mode always shown  
11. Agents supervised and isolated (own identity + ACLs; not the human by default)  
12. Shell native: CLI + plain language; **dual path** (instant + agent)  
13. Durable agent data on disk (btrfs preferred)  
14. Wedge is **OS agent harness**—not boiling every layer ([product/WEDGE.md](./product/WEDGE.md))  
15. Lean default chrome; progressive app replacement  

**Direction (not numbered law):** checkpoint/restore of agents (e.g. CRIU when we commit), dual model stack + budgets, display server choice, hardware profiles, runtime ABI details—see exploration notes under product/.

→ [principles/PRINCIPLES.md](./principles/PRINCIPLES.md)

---

## Version 1

Show the **harness** works on **real tasks**, not toys.

Bootable image (QEMU OK): goal → supervised agent → capability → work mode, including:

- real develop session (edit + terminal + agent; plain-language path works)  
- real investigation or write-up with sources  
- create or teach one capability and use it again  
- instant normal path without waiting on a model  
- at least two agents with **distinct identities**  
- durable agent/workspace data (btrfs when available)  

**Fail if:** pretty ISO + chat on a normal desktop · only gimmick demos · agent-only UI with no instant tools · “another compositor” with no harness · no wedge beyond shelling out to a chat CLI.

**Not v1 fail conditions:** CRIU dump→restore demo, documented hardware profile matrix, linker/libc policy document, specific display server brand.

→ [product/V1_SCOPE.md](./product/V1_SCOPE.md) · [product/SUCCESS_CRITERIA.md](./product/SUCCESS_CRITERIA.md)

---

## Team decision

One product: **OS-level agent harness** + goal-first surface + lean base: dual path, capabilities (teach→sandbox), supervised multi-agent identity, durable agent data. Prove the harness before boiling the ocean. Stack details are candidates until decided.

→ [product/DECISION.md](./product/DECISION.md)
