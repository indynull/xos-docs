# Success criteria

Parent: [../VISION.md](../VISION.md) · See: [V1_SCOPE.md](./V1_SCOPE.md)

## Pass / fail

Someone who does real work tries the image for an afternoon and can say:

> This is not just chat on Linux. Agents are part of the OS (identity, limits, log). I still have instant tools. Modes and capabilities stick. I cannot get this stack by installing a random agent CLI.

**Fail:** pretty Linux + chat CLI · only gimmick demos · QEMU boot with no harness · no wedge beyond opencode-on-Ubuntu.

## Checklist

- [ ] Boots and runs harness path on **QEMU and/or documented real-hardware profile**  
- [ ] Wedge is obvious: OS harness, not shell-out chat alone ([WEDGE.md](./WEDGE.md))  
- [ ] Wayland modal shell; mode always visible  
- [ ] Plain-language goal path and normal CLI both work  
- [ ] Instant path usable without waiting on a model (e.g. open terminal / known hot path)  
- [ ] At least one teach → sandboxed capability reuse (or explicit create-and-reuse)  
- [ ] Real tasks as in [V1_SCOPE.md](./V1_SCOPE.md)  
- [ ] Supervisor: status, stop, and action log for non-trivial steps  
- [ ] **CRIU first-class on our kernel:** dump → restore → agent continues a known task (primary profile); fail-soft only after real attempt  
- [ ] Agent data/workspace paths durable; btrfs snapshots when the image uses btrfs  
- [ ] Documented **formats + linker/loader + system libc + debug** policy for the canonical agent tree  
- [ ] At least two agent identities with distinct users/units/ACLs  
- [ ] Create (or register) a capability and use it again  
- [ ] Real Develop session  
- [ ] Security notes match behavior  
- [ ] Demo needs no “ignore the rest of the desktop” disclaimer  
- [ ] People do not leave thinking this is toy lookups on Linux  

## Hard fails

Chat on stock desktop as the product · only toys · someone secretly driving the agent · risky actions with no confirm · agent effectively running as the human by default · success measured by themes, package count, or QEMU boot alone · “we deleted packages from Ubuntu” with no multicall/composable story · busybox-only with no real goal path · CRIU missing, bolted on a random kernel, or only “works sometimes” with no test.
