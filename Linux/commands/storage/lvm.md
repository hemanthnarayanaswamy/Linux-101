# LVM Commands

Tools to build and grow LVM storage. See the concept note [LVM](../../concepts/storage/lvm.md) for PV/VG/LV.

Commands are grouped by layer: `pv*` (physical volumes), `vg*` (volume groups), `lv*` (logical volumes). Most have a `*create`, `*display`, `*s` (summary), `*extend`, `*remove` form.

## Build the Stack (bottom → top)

```bash
# 1. Initialize partitions as physical volumes
sudo pvcreate /dev/sdb1 /dev/sdc1

# 2. Create a volume group from one or more PVs
sudo vgcreate vg0 /dev/sdb1

# 3. Carve a logical volume out of the VG
sudo lvcreate -L 1G -n data vg0          # 1GB LV named "data"
#   → appears as /dev/vg0/data and /dev/mapper/vg0-data

# 4. Format and mount it
sudo mkfs.ext4 /dev/vg0/data
sudo mount /dev/vg0/data /mnt/data
```

## Inspect

```bash
sudo pvs      /  pvdisplay      # physical volumes
sudo vgs      /  vgdisplay      # volume groups — watch the VFree column
sudo lvs      /  lvdisplay      # logical volumes
```

## Grow

Add a new disk into the VG:

```bash
sudo pvcreate /dev/sdc1
sudo vgextend vg0 /dev/sdc1     # VG now has more free space
```

Grow a logical volume (and its filesystem):

```bash
sudo lvextend -L +1G /dev/vg0/data     # add 1GB to the LV
sudo resize2fs /dev/vg0/data           # then grow the ext4 fs (xfs: xfs_growfs)

# one-shot: grow LV + filesystem together
sudo lvextend -r -L +1G /dev/vg0/data
sudo lvextend -r -l +100%FREE /dev/vg0/data   # consume all remaining VG space
```

> Growing the **LV** does not grow the **filesystem** — that's a separate step (`resize2fs`/`xfs_growfs`), unless you use `-r`. This is the Week 26 self-check.

## Related

- Concept: [LVM](../../concepts/storage/lvm.md)
- [`mkfs`, `fsck`, `resize2fs`](./mkfs_fsck_tune2fs.md), [`lsblk`, `blkid`](./lsblk_blkid.md)
