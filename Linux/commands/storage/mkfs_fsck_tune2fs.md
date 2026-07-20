# `mkfs`, `fsck`, `tune2fs`, `resize2fs`

Creating, checking, tuning, and resizing filesystems. See the concept note [filesystems](../../concepts/storage/filesystems.md).

## `mkfs` — Create a Filesystem

One tool per filesystem type. **This erases the target device.**

```bash
sudo mkfs.ext4 /dev/vg0/data          # ext4
sudo mkfs.xfs  /dev/vg0/data          # xfs
sudo mkfs.ext4 -L mydata /dev/sdb1    # set a label
sudo mkfs.btrfs /dev/sdb1             # btrfs
```

## `fsck` / `e2fsck` — Check and Repair

```bash
sudo fsck /dev/sdb1        # dispatch to the right fs checker
sudo e2fsck -f /dev/sdb1   # ext2/3/4, force a full check
sudo fsck -y /dev/sdb1     # auto-answer "yes" to repairs
```

> The filesystem **must be unmounted** (or read-only) first. Checking a mounted read-write filesystem can corrupt it.

## `tune2fs` — Tune ext Filesystems

Adjust parameters on an existing ext2/3/4 filesystem without reformatting.

```bash
sudo tune2fs -l /dev/sdb1          # dump all superblock info (UUID, mount count, features)
sudo tune2fs -L newlabel /dev/sdb1 # change the label
sudo tune2fs -m 1 /dev/sdb1        # reserved-blocks % for root (default 5% → 1% on big data disks)
sudo tune2fs -c 30 /dev/sdb1       # force fsck every 30 mounts
```

## `resize2fs` — Grow/Shrink an ext Filesystem

```bash
sudo resize2fs /dev/vg0/data       # grow to fill the underlying LV/partition
sudo resize2fs /dev/vg0/data 2G    # resize to an explicit size
```

- Pairs with `lvextend` (see [LVM commands](./lvm.md)). ext4 can grow online (mounted) and shrink offline.
- **xfs** has no shrink and uses `xfs_growfs /mountpoint` to grow (online only).

## Inodes

```bash
df -i                  # inode usage — check when "disk full" but df -h shows space
sudo mkfs.ext4 -N 2000000 /dev/sdb1   # create with more inodes for many tiny files
```

## Related

- Concept: [filesystems](../../concepts/storage/filesystems.md)
- [LVM commands](./lvm.md), [`mount`, `umount`](./mount_umount.md)
