# Capabilities

Parent: [../VISION.md](../VISION.md) · See: [AGENTS.md](./AGENTS.md) · [../product/SECURITY.md](../product/SECURITY.md)

## What one is

A **capability** is a saved way to use a system: internal service, vendor tool, repo, file format, API, website. Tool shape can follow MCP or anything similar.

```text
goal → do we have a capability?
         yes → run it → show result
         no  → build one with the user → check permissions → save → reuse later
```

**Teach path (preferred when freeform agents are slow or creative):** the human does the job the **normal/instant** way until the pattern is clear, then that pattern is **persisted** as a capability under sandbox limits—so the next goal hits a fixed path, not an improvising model.

**Good ones are:** predictable, limited in power, inspectable, versioned, turn-off-able, explained in plain words, sandboxed so they do not “get creative.”

## What we care about

Connecting **real work systems** under clear permissions. Tiny demos can show the pattern; they are not the goal list.

## Platform capabilities (OS-provided)

Some capabilities are **built into the OS**, not only user-wired:

| Platform capability | Role |
|---------------------|------|
| **btrfs snapshot / send** (preferred) | Durable agent/workspace points |
| **Process checkpoint** (direction) | Freeze/resume agent trees when committed—CRIU is a candidate ([CHECKPOINTING.md](./CHECKPOINTING.md)) |

User-built capabilities should **declare** permissions and runtime needs so restore and reuse stay reliable.

## How work should feel

| Kind of work | Preferred path |
|--------------|----------------|
| Multi-step job | Mode + shared project context + several capabilities |
| Same painful tool every week | Wire once (or teach path), then call by goal |
| Need it *now* (no model lag) | Instant normal tools (terminal, launcher, editor)—always available |
| Open-ended browsing | Normal browser is fine |
| Enterprise auth / mail / flaky SaaS | Capability with explicit auth story—not freeform agent hope |

**Check:** less hunting? something saved for next time? right mode? would this still make sense on a hard multi-day job?

Risky actions need a log and a yes from the user ([../product/SECURITY.md](../product/SECURITY.md)).

v1 list: [../product/V1_SCOPE.md](../product/V1_SCOPE.md).
