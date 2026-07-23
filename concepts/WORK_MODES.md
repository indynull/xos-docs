# Work modes

Parent: [../VISION.md](../VISION.md) · See: [AGENTS.md](./AGENTS.md) · [CAPABILITIES.md](./CAPABILITIES.md)

## What a mode is

A **work mode** sets up the desktop for one kind of work: main view, available tools, agent defaults, little chrome.

The UI is **modal**: the active mode and agent interaction sit at the center. Extra windows and tabs are allowed when a workflow needs them; they are not the default layout.

| Rule | |
|------|--|
| Visible | You always know the current mode |
| Easy to switch | Changing mode is intentional and quick |
| No hidden modes | Context does not change in secret |
| About the task | Chosen by kind of work, not by which window is focused |
| Quiet UI | Mode controls are not another app launcher |
| Modal, not window soup | Wayland surface; goal/agent entry is first-class |

## Example modes (names can change)

| Mode | Focus | Example |
|------|--------|---------|
| Develop | Editor, project, terminal | Code, config, tests |
| Research | Reading and notes | Investigation, docs |
| Create | Writing or design surface | Docs, design packages |
| Ops | Logs, checks, careful changes | Bring-up, deploys |

## Automatic switch

OK if the mode is shown, easy to undo, and you can lock a choice.

## Surfaces

Editor, terminal, files, and browser engine share one session/context. They should feel like parts of the same desk, not five separate products. Full browser is an escape hatch, not the usual first step.

## Shell and dual path

In every mode:

- **Instant path:** normal CLI, terminal, launcher-class actions—no waiting on a model.  
- **Goal path:** plain-language goals use the supervised agent stack ([AGENTS.md](./AGENTS.md)).  
- **Both stay.** Agent lag must not own the only way to open a terminal or flip a known toggle.

See dual path and progressive app replacement: [../product/OS_VS_DE.md](../product/OS_VS_DE.md).
