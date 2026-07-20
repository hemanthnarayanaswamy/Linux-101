# Filesystems

A **filesystem** is the on-disk structure that turns a block device into files and directories. You create one with `mkfs.*` and attach it to the tree with `mount`.

## Choosing a Filesystem

| FS | Strengths | Watch out for |
|:---|:----------|:--------------|
| **ext4** | Default, robust, well-understood, can grow *and* shrink | Nothing fancy — no built-in snapshots/compression |
| **xfs** | Great with large files and parallel I/O; RHEL default | **Cannot shrink** — only grow |
| **btrfs** | Snapshots, compression, copy-on-write, checksums | More complexity; historically weaker RAID5/6 |
| **zfs** | Snapshots, checksums, integrity, pooling | Not in mainline kernel on most distros; heavier |

Rule of thumb: **ext4** unless you have a reason. **xfs** for large-file/throughput workloads (and RHEL defaults). **btrfs/zfs** when you specifically need snapshots/compression and accept the complexity.

## Inodes

An **inode** stores a file's metadata (permissions, owner, timestamps, size, and pointers to data blocks) — everything about a file *except its name and contents*. The directory entry maps a name → inode number.

- A filesystem has a **fixed number of inodes**, set at creation. One inode per file.
- You can run **out of inodes while disk space remains** — millions of tiny files exhaust inodes before bytes. Symptom: `No space left on device` even though `df -h` shows free space.

```bash
df -h     # space usage (bytes)
df -i     # inode usage  ← check this when "disk full" but df -h looks fine
```

## Check and Repair

```bash
fsck /dev/sdb1        # check/repair — filesystem MUST be unmounted
e2fsck -f /dev/sdb1   # ext-specific, force full check
```

Never `fsck` a mounted read-write filesystem — it can corrupt it.

## Mount Options

Set in [`/etc/fstab`](./fstab.md) or on the `mount` command:

| Option | Effect |
|:-------|:-------|
| `defaults` | `rw,suid,dev,exec,auto,nouser,async` |
| `ro` / `rw` | read-only / read-write |
| `noatime` | Don't write an access-time on every read → less I/O (great for busy servers, DBs) |
| `nodiratime` | Same, but for directories only |
| `discard` | Issue TRIM to SSDs on delete (often better as a periodic `fstrim` timer) |

`noatime` matters on read-heavy workloads because, by default, *reading* a file causes a *write* to update its access time — pure overhead you usually don't need.

## Related

- [Block devices and partitions](./block_devices_partitions.md), [/etc/fstab](./fstab.md), [LVM](./lvm.md)
- Commands: [`mkfs`, `fsck`, `tune2fs`, `resize2fs`](../../commands/storage/mkfs_fsck_tune2fs.md), [`mount`, `umount`](../../commands/storage/mount_umount.md)
