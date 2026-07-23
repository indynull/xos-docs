# UX lineage (optional inspiration)

Parent: [../VISION.md](../VISION.md) · Grounding: [../context/REFERENCES.md](../context/REFERENCES.md)

> **Optional — not required reading, not product identity.**  
> Prior art only. Do not treat as a formal standard or as xOS IP claims.

## Raskin / Archy

Primary: Jef Raskin, *The Humane Interface: New Directions for Designing Interactive Systems*, Addison-Wesley, 2000, ISBN **0-201-37937-6**.

Secondary: D. Brown, “Review of The humane interface,” *ACM SIGCHI Bulletin* supplement, 2002, doi:[10.1145/967135.967153](https://doi.org/10.1145/967135.967153).

Historical software lineage (Archy / Raskin Center) is summarized in secondary encyclopedic sources; use the **book** as the design source of record for:

- content persistence  
- commands / nucleus rather than app-as-unit  
- navigation and humane defaults  

**Map (loose, not a clone):**

| Raskin-class idea | xOS product shape (hypothesis) |
|-------------------|--------------------------------|
| Commands over app hunting | Goals + **capabilities** |
| Persistence | Durable workspace + agent data (btrfs *preferred*; FS paper: doi:10.1145/2501620.2501623) |
| Fast command invocation | Dual path: instant tools + goal entry |

We are **not** claiming a full ZUI or Archy reimplementation.

## Enso / Humanized

Humanized Enso (historical): lightweight command invocation over a desktop session. Treat as HCI prior art (demos / product history), not a peer-reviewed protocol. Same dual-path lesson: type a command, act, return—without making the whole machine a classic app launcher.

## Isolation / capabilities language

Unix users, ACLs, and process isolation are standard OS mechanisms—not novel. Academic “capability systems” literature is broader; do not confabulate a specific paper without a DOI from [REFERENCES.md](../context/REFERENCES.md).
