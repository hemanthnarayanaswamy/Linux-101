# The Linux Boot Process

What happens between pressing the power button and the login prompt.

```
Power on
  ↓
BIOS / UEFI          firmware; runs POST, finds the boot device
  ↓
Bootloader (GRUB)    loads the kernel + initramfs into memory
  ↓
Kernel               initializes hardware, mounts a temporary root
  ↓
initramfs            tiny temp filesystem with drivers to find the real root
  ↓
systemd (PID 1)      mounts real root, starts units up to the default target
  ↓
default.target       (usually multi-user.target or graphical.target)
  ↓
Login prompt
```

## The Stages

- **BIOS / UEFI** — firmware on the motherboard. Runs POST (power-on self test), then hands off to the bootloader. UEFI is the modern replacement for legacy BIOS (supports GPT, secure boot, larger disks).
- **GRUB** — the bootloader. Presents the menu, loads the selected kernel and initramfs. Config: `/etc/default/grub` → run `update-grub` to regenerate `/boot/grub/grub.cfg`. Never edit the generated file directly.
- **Kernel** — decompresses, sets up memory/CPU/devices, then mounts the initramfs as a temporary root.
- **initramfs** — a small in-memory filesystem holding just enough drivers/modules to locate and mount the *real* root filesystem (e.g. drivers for LVM, RAID, encrypted disks). Needed because the kernel can't contain every possible driver.
- **systemd** — becomes PID 1, mounts the real root, and activates units until the default target is reached.

## Kernel Command Line

Parameters GRUB passes to the kernel are visible at runtime:

```bash
cat /proc/cmdline
# e.g. BOOT_IMAGE=/vmlinuz root=UUID=... ro quiet splash
```

Common parameters: `ro` (mount root read-only first), `quiet` (suppress boot messages), `single` / `systemd.unit=rescue.target` (recovery).

## Recovery

- **rescue.target** — single-user, minimal services, root shell. Append `systemd.unit=rescue.target` to the kernel line in GRUB.
- **emergency.target** — even more minimal: root shell with root mounted read-only and almost nothing else started. For when rescue itself fails.
- A broken `/etc/fstab` entry can hang boot — recover by booting to emergency mode, remounting root read-write (`mount -o remount,rw /`), fixing fstab, rebooting.

## Related

- [Targets and runlevels](./targets_runlevels.md)
- Command references: [`systemd-analyze`](../../commands/systemd/systemd-analyze.md), [`dmesg`](../../commands/logging/dmesg.md)
