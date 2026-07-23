# Security

Parent: [../VISION.md](../VISION.md) · See: [../concepts/CAPABILITIES.md](../concepts/CAPABILITIES.md) · [../concepts/AGENTS.md](../concepts/AGENTS.md)

The agent can touch files, network, and tools. If ease of use and safety fight, **safety wins** until we write an exception.

Isolation is not only sandbox flags: **each agent has its own Unix user and ACLs**, under a supervisor that owns lifecycle and channels.

## Risks (examples)

Tools that are too broad · bad content tricking the agent · silent money/delete/auth actions · bad shared tools · acting with your rights without clear intent · no record of what happened · one agent process that is effectively you · agents talking to each other without policy · supervisor that cannot stop a runaway agent.

## Isolation model (product shape)

| Piece | Rule |
|-------|------|
| Supervisor | Starts/stops agents; defines allowed IPC; enforces policy hooks; owns CRIU dump/restore |
| Agent user | Distinct from the human login; no default root; no silent “become me” |
| ACLs | Files, sockets, and tools granted per agent and per capability |
| Checkpoint images | Same sensitivity as process memory / swap; per-agent paths; not world-readable; export off-box is high-risk |
| Human | Approves high-risk steps; can stop or dump any agent; sees the log |
| Capabilities | State network, files, secrets, side effects; off until opened |

Agents do not inherit the full rights of the logged-in user by default. Grants are explicit and reviewable.

## v1 rules

1. Tools off by default; open only as needed.  
2. Each capability states network, files, secrets, side effects.  
3. Login, pay, delete, irreversible change → confirm by default.  
4. Keep a readable log of non-trivial actions (which agent identity did what).  
5. Allow stop (supervisor must be able to kill the agent process).  
6. Agent is not root by default; agent uid ≠ human uid by default.  
7. New capabilities are not fully trusted until reviewed.  
8. Inter-agent channels are deny-by-default; supervisor opens what is needed.  
9. No “run as user” escape without an explicit, logged grant.  
10. CRIU dumps are supervised and logged; restoring an image re-enters that agent identity’s ACL, not the human’s by default.  
11. Copying checkpoint images off the machine needs the same class of confirm as secret export.

## Permission sketch

| Kind | Default |
|------|---------|
| Public read | Allow with limits |
| Private read | Explicit allow (agent ACL) |
| Write in workspace | Limited grant to that agent’s paths |
| Change remote systems | Confirm |
| Change the OS | Confirm; often blocked in v1 |
| Agent → agent call | Deny until supervisor + policy allow |
| Agent uses human credentials | Confirm; prefer short-lived / scoped secrets |
| CRIU dump / restore | Supervisor only; log; images ACL-scoped |
| Export checkpoint images | Confirm; treat as secret-bearing |

Update this file if defaults get wider.
