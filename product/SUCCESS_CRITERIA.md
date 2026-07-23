# Success criteria

Parent: [../VISION.md](../VISION.md) · See: [V1_SCOPE.md](./V1_SCOPE.md) · [WEDGE.md](./WEDGE.md)

## Pass / fail

Someone who does real work tries the image for an afternoon and can say:

> This is not just chat on Linux. Agents are part of the OS (identity, limits, log). I still have instant tools. Modes and capabilities stick. I cannot get this by installing a random agent CLI.

**Fail:** pretty Linux + chat CLI · only gimmick demos · no harness path · no wedge beyond opencode-on-Ubuntu · agent-only UI with no instant tools.

## Checklist (v1 law)

- [ ] Boots and runs the **harness path** (QEMU OK)  
- [ ] Wedge is obvious: OS harness, not shell-out chat alone  
- [ ] Mode always visible  
- [ ] Plain-language goal path and normal CLI both work  
- [ ] Instant path usable without waiting on a model  
- [ ] At least one teach → sandboxed capability reuse (or explicit create-and-reuse)  
- [ ] Real tasks as in [V1_SCOPE.md](./V1_SCOPE.md)  
- [ ] Supervisor: status, stop, and action log for non-trivial steps  
- [ ] At least two agent identities (distinct user/unit/ACL story)  
- [ ] Agent data/workspace durable on disk (btrfs when the image uses it)  
- [ ] Real Develop session  
- [ ] Security notes match behavior  
- [ ] Demo needs no “ignore the rest of the desktop” disclaimer  
- [ ] People do not leave thinking this is toy lookups on Linux  

## Not required to pass v1

- CRIU automated dump→restore demo  
- Documented multi-machine hardware profile matrix  
- Linker/loader/libc policy document  
- Specific display server (e.g. Wayland) as brand requirement  
- Dual local+remote model fully working  

Those remain **direction** in exploration notes; when CRIU ships for real, kernel-deep + tested restore is the right bar.

## Hard fails

Chat on stock desktop as the product · only toys · someone secretly driving the agent · risky actions with no confirm · agent effectively running as the human by default · success measured by themes or package count · busybox-only with no goal path · “we deleted packages from Ubuntu” with no harness.
