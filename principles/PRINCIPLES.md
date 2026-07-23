# Principles

Parent: [../VISION.md](../VISION.md) · See also: [NON_GOALS.md](./NON_GOALS.md)

If we break one of these, write down why. These are **product law**. Implementation bets (CRIU ship date, Wayland, malloc, hardware profile machinery) are **not** listed here—see exploration notes under [../product/](../product/).

| # | Rule | Meaning |
|---|------|---------|
| 1 | Work first | Default path helps the current task. Themes and docks are not the product. |
| 2 | Goals first | Prefer goal → result over hunting apps and tabs. |
| 3 | Capabilities first | Long-lived interface is reusable tools/contracts, not “which app.” |
| 4 | Save skills | Wire once or teach from normal use; keep for next time (sandboxed). |
| 5 | Security matters | Tight permissions, log, stop, ask before dangerous steps. |
| 6 | Stay small | Ship a clear product, not every package. |
| 7 | Multicall-leaning base | Build up from a small core—not “consumer distro minus packages.” |
| 8 | Honest v1 | Harness on real tasks—not every layer rewritten. |
| 9 | Visible agent | Status, stop, trail; name the agent identity. |
| 10 | Clear mode | Always show which work mode is active. |
| 11 | Supervised agents | Lifecycle and policy through a supervisor—not ad-hoc chat processes. |
| 12 | Isolated agents | Per-agent identity (user + unit-class isolation + ACLs); not the human’s full rights by default. |
| 13 | Native shell | CLI and plain-language goals share one stack. |
| 14 | Dual path | Instant normal tools always; agent path for multi-step goals. |
| 15 | Durable agent state | Agent data on disk; **btrfs** preferred for homes/workspaces/snapshots. |
| 16 | OS not only DE | Goal-first surface + harness + lean base—not a tiling-WM fork as the project. |
| 17 | Wedge first | OS agent harness is the unique product; defer non-wedge layers. |
| 18 | Progressive replace | Start lean; replace apps over time; no bloated default desktop suite. |

**Direction only (exploration):** agent process checkpoint/restore (e.g. CRIU when committed), dual remote/local models + budgets, display stack, hardware profiles, linker/loader policy—see product tech notes.

More: [../concepts/](../concepts/) · [../product/SECURITY.md](../product/SECURITY.md) · [../product/V1_SCOPE.md](../product/V1_SCOPE.md) · [../product/WEDGE.md](../product/WEDGE.md)
