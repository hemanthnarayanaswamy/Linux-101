# /etc/fstab

`/etc/fstab` (filesystem table) declares which filesystems mount where, and with what options, **at boot**. `mount -a` applies it without rebooting.

## Field Layout

Six space-separated fields per line:

```
# <device>            <mount point>   <type>  <options>        <dump> <pass>
UUID=1b3e...9f  /data           ext4    defaults,noatime  0      2
```

| # | Field | Meaning |
|:-:|:------|:--------|
| 1 | Device | What to mount — **use `UUID=` or `LABEL=`**, not `/dev/sdX` |
| 2 | Mount point | Directory to mount it on (must exist) |
| 3 | Type | Filesystem type: `ext4`, `xfs`, `nfs`, `tmpfs`… |
| 4 | Options | Comma-separated mount options (`defaults`, `noatime`, `ro`…) |
| 5 | Dump | Backup flag for the old `dump` tool — almost always `0` |
| 6 | Pass | `fsck` order at boot: `0` = skip, `1` = root fs, `2` = other fs |

## Why UUID Instead of /dev/sdX

Device names like `/dev/sdb` are assigned in **detection order**, which can change when you add/remove disks or reorder controllers — `sdb` today may be `sdc` after a reboot, and the wrong filesystem mounts (or boot fails). A **UUID** is baked into the filesystem itself and never changes, so the mount is stable.

```bash
blkid /dev/sdb1        # find the UUID
lsblk -f               # UUIDs + types in a tree
```

## Safe Editing Workflow

A bad fstab line can **hang boot**. Always test before rebooting:

```bash
sudo mount -a          # try to mount everything in fstab now
findmnt --verify       # sanity-check fstab entries
```

If a broken entry does block boot, recover via [emergency.target](../systemd/boot_process.md): remount root read-write (`mount -o remount,rw /`), fix the line, reboot.

## Related

- [Block devices and partitions](./block_devices_partitions.md), [filesystems](./filesystems.md)
- Commands: [`mount`, `umount`](../../commands/storage/mount_umount.md), [`lsblk`, `blkid`](../../commands/storage/lsblk_blkid.md)
