# xOS -- brief

Share this first. Detail: [VISION.md](./VISION.md), then [README.md](./README.md). Grounding: [context/REFERENCES.md](./context/REFERENCES.md), [refs/xos-docs.bib](./refs/xos-docs.bib).

---

Most "agent desktops" still leave you as the glue between apps, tabs, and a chat box. xOS inverts that: start from the **goal**, run a **capability** if one exists, otherwise build one under permissions and keep it. Next time the path is short.

Two ways in, always. The **instant path** is a normal CLI and launcher-class actions with no model wait -- mixed-initiative work already assumes the user keeps initiative when automation is the wrong tool [horvitzPrinciplesMixedInitiative1999; amershiGuidelinesHumanAIInteraction2019]. The **goal path** is plain language into the same supervised stack. When a pattern is clear from normal use, freeze it as a **sandboxed capability** (inspectable, stoppable), not a freestyle agent every time. Controllability is the point, not vibes [shneidermanDirectManipulation1997].

Agents are not one process with your full rights. Each agent gets an identity (user + unit-class isolation + ACLs) and **durable data** on disk (btrfs preferred for homes and snapshots [rodehBtrfs2013]). Process freeze/resume is **intent** with CRIU as a candidate stack [andrijauskasCriuCheckpointRestore2024; criu.org], not a v1 demo gate. The base stays multicall-leaning: build up, do not delete a consumer distro into thinness.

This is not another tiling DE and not "shell out to Grok on Ubuntu." The product wedge is an **OS-level agent harness** -- identities, capabilities, dual path, durable state -- which you do not get from an app harness alone. Stack details (display server brand, hardware profile machinery, linker policy) live in exploration notes.

Audience: people who ship real work on machines all day. v1 only has to prove the harness on a few real tasks (Develop session, investigation or write-up, teach or create one capability and reuse it) with instant tools always available. Pretty ISO plus chat fails. When an agent acts, show status, allow stop, confirm risk.

Keys: [refs/xos-docs.bib](./refs/xos-docs.bib). Collection: ookcite `xos-docs`.
