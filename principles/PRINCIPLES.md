# Principles

Parent: [../VISION.md](../VISION.md) · See also: [NON_GOALS.md](./NON_GOALS.md)

If we break one of these, write down why.

| # | Rule | Meaning |
|---|------|---------|
| 1 | Work first | Default path helps the current task. Themes and docks are not the product. |
| 2 | Goals first | Prefer goal → result over hunting apps and tabs. |
| 3 | Capabilities first | Long-lived interface is reusable tools/contracts, not “which app.” |
| 4 | Save skills | When we wire something new, keep it for next time. |
| 5 | Security matters | Agent is powerful: tight permissions, log, stop, ask before dangerous steps. |
| 6 | Stay small | Ship a clear product, not every package. |
| 7 | Radical small base | Prefer Linux + multicall core (BusyBox-class) over a trimmed consumer distro. Cosmopolitan/APE tools when portability pays. Optimize and measure real work. |
| 8 | Honest v1 | Success is showing the idea on real work—not a full distro program or a QEMU demo. |
| 9 | Visible agent | Show status, allow stop, keep a trail; name the agent identity. |
| 10 | Clear mode | Always show which work mode is active. |
| 11 | Supervised agents | Lifecycle via units + supervisor; soft stop can CRIU-checkpoint; data lives on disk. |
| 12 | Isolated agents | Each agent: user + unit sandbox + ACLs; not the human’s full rights by default. |
| 13 | Native shell | CLI and plain-language goals share one stack; shell is not an afterthought. |
| 14 | Modal desktop | Wayland + mode-centered UI; many windows possible, not the default. |
| 15 | Durable agent state | btrfs (or equal) for memories/workspaces; snapshots under agent traffic. |
| 16 | Hardware-aware base | Real profiles and measurement; QEMU is CI, not the product. |

More: [../concepts/](../concepts/) · [../product/SECURITY.md](../product/SECURITY.md) · [../product/V1_SCOPE.md](../product/V1_SCOPE.md) · [../product/TECHNICAL_SHAPE.md](../product/TECHNICAL_SHAPE.md)
