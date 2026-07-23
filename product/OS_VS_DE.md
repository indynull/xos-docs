# OS product vs “just a DE”

Parent: [../VISION.md](../VISION.md) · See: [TECHNICAL_SHAPE.md](./TECHNICAL_SHAPE.md) · [LINKERS_LOADERS.md](./LINKERS_LOADERS.md)

## Tension

If we only ship a modal shell, agent chat, and a compositor, someone will ask: **is this just an alternative to Sway (or another Wayland DE)?**

If we only optimize glibc and CRIU without goal-first UX, someone will ask: **where is the product?**

Both layers are real. The product is **neither** “Sway with Grok” **nor** “libc research with a wallpaper.”

## Layers

| Layer | Examples | Role in xOS |
|-------|----------|-------------|
| **OS-adjacent base** | Kernel profiles, system libc, linkers/loaders, CRIU, btrfs, units, ACLs, multicall core | Makes agents durable, isolated, restorable, fast on metal |
| **Desktop surface** | Wayland, modal shell, work modes, goal entry | How humans drive the system |
| **Application layer** | Browser engine, full IDE, Chromium-class apps | Must run; not the nucleus; escape hatch |

```text
goal → capability → result   (product idea)
        ↑
   agents + modes + shell    (desktop surface)
        ↑
   CRIU + units + ACLs + libc + formats   (OS-adjacent)
```

## What we are not

| Not this | Why |
|----------|-----|
| **Sway fork as the project** | Compositor is a piece; product is goal-first work + supervised agents |
| **Browser-as-OS** | Browser is app-layer; embed when needed ([../principles/NON_GOALS.md](../principles/NON_GOALS.md)) |
| **Libc science project as v1** | Stay near existing system libc; alternate libc is future-scale ([LINKERS_LOADERS.md](./LINKERS_LOADERS.md)) |
| **Chat on stock Plasma/GNOME** | Leaves the old app-hunting workflow intact |

## v1 implication

- Ship enough **OS-adjacent** reality that agents + CRIU + modes are real (not a theme).  
- Ship a **desktop surface** that is modal/goal-first—not a full traditional DE clone.  
- Treat browsers and heavy apps as **app-layer** consumers of a boring, compatible system libc.  
- Do not expand scope to “rewrite the world under Chromium” for v1.
