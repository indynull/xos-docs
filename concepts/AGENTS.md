# Agents

Parent: [../VISION.md](../VISION.md) · See: [CAPABILITIES.md](./CAPABILITIES.md) · [WORK_MODES.md](./WORK_MODES.md) · [../product/SECURITY.md](../product/SECURITY.md)

## Role

The agent is part of the OS. It turns goals into capability calls, and helps create capabilities that do not exist yet. It is not a chat bubble on a normal desktop.

## What it does

| | |
|--|--|
| **Read** | Fetch and present information; structured views; short write-ups with sources when needed |
| **Act** | Multi-step work through capabilities, under limits; show progress; allow stop |
| **Build** | Propose a new capability with the user, get permission, save it |

## Must show

What is happening · history of actions · stop · undo when possible · ask before high-risk steps.

## Modes

The agent stays in the current work mode. It should not drag you into another context without asking.

Which agent library we use is an engineering choice ([../product/TECHNICAL_SHAPE.md](../product/TECHNICAL_SHAPE.md)), not the product idea.
