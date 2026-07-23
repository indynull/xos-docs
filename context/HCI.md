# HCI / UX notes (grounded)

Parent: [../VISION.md](../VISION.md) · Bib: [../refs/xos-docs.bib](../refs/xos-docs.bib) · Collection: ookcite `xos-docs` · **Precis for every cite:** [READING.md](./READING.md) · Index: [REFERENCES.md](./REFERENCES.md)

Exploration, not product law. The spine still wins if we never ship a custom widget kit.

## Problem class

Agent UIs fail in two boring ways.

1. **Only chat.** Latency eats muscle memory. Horvitz already framed the tension between automation and user initiative as a *mixed-initiative* design problem [@horvitzPrinciplesMixedInitiative1999]. Waiting on a model for "open terminal" is not mixed initiative; it is unilateral.

2. **Only classic desktop.** Users become the glue across apps. That is the failure mode the brief targets. Direct manipulation literature stresses predictable, controllable surfaces [@shneidermanDirectManipulation1997]; a freeform agent that rewrites your mail rules without a durable, inspectable capability is the opposite of controllable.

xOS product law takes both seriously: **dual path** (instant tools always; goal path when multi-step work pays) and **teach -> sandboxed capability** (pattern freezes into something stoppable and reviewable). That maps to Human-AI guidelines on making clear what the system can do, supporting efficient dismissal/correction, and avoiding unexplained agency [@amershiGuidelinesHumanAIInteraction2019].

## What the literature actually buys us

| Claim in product docs | Grounding | Does *not* buy |
|----------------------|-----------|----------------|
| Keep a non-model path for known actions | Mixed-initiative principles [@horvitzPrinciplesMixedInitiative1999]; Human-AI guidelines on timing and user control [@amershiGuidelinesHumanAIInteraction2019] | A specific compositor brand |
| Prefer inspectable, reversible agent work | Controllability / predictability in direct manipulation [@shneidermanDirectManipulation1997]; accountability/explainability survey space [@abdulTrendsTrajectoriesExplainable2018] | Full formal verification of agents |
| Command nucleus / humane defaults as *inspiration* | Raskin, *The Humane Interface* [@raskinHumaneInterface2000]; review [@brownReviewHumaneInterface2002] | Archy clone or ZUI roadmap |
| LLM UI as direct manipulation hybrid | DirectGPT studies direct-manipulation + LLM interaction [@massonDirectGPT2024] | Proof that "modal shell" is optimal |
| Learnability matters when users lack prior patterns | Design-for-learnability critique of "intuitive" UX [@pottDesignLearnabilityChallenging2023] | License to ship empty chrome |

Keys resolve in [../refs/xos-docs.bib](../refs/xos-docs.bib). DOIs were ookcite-validated into collection `xos-docs`.

## Failure modes the papers warn about (map to non-goals)

- **Hidden automation** that steals initiative without a cheap undo path [@horvitzPrinciplesMixedInitiative1999; @amershiGuidelinesHumanAIInteraction2019] -> agent-only UI is out.
- **Unintelligible systems** at scale [@abdulTrendsTrajectoriesExplainable2018] -> status, stop, action log are product law, not polish.
- **Assuming prior digital literacy** for "intuitive" chrome [@pottDesignLearnabilityChallenging2023] -> dual path and teach-to-capability beat a mystery gesture DE.

## What we are *not* doing from HCI fashion

- Treating avatar embodiment papers or random CHI demos as design law (several free-text lookups hit unrelated CHI work; see REFERENCES hygiene).
- Claiming a new interaction theory. Composition of known HCI constraints with an OS-level agent harness is the *hypothesis* ([NOVELTY.md](./NOVELTY.md)).
- Shipping research-UI without isolation. Controllability without ACLs and durable state is theatre.

## Open questions (need measurement, not slogans)

1. How often do users take the instant path vs the goal path in a Develop day? Log it.
2. Does teach-to-capability cut freeform agent steps by a measurable factor on repeated tasks?
3. Where do users still demand full browser chrome despite structured sources?

Until those numbers exist, keep the UI claims small and the dual-path requirement hard.
