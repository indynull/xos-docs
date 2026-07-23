# Success criteria

Parent: [../VISION.md](../VISION.md) · See: [V1_SCOPE.md](./V1_SCOPE.md)

## Pass / fail

Someone who does real work tries the image for an afternoon (on real hardware when possible) and can say:

> I barely needed a normal browser or app menu. The mode matched what I was doing. Agents felt supervised and limited, not like my whole account on autopilot. I can see this growing into how I work.

**Fail:** pretty Linux + chat on a normal desktop · only gimmick demos · “it boots in QEMU” with no real-work path.

## Checklist

- [ ] Boots and runs real tasks (prefer real hardware; QEMU OK only as CI smoke)  
- [ ] Wayland modal shell; mode always visible  
- [ ] Plain-language goal path and normal CLI both work  
- [ ] Real tasks as in [V1_SCOPE.md](./V1_SCOPE.md)  
- [ ] Supervisor: status, stop, and action log for non-trivial steps  
- [ ] (Stretch) CRIU dump/restore of one simple agent tree on real hardware, or honest fail-soft logged  
- [ ] Agent data/workspace paths durable; btrfs snapshots when the image uses btrfs  
- [ ] At least two agent identities with distinct users/units/ACLs  
- [ ] Create (or register) a capability and use it again  
- [ ] Real Develop session  
- [ ] Security notes match behavior  
- [ ] Demo needs no “ignore the rest of the desktop” disclaimer  
- [ ] People do not leave thinking this is toy lookups on Linux  

## Hard fails

Chat on stock desktop as the product · only toys · someone secretly driving the agent · risky actions with no confirm · agent effectively running as the human by default · success measured by themes, package count, or QEMU boot alone · “we deleted packages from Ubuntu” with no multicall/composable story · busybox-only with no real goal path.
