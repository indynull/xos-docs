# Capabilities

Parent: [../VISION.md](../VISION.md) · See: [AGENTS.md](./AGENTS.md) · [../product/SECURITY.md](../product/SECURITY.md)

## What one is

A **capability** is a saved way to use a system: internal service, vendor tool, repo, file format, API, website. Tool shape can follow MCP or anything similar.

```text
goal → do we have a capability?
         yes → run it → show result
         no  → build one with the user → check permissions → save → reuse later
```

**Good ones are:** predictable, limited in power, inspectable, versioned, turn-off-able, explained in plain words.

## What we care about

Connecting **real work systems** under clear permissions. Tiny demos can show the pattern; they are not the goal list.

## How work should feel

| Kind of work | Preferred path |
|--------------|----------------|
| Multi-step job | Mode + shared project context + several capabilities |
| Same painful tool every week | Wire once, then call by goal |
| Open-ended browsing | Normal browser is fine |

**Check:** less hunting? something saved for next time? right mode? would this still make sense on a hard multi-day job?

Risky actions need a log and a yes from the user ([../product/SECURITY.md](../product/SECURITY.md)).

v1 list: [../product/V1_SCOPE.md](../product/V1_SCOPE.md).
