# Phase 4 - Storage & Filesystems

This is Phase 5 (Storage & Filesystems) of the roadmap, kept as Phase 4 in this repo because the roadmap's Phase 2 (Shell Scripting & Automation) was skipped.

**Phase outcome:** partition disks, manage LVM, choose appropriate filesystems, and configure NFS shares.

Primary roadmap sources:

- [linux-mastery-roadmap.md](../../resources/roadMap/linux-mastery-roadmap.md)
- [linux-mastery-roadmap-detailed.md](../../resources/roadMap/linux-mastery-roadmap-detailed.md)

## Quick Index

| Order | Topic | Main Note |
|---:|:------|:----------|
| 1 | Disks, partitions, LVM | [1_disks_partitions_lvm.md](./1_disks_partitions_lvm.md) |
| 2 | Filesystems | [2_filesystems.md](./2_filesystems.md) |
| 3 | NFS, bind mounts, tmpfs | [3_nfs_bind_tmpfs.md](./3_nfs_bind_tmpfs.md) |

## Reference Indexes

| Area | Index |
|:-----|:------|
| Commands | [Linux commands index](../commands/README.md) |
| Concepts | [Linux concepts index](../concepts/README.md) |

---

## 1. Disks, Partitions, LVM

Block devices, MBR vs GPT partition tables, the LVM stack (PV/VG/LV), UUID-based `/etc/fstab` mounts, and growing storage online.

Main note:

- [1_disks_partitions_lvm.md](./1_disks_partitions_lvm.md)

Concepts:

- [Block devices and partitions](../concepts/storage/block_devices_partitions.md)
- [LVM](../concepts/storage/lvm.md)
- [/etc/fstab](../concepts/storage/fstab.md)

Commands:

- [`lsblk`, `blkid`](../commands/storage/lsblk_blkid.md)
- [`parted`, `fdisk`, `gdisk`](../commands/storage/parted_fdisk_gdisk.md)
- [LVM tools](../commands/storage/lvm.md)
- [`mkfs`, `fsck`, `tune2fs`, `resize2fs`](../commands/storage/mkfs_fsck_tune2fs.md)

---

## 2. Filesystems

Choosing ext4/xfs/btrfs/zfs, inodes, checking and repairing, tuning, and mount options.

Main note:

- [2_filesystems.md](./2_filesystems.md)

Concepts:

- [Filesystems](../concepts/storage/filesystems.md)

Commands:

- [`mkfs`, `fsck`, `tune2fs`, `resize2fs`](../commands/storage/mkfs_fsck_tune2fs.md)
- [`mount`, `umount`](../commands/storage/mount_umount.md)
- [`df`, `du`](../commands/file-metadata/file_stat_du_df.md) (`df -i` for inodes)

---

## 3. NFS, Bind Mounts, tmpfs

Sharing storage across machines with NFS, plus bind mounts and RAM-backed tmpfs for special cases.

Main note:

- [3_nfs_bind_tmpfs.md](./3_nfs_bind_tmpfs.md)

Concepts:

- [NFS, bind mounts, tmpfs](../concepts/storage/nfs_bind_tmpfs.md)
- [/etc/fstab](../concepts/storage/fstab.md)

Commands:

- [NFS (`exportfs`, `showmount`)](../commands/storage/nfs.md)
- [`mount`, `umount`](../commands/storage/mount_umount.md)

---

## Phase Capstone — "Resilient File Server"

Build a file server VM:

- 2 extra disks combined into an LVM VG.
- An xfs LV mounted at `/srv/data`.
- Daily LVM snapshot script, retained for 7 days.
- NFS exports to the lab subnet, with separate read-only and read-write shares.
- Monitoring script that warns when VG free space < 20%.
- Runbook: how to add another disk and grow the share **without downtime**.
