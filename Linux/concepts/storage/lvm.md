# LVM (Logical Volume Manager)

LVM adds a flexible layer between physical disks and filesystems, so you can resize, span, and snapshot storage without repartitioning.

## The Three Layers

```
Physical disks/partitions   /dev/sdb1   /dev/sdc1
        ↓  pvcreate
PV  (Physical Volume)        each disk/partition initialized for LVM
        ↓  vgcreate / vgextend
VG  (Volume Group)           a pool that combines one or more PVs
        ↓  lvcreate
LV  (Logical Volume)         a "virtual partition" carved from the VG
        ↓  mkfs
Filesystem                   ext4 / xfs on the LV, then mounted
```

| Term | What it is | Analogy |
|:-----|:-----------|:--------|
| **PV** — Physical Volume | A disk or partition prepared for LVM (`pvcreate`) | A raw ingredient |
| **VG** — Volume Group | A pool of storage made of one or more PVs (`vgcreate`) | The mixing bowl |
| **LV** — Logical Volume | A slice of the VG you format and mount (`lvcreate`) | A portion served |

## Why LVM Over Plain Partitions

- **Resize online:** grow an LV and its filesystem while mounted — no reboot, no repartition.
- **Span disks:** a single LV can stretch across multiple physical disks in the VG.
- **Add capacity later:** drop a new disk in, `pvcreate` + `vgextend`, and the VG grows.
- **Snapshots:** point-in-time copies for backups (used in the Phase capstone).

## Grow an LV (the key workflow)

Two distinct steps — growing the LV does **not** grow the filesystem:

```bash
lvextend -L +1G /dev/vg0/data     # 1. grow the logical volume
resize2fs /dev/vg0/data           # 2. grow the ext4 filesystem to fill it
# (xfs uses: xfs_growfs /mount/point)
```

> Shortcut: `lvextend -r -L +1G /dev/vg0/data` grows the LV **and** the filesystem in one command (`-r` = resize fs).

## Inspecting

```bash
pvs / pvdisplay      # physical volumes
vgs / vgdisplay      # volume groups (watch VFree here)
lvs / lvdisplay      # logical volumes
```

## Related

- [Block devices and partitions](./block_devices_partitions.md), [filesystems](./filesystems.md)
- Commands: [LVM tools](../../commands/storage/lvm.md), [`mkfs`, `fsck`, `resize2fs`](../../commands/storage/mkfs_fsck_tune2fs.md)
