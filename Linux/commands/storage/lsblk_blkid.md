# The `lsblk` and `blkid` Commands

Two tools for discovering what block devices exist and what's on them. See [block devices and partitions](../../concepts/storage/block_devices_partitions.md).

## `lsblk` — List Block Devices

Shows disks → partitions → mount points as a tree. Reads from sysfs, no root needed.

```bash
lsblk                 # tree of all block devices
lsblk -f              # + filesystem type, UUID, mountpoint (very useful)
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT
lsblk /dev/sda        # just one device
```

Example:

```
NAME        SIZE TYPE MOUNTPOINT
sda          50G disk
├─sda1        1G part /boot
└─sda2       49G part
  └─vg0-root 49G lvm  /
```

## `blkid` — Block Device Attributes

Prints UUID, LABEL, and filesystem TYPE — the values you put in [`/etc/fstab`](../../concepts/storage/fstab.md).

```bash
blkid                 # all devices with a filesystem/UUID
blkid /dev/sda1       # one device
sudo blkid -s UUID -o value /dev/sda1   # just the UUID (scriptable)
```

## Typical Use

Find a newly added disk and its UUID:

```bash
lsblk                                    # spot the new /dev/sdb (no mountpoint)
sudo blkid /dev/sdb1                      # grab its UUID for fstab
```

## Related

- Concepts: [block devices and partitions](../../concepts/storage/block_devices_partitions.md), [/etc/fstab](../../concepts/storage/fstab.md)
- [`parted`, `fdisk`, `gdisk`](./parted_fdisk_gdisk.md)
