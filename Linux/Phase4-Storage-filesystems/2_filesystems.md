# Filesystems

**Roadmap: Week 27.** Pick the right filesystem, and check, repair, and tune it.

## Choosing One

See the concept note [filesystems](../concepts/storage/filesystems.md).

| FS | Use when | Caveat |
|:---|:---------|:-------|
| **ext4** | Default, general purpose; can grow **and** shrink | No snapshots/compression |
| **xfs** | Large files, high throughput; RHEL default | **Cannot shrink** |
| **btrfs** | Need snapshots / compression / CoW | More complexity |
| **zfs** | Integrity + pooling at scale | Not mainline on most distros |

Default to **ext4**; use **xfs** for large-file workloads; reach for **btrfs/zfs** only when you specifically need their features.

## Creating and Mounting

```bash
sudo mkfs.ext4 /dev/vg0/data
sudo mkfs.xfs  /dev/vg0/data
sudo mount -o noatime /dev/vg0/data /mnt/data
```

Commands: [`mkfs`, `fsck`, `tune2fs`, `resize2fs`](../commands/storage/mkfs_fsck_tune2fs.md).

## Inodes

An **inode** holds a file's metadata (perms, owner, timestamps, block pointers) — everything but the name and contents. A filesystem has a **fixed** number of them.

- You can run **out of inodes while bytes remain** — millions of tiny files. Symptom: `No space left on device` even though `df -h` shows free space.

```bash
df -h     # bytes
df -i     # inodes  ← check this when "disk full" makes no sense
```

## Check / Repair (unmounted only)

```bash
sudo fsck /dev/sdb1
sudo e2fsck -f /dev/sdb1     # ext, force full check
```

Never `fsck` a mounted read-write filesystem.

## Tune and Resize

```bash
sudo tune2fs -l /dev/sdb1        # inspect ext superblock
sudo resize2fs /dev/vg0/data     # grow ext4 (xfs: xfs_growfs /mnt/data)
```

## Mount Options That Matter

| Option | Effect |
|:-------|:-------|
| `noatime` | Skip access-time writes on every read → less I/O (busy servers/DBs) |
| `ro` / `rw` | read-only / read-write |
| `discard` | TRIM on delete for SSDs (often better as a periodic `fstrim`) |

Verify with `mount | grep /mnt/data`.

## Practice (roadmap exercises)

1. Format three loopback files as ext4/xfs/btrfs, mount them, compare `df`.
2. Exhaust inodes on a small ext4 volume with tiny files — confirm `No space left on device` while `df -h` shows space.
3. Take a btrfs snapshot, modify files, restore from it.
4. Run `fsck` on an unmounted filesystem, read the output.
5. Mount with `noatime`, verify via `mount`, explain when it matters.

**Mini-project:** a "filesystem decision tree" — given constraints (small vs large files, snapshots, growth, RHEL vs Ubuntu), which FS and why, with real examples.

**Self-check:** What's an inode? Why does xfs lack a shrink operation?

## Related

- Concept: [filesystems](../concepts/storage/filesystems.md), [/etc/fstab](../concepts/storage/fstab.md)
- Commands: [`mkfs`, `fsck`, `tune2fs`, `resize2fs`](../commands/storage/mkfs_fsck_tune2fs.md), [`mount`, `umount`](../commands/storage/mount_umount.md)
