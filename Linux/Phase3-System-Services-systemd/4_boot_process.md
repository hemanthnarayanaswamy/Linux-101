# Boot Process

**Roadmap: Week 24.** Understand what happens between power-on and the login prompt, and diagnose boot failures.

## The Chain

Full walkthrough in the concept note: [the boot process](../concepts/systemd/boot_process.md).

```
BIOS/UEFI → GRUB (bootloader) → kernel → initramfs → systemd (PID 1) → default.target → login
```

- **BIOS/UEFI** — firmware; POST, then hands off to the bootloader.
- **GRUB** — loads the kernel + initramfs; config in `/etc/default/grub`, regenerate with `update-grub`.
- **kernel** — initializes hardware, mounts initramfs.
- **initramfs** — tiny temp root with just enough drivers to find/mount the *real* root (LVM, RAID, encryption).
- **systemd** — becomes PID 1, mounts real root, starts units to the default target.

## Analyzing Boot Time

See [`systemd-analyze`](../commands/systemd/systemd-analyze.md).

```bash
systemd-analyze                    # total boot time (kernel + userspace)
systemd-analyze blame              # slowest units, worst first
systemd-analyze critical-chain     # what actually gated boot
```

`blame` = slow in isolation; `critical-chain` = slow *and* on the critical path.

## Kernel Command Line

```bash
cat /proc/cmdline
# root=UUID=... ro quiet splash
```

Add a parameter via `/etc/default/grub` (edit `GRUB_CMDLINE_LINUX`), then:

```bash
sudo update-grub    # regenerate /boot/grub/grub.cfg
sudo reboot
```

## Recovery Targets

- **rescue.target** — single-user, minimal, root shell. Append `systemd.unit=rescue.target` to the kernel line in GRUB at boot.
- **emergency.target** — even more minimal (root mounted read-only, almost nothing started). For when rescue fails.
- A broken `/etc/fstab` can hang boot → boot to emergency, `mount -o remount,rw /`, fix fstab, reboot.

## Hardware Detection

```bash
dmesg | head -100     # early boot / hardware messages
journalctl -k -b      # same kernel messages, persistent
```

See [`dmesg`](../commands/logging/dmesg.md).

## Practice (roadmap exercises)

1. `systemd-analyze` (note total time) → `systemd-analyze blame` (slowest service).
2. `cat /proc/cmdline` — match each parameter to its purpose.
3. Add `quiet` via `/etc/default/grub`, `update-grub`, reboot, verify.
4. (VM, snapshot first) boot to rescue via GRUB edit (`systemd.unit=rescue.target`), get a root shell, `exit` to continue.
5. `dmesg | head -100` — identify hardware detection messages.

**Mini-project:** a boot-time analysis report — total time + breakdown, top 5 slowest units, one service you can justify disabling, then disable it and measure the improvement.

**Self-check:** What is initramfs and why do we need it? How would you recover a system whose `/etc/fstab` has a broken entry?

## Related

- Concept: [the boot process](../concepts/systemd/boot_process.md), [targets and runlevels](../concepts/systemd/targets_runlevels.md)
- Commands: [`systemd-analyze`](../commands/systemd/systemd-analyze.md), [`dmesg`](../commands/logging/dmesg.md)
