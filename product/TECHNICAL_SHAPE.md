# Technical shape

Parent: [../VISION.md](../VISION.md)

Guesses only. Swap parts if the idea still holds. The spine below is intentional: **Wayland + modal shell**, **agent supervisor**, **per-agent Unix identity**, **optimized base**. Not “stock desktop + chat” and not “QEMU demo distro.”

## Layers

| Layer | Direction |
|-------|-----------|
| Base | Small, optimized Linux; real toolchain work (allocators, compiler, kernel knobs)—not only package cuts |
| Limits | Process isolation; network/file limits; **per-agent Unix user + ACLs** |
| Agent supervisor | Supervisord-style: lifecycle, restart policy, inter-agent channels, policy hooks |
| Capability store | Versioned tools; local first |
| Agents | One or more workers under the supervisor; each identity-scoped; reuse existing stacks if they fit |
| Shell | CLI + plain-language goal entry; modes; action log; editor/terminal surfaces |
| Desktop | Wayland compositor; **modal** focus (mode + agent), multi-window allowed not default |
| Web engine | Embed when needed—not the main UI |
| Packaging | Reproducible images; fast rebuilds; prefer real hardware for product proof |

## Desktop and shell

- **Wayland** as the display path. No X11-first design.  
- **Modal UI:** work mode and agent interaction are the center. Extra windows and tabs exist for workflows that need them; they are not the default layout.  
- **Shell is first-class:** ordinary command line remains. Plain-language goals hit the same supervised agent stack (not a second, unconstrained agent).  
- Surfaces (editor, terminal, files, embedded web) share project/mode context.

## Agent supervisor

Think **supervisord / init for agents**, not “spawn a chat process and hope.”

| Concern | Direction |
|---------|-----------|
| Lifecycle | Start, stop, restart, idle kill; explicit, inspectable |
| Identity | Each agent runs as its own Unix user (or equivalent mapped identity) |
| Rights | ACLs and capability grants per agent; human user is not the agent’s effective uid for work |
| Intercommunicate | Named channels / IPC the supervisor allows; no free-for-all among agents |
| Policy | Confirm hooks for high-risk steps; shared action log; stop always works |
| Multi-agent | Multiple agents concurrent when useful; still one visible mode for the human |

Which library implements the agent loop is an engineering choice. **Supervisor + identity + ACLs are product shape.**

## Base system and optimizations

- Remove unused default apps and services.  
- Keep what is left clear and solid (images, deps, compilers/runtimes as needed).  
- **Optimize for real work:** toolchain and libc/kernel choices that matter on real machines (pluggable mallocs, compiler improvements on known hot paths, kernel config/patches that earn their keep). Prior art and team work in this area is in-scope for the base track.  
- **Not enough:** stock desktop plus an AI box.  
- **Not the whole product:** a fast toolchain alone.  
- **Not the product goal:** “boots in QEMU.” QEMU is fine for CI smoke; product proof is real tasks on real hardware when available.

## v1 image

x86_64 first. Prefer install/boot on real hardware for demos. CI may use QEMU. More architectures later (not v1 program).
