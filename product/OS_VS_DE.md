# OS product vs “just a DE”

Parent: [../VISION.md](../VISION.md) · See: [TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md) · [LINKERS_LOADERS.md](./LINKERS_LOADERS.md) · [../concepts/CAPABILITIES.md](../concepts/CAPABILITIES.md)

## Tension

If we only ship a modal shell, agent chat, and a compositor, someone will ask: **is this just an alternative to Sway (or another Wayland DE)?**

If we only optimize glibc and CRIU without goal-first UX, someone will ask: **where is the product?**

Both layers are real. The product is **neither** “Sway with Grok” **nor** “libc research with a wallpaper.”

**Team alignment (not a fork in the road):** the desktop surface is a **big part**, and we still **touch the base** (leaner kernel/libc/toolchain, CRIU, isolation) so the same machine is more capable *and* faster than a stock “AI on ChromeOS-shaped” story. Goal-first UX + optimized base are one product, not two camps.

## Layers

| Layer | Examples | Role in xOS |
|-------|----------|-------------|
| **OS-adjacent base** | Kernel profiles, system libc (near stock for v1), linkers/loaders, CRIU, btrfs, units, ACLs, multicall core | Durable, isolated, restorable agents; measured speed |
| **Desktop surface** | Wayland, modal shell, work modes, goal entry; little chrome | How humans drive the system |
| **Application layer** | Browser, mail, full IDE, Chromium-class apps | Must run; escape hatch; **gradually** replaceable by capabilities |

```text
goal → capability → result   (product idea)
        ↑
   agents + modes + shell    (desktop surface)
        ↑
   CRIU + units + ACLs + libc + formats   (OS-adjacent)
```

## Dual path: instant “normal” + agent

Agents (especially model-backed) can be **slow** compared to rofi, foot, or a hot key. That is a product constraint, not a footnote.

| Path | When | Rule |
|------|------|------|
| **Instant / normal** | Known muscle memory: terminal, launcher, editor, theme-ish toggles that hot-reload | Always available; no “wait for thinking” |
| **Agent / goal** | Ambiguous multi-step work, wiring a new system, investigation | Visible status; stop; log |
| **Teach then freeze** | Operator does a thing the normal way enough times that a **pattern** is clear | Persist as a **capability in a sandbox**—do not let the agent keep freestyling that path |

```text
human does it the old way a few times
  → pattern is clear
  → save capability (sandboxed, versioned)
  → next time: goal hits the capability (fast path, not freeform inventing)
```

**Keep normal access.** Leaving everything to the model “for now” fails latency and control. Bernhard: keep ways to access stuff normally. Ali: operator establishes pattern, then persist so the agent does not get creative.

## Progressive replacement of apps

We cannot replace all applications from the start. Default install stays **small** (no full ricing/admin GUI subsystem the agent can own). Real users will still pull Qt/GTK stacks over time—that is expected; the product does not *start* as “mini Arch that bloated into everything.”

| Phase | Direction |
|-------|-----------|
| v1 | Lean default; real Develop/Research paths; old-way tools present where needed |
| Later | More workflows become capabilities; fewer default “desktop apps” |
| Always | Escape hatch to full browser / normal apps |

**Strip by default:** wallpaper/theme *products*, icon-theme bureaucracy, GUI admin for what agents + config can do. **Keep when needed:** icons/affordances that make sense in a goal-first, agent-heavy desk—not zero chrome for ideology.

Theme/light-dark: prefer **implicit** system behavior (time, ambient if available) and hot-reload-friendly configs—not a separate “ricing app.”

## GUI that is not 1995 window soup

Traditional GUI paradigms fight “agents do most of the work.” We still need some visual language (icons, surfaces) that fits modes + goals + status—not a clone of decades of launcher/desktop brainwashing, and not a pure CLI that people refuse to try.

Speech-to-text (e.g. whisper.cpp → same goal/CLI path) is an **input modality**, later—same stack as typed goals, not a second product.

## What we are not

| Not this | Why |
|----------|-----|
| **Sway fork as the project** | Compositor is a piece; product is goal-first work + supervised agents + base |
| **Agent-only UI with no instant path** | Latency and trust; always keep normal tools |
| **Browser-as-OS** | App layer; embed when needed |
| **Libc science project as v1** | Stay near existing system libc |
| **Chat on stock Plasma/GNOME** | Old workflow intact |
| **Default desktop ricing subsystem** | Not the product; strip |
| **Promise zero third-party app deps forever** | Users install apps; we manage *defaults* and progressive replacement |

## Hard apps (email, auth, Exchange-class)

“Do I have new email?” is a goal shape. Delivery may use real mail stacks (local or remote). Enterprise auth (2FA device registration, Exchange quirks) is **capability + security** work, not “the agent invents OAuth.” Do not pretend v1 owns every mail server.

## v1 implication

- Ship **OS-adjacent** reality (agents, CRIU, lean base) **and** a goal-first surface.  
- Ship **instant normal** tools beside the agent path.  
- At least one **teach → capability** path (operator does it, then save).  
- Do not expand to rewrite-the-world under Chromium or a new libc for v1.  
- Do not ship as “Grokdesk = lxqt with a chat box.”
