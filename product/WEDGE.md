# Product wedge (what only we ship)

Parent: [../VISION.md](../VISION.md) · See: [V1_SCOPE.md](./V1_SCOPE.md) · [OS_VS_DE.md](./OS_VS_DE.md) · [MODEL_STACK.md](./MODEL_STACK.md)

## The fear

If we improve every layer (compositor, browser, libc, network, local LLM, mail, …) we ship **nothing only we can do**—another thin distro with a chat box, or a half-finished stack at every altitude.

## The non-negotiable wedge

**xOS is an OS-level agent harness**, not “shell out to Grok on a normal desktop.”

| Piece | Why it is hard to fake with “app on Ubuntu” |
|-------|-----------------------------------------------|
| **Supervised agents** | Per-agent user + unit + ACLs; not one chat process as you |
| **Capabilities as OS objects** | Teach → sandbox → reuse; versioned, stoppable, logged |
| **Dual path** | Instant normal tools **and** goal path in one session/mode |
| **CRIU first-class** | Freeze/restore agent trees with the kernel we ship |
| **Durable agent data** | btrfs (or equal) homes/memories, not only chat history |
| **Work modes** | Desktop state follows the task, not the focused window |

That is the **“can’t be done without our thing”** center. Everything else either **supports** the harness or is **later**.

```text
wedge (must be uniquely good)
  agent harness + capabilities + dual path + CRIU/identity/modes

supports the wedge
  lean base, system libc, Wayland modal surface, hardware profiles

later / optional
  custom network stack, full browser product, alternate libc,
  residential-IP agent browsing fleet, speech-first UI, …
```

## What a “better hermes / MCP / opencode” is

Yes: at the OS level this **is** a harness. The difference from hermes/omp/opencode/MCP-only tools:

| App harness | OS harness (us) |
|-------------|-----------------|
| Runs *on* a desktop | *Is* how the desktop/session is structured |
| Your user rights by default | Per-agent identity + ACLs |
| Project/repo scoped | System + project; modes; durable OS state |
| Optional CRIU if you wire it | CRIU + units as product shape |
| Budget is “your API key” | **OS-level** routing and budgets (local vs remote model) |

Do not rebuild every coding-agent feature. Own the **OS integration surface**.

## Scope discipline

| In the wedge for v1 | Explicitly not the wedge for v1 |
|---------------------|----------------------------------|
| Goal → capability → mode loop that feels native | Replacing NetworkManager/iwd |
| Supervisor + two agent identities + dual path | Full agent browser farm (puppeteer + residential IP) |
| CRIU dump/restore of canonical agent tree | Novel system libc |
| One real Develop path + one investigation path | Perfect offline local-LLM desktop for $180 laptops |
| Documented model routing (even if simple) | Owning every website |

If a proposal does not make the **harness** stronger, cheaper, or more trustworthy, it waits.

## Personas (80% later, one now)

We need product personas eventually. For v1, one **primary persona** is enough (e.g. developer/operator who lives in terminal + browser + tickets). Do not design five personas before the harness works.
