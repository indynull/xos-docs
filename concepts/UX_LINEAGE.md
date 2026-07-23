# UX lineage (inspiration)

Parent: [../VISION.md](../VISION.md) · See: [WORK_MODES.md](./WORK_MODES.md) · [CAPABILITIES.md](./CAPABILITIES.md)

Not clones. Prior art that still points at the product.

## Archy (Jef Raskin)

[Archy](https://en.wikipedia.org/wiki/Archy_(software)) (Raskin Center; *The Humane Interface*): content persistence, a **nucleus of commands** instead of app hunting, navigation by incremental text search, zooming UI ideas, humane defaults.

**Map to xOS:**

| Archy idea | xOS shape |
|------------|-----------|
| Commands / nucleus, not apps as the unit | Goals + **capabilities** |
| Content persistence | Workspace + agent memory on **btrfs**; capabilities saved |
| Search-first navigation | Goal entry / plain-language shell as primary path |
| Modeless ideals | We still use **work modes** as explicit task context (visible, intentional)—not app windows as mode |

We are not rebuilding Archy’s full ZUI in v1. We take the *command and persistence* lesson seriously.

## Enso (Humanized)

Enso: lightweight **command invocation** over the desktop—type a command, act, return—without turning the whole machine into a traditional app launcher.

**Map to xOS:** modal / plain-language shell as the center; agents execute under policy; many windows allowed, not required.

## What we do not inherit blindly

| Trap | Avoid |
|------|--------|
| Pure modeless dogma | Work modes stay visible; agents stay in mode |
| Wallpaper / theme as product | Still a non-goal |
| Research UI with no isolation story | ACLs, units, CRIU/btrfs are product shape |

Full non-goals: [../principles/NON_GOALS.md](../principles/NON_GOALS.md).
