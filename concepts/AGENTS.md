# Agents

Parent: [../VISION.md](../VISION.md) · See: [CAPABILITIES.md](./CAPABILITIES.md) · [WORK_MODES.md](./WORK_MODES.md) · [../product/SECURITY.md](../product/SECURITY.md) · [../product/TECHNICAL_SHAPE.md](../product/TECHNICAL_SHAPE.md)

## Role

Agents are part of the OS. They turn goals into capability calls, and help create capabilities that do not exist yet. They are not a chat bubble on a normal desktop, and not a single process with the user’s full rights.

## Supervisor

A **supervisor** (policy layer on top of **systemd-class units**, or units alone if enough) owns agent processes:

- start / stop / restart / idle cleanup via **per-agent units**  
- **durable data**: on disk (**btrfs** preferred); process freeze/resume when we commit a stack (e.g. CRIU—[CHECKPOINTING.md](./CHECKPOINTING.md) is exploration)  
- which agents may talk to which (controlled channels)  
- policy hooks (confirm, log, kill, dump, snapshot)  
- mapping each agent to a **Unix user + ACLs + home/memory paths**

The human talks to the system (modal shell / goal entry). The supervisor talks to agents. Agents do not become a free-for-all mesh.

**Lifecycle upgrade path:** `stop` is not only SIGTERM. Soft path: CRIU dump of the agent tree (images on that agent’s btrfs subvol) → optional unit stop → later restore and continue. Always keep durable agent **memories and workspace** on disk (snapshotted). If CRIU dump fails, kill unit + resume from capability/action log; data remains. Design trees to be dump-friendly (ordinary files, pipes, sockets).

## Identity

| | |
|--|--|
| **Human user** | Interactive login; modes; approvals |
| **Agent user** | One Unix account per agent (or class); limited home/workspace ACLs |
| **Agent unit** | systemd (or compatible) unit: sandbox, cgroup, restart, journal |
| **Agent data** | Concrete paths (memory, logs, capability local state) on **btrfs** subvol—not only chat context in RAM |
| **Capability grants** | Extra rights only as declared and opened |

v1 should demonstrate at least two concurrent agents with **distinct** users/units, even if the default UI path only surfaces one at a time.

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
