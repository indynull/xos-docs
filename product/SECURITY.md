# Security

Parent: [../VISION.md](../VISION.md) · See: [../concepts/CAPABILITIES.md](../concepts/CAPABILITIES.md)

The agent can touch files, network, and tools. If ease of use and safety fight, **safety wins** until we write an exception.

## Risks (examples)

Tools that are too broad · bad content tricking the agent · silent money/delete/auth actions · bad shared tools · acting with your rights without clear intent · no record of what happened.

## v1 rules

1. Tools off by default; open only as needed.  
2. Each capability states network, files, secrets, side effects.  
3. Login, pay, delete, irreversible change → confirm by default.  
4. Keep a readable log of non-trivial actions.  
5. Allow stop.  
6. Agent is not root by default.  
7. New capabilities are not fully trusted until reviewed.

## Permission sketch

| Kind | Default |
|------|---------|
| Public read | Allow with limits |
| Private read | Explicit allow |
| Write in workspace | Limited grant |
| Change remote systems | Confirm |
| Change the OS | Confirm; often blocked in v1 |

Update this file if defaults get wider.
