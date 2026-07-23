# Agents

Parent: [../VISION.md](../VISION.md) · See: [CAPABILITIES.md](./CAPABILITIES.md) · [WORK_MODES.md](./WORK_MODES.md) · [../product/SECURITY.md](../product/SECURITY.md) · [../product/TECHNICAL_SHAPE.md](../product/TECHNICAL_SHAPE.md)

## Role

Agents are part of the OS. They turn goals into capability calls, and help create capabilities that do not exist yet. They are not a chat bubble on a normal desktop, and not a single process with the user’s full rights.

## Supervisor

A **supervisor** (supervisord-style) owns agent processes:

- start / stop / restart / idle cleanup  
- **checkpoint / restore** via [CRIU](./CHECKPOINTING.md) when the process tree allows (freeze instead of only kill)  
- which agents may talk to which (controlled channels)  
- policy hooks (confirm, log, kill, dump)  
- mapping each agent to a **Unix user + ACLs**

The human talks to the system (modal shell / goal entry). The supervisor talks to agents. Agents do not become a free-for-all mesh.

**Lifecycle upgrade path:** `stop` is not only SIGTERM. Preferred soft path: CRIU dump of the agent tree (images under that agent’s ACL store) → optional leave frozen or exit → later restore and continue. If dump fails, fall back to kill + resume from capability/action log. Design agent trees to be dump-friendly (ordinary files, pipes, sockets; avoid mystery devices).

## Identity

| | |
|--|--|
| **Human user** | Interactive login; modes; approvals |
| **Agent user** | One per agent (or per agent class); limited home/workspace ACLs |
| **Capability grants** | Extra rights only as declared and opened |

v1 should demonstrate at least two concurrent agents under the supervisor with **distinct** identities, even if the default UI path only surfaces one at a time.

## What an agent does

| | |
|--|--|
| **Read** | Fetch and present information; structured views; short write-ups with sources when needed |
| **Act** | Multi-step work through capabilities, under limits; show progress; allow stop |
| **Build** | Propose a new capability with the user, get permission, save it |
| **Talk** | Call another agent only through supervisor-allowed channels |

## Must show

What is happening · which agent identity · history of actions · stop · checkpoint/restore status when used · undo when possible · ask before high-risk steps.

## Modes

Agents stay in the current work mode. They should not drag you into another context without asking.

## Shell path

Plain-language goals and the normal CLI both reach the same supervised stack. The shell is not a second, unconstrained agent.

Which agent library we use is an engineering choice ([../product/TECHNICAL_SHAPE.md](../product/TECHNICAL_SHAPE.md)). **Supervisor, identity, and ACLs are product shape**, not library trivia.
