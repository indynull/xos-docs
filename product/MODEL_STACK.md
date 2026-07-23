# Model stack (local + remote)

Parent: [../VISION.md](../VISION.md) · See: [WEDGE.md](./WEDGE.md) · [../concepts/AGENTS.md](../concepts/AGENTS.md)

## Problem

Shelling out to a chat CLI is **not enough for an OS**. Latency, cost, offline, and policy need a **system-level** path.

Most of the public web is also **hostile to agents** (bot walls, monetized search, anti-scrape). That is a separate, expensive problem from “show structured results for agent-friendly sources.”

## Dual model stack

| Tier | Role | Notes |
|------|------|--------|
| **Remote (e.g. Grok)** | Hard multi-step goals, writing, planning | Needs real OS integration and **budget**, not only `grok` in a terminal |
| **Local (e.g. llama.cpp + open model)** | Offline, cheap, private, narrow tasks | Needs real GPU/CPU; **virtio-gpu is weak** for serious local LLM |

Routing lives in the **supervisor / OS harness**, not in each app reinventing it.

## Cost and budgets

If the agent interface is OS-level, **budgets can be enforced** (per agent, per mode, per day). That is part of the wedge.

| Concern | Direction |
|---------|-----------|
| API cost | Caps, confirm on expensive steps, prefer local for bulk/log skimming when quality allows |
| Offline | Local tier must degrade honestly (“offline; local-only skills”) |
| Logs at scale | Ingesting rsyslog-scale streams needs **local** summarization or heavy filtering before remote—do not ship raw firehoses to the API by default |

## Hardware vs QEMU

| Target | Fit |
|--------|-----|
| **QEMU first** | Bring-up, CI, agent harness without GPU—**agreed team default for early work** |
| **Real hardware** | Do not ignore; required for meaningful **local LLM**, drivers, and product demos that claim performance |
| **$180 laptop persona** | Instant tools + remote model; local LLM optional/later or tiny models only |
| **Strong local HW** | Local inference experiments (e.g. advanced local stacks) allowed as profile extras—not v1 requirement |

QEMU-first **bring-up** is fine. “Product story is only a QEMU screenshot” is still a fail.

## Web middle path (not elinks, not full Chrome)

| Approach | Use |
|----------|-----|
| **Structured / agent-friendly sources** | Prefer APIs, feeds, plain or structured extracts—no full HTML chrome |
| **Embedded engine when needed** | Escape hatch for real sites |
| **Full browser** | App layer; progressive |
| **Puppeteer + VPN + residential IP rotation** | Hostile-web scraping fleet—**out of v1 wedge**; legal/ethical/cost landmine; not required for the harness demo |

Most interactions need **facts**, not a full browser. Most of the *modern web* will not cooperate without heavy automation—do not make “beat Cloudflare for everyone” the product.

## Network stack

| Now | Later |
|-----|--------|
| Ship **NetworkManager** and/or **iwd** (NM easier for Bluetooth; iwd more “unixy”) | Replace when it earns keep—**not first priority** |

## v1 expectation

- Document **how** the agent talks to models (even if v1 is “remote only + stub local”).  
- Prefer QEMU for early harness bring-up; schedule at least one **real HW** path for CRIU/local experiments.  
- Web: structured sources + escape hatch; no residential-IP farm.  
- Cost: at least a simple budget/confirm story for remote calls.
