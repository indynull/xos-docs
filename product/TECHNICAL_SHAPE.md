# Technical shape

Parent: [../VISION.md](../VISION.md)

Guesses only. Swap parts if the idea still holds.

## Layers

| Layer | Direction |
|-------|-----------|
| Base | Small Linux, not a full consumer distro |
| Limits | Process isolation; network/file limits |
| Capability store | Versioned tools; local first |
| Agent | Uses tools under limits; reuse existing stacks if they fit |
| Shell | Modes, goal entry, log, editor/terminal |
| Web engine | Embed when needed—not the main UI |
| Packaging | Reproducible images; fast rebuilds |

## Base system

- Remove unused default apps and services.  
- Keep what is left clear and solid (images, deps, compilers/runtimes as needed).  
- **Not enough:** stock desktop plus an AI box.  
- **Not the whole product:** a fast toolchain alone.

## v1 image

x86_64 QEMU first. Boot tests in CI. More architectures later.
