# Block Devices and Partitions

## Block Devices

A **block device** is storage the kernel reads/writes in fixed-size blocks (as opposed to character devices). Disks, partitions, and LVM volumes are all block devices, exposed under `/dev/`.

| Name pattern | Meaning |
|:-------------|:--------|
| `/dev/sda`, `/dev/sdb` | SATA/SCSI/USB disks (whole disk) |
| `/dev/sda1`, `/dev/sda2` | Partitions on `sda` |
| `/dev/nvme0n1` | First NVMe disk |
| `/dev/nvme0n1p1` | Partition 1 on that NVMe disk (note the `p`) |
| `/dev/mapper/vg-lv` | An LVM logical volume |

Inspect them:

```bash
lsblk            # tree of disks → partitions → mounts
blkid            # UUID and filesystem type of each block device
```

See [`lsblk`, `blkid`](../../commands/storage/lsblk_blkid.md).

## Partition Tables: MBR vs GPT

A **partition table** at the start of a disk records how the disk is divided. Two formats:

| | MBR (msdos) | GPT |
|:--|:--|:--|
| Max disk size | 2 TB | 9.4 ZB (effectively unlimited) |
| Max primary partitions | 4 (extended needed for more) | 128 |
| Redundancy | Single copy | Header + backup at end of disk |
| Firmware | Legacy BIOS | UEFI |
| Verdict | Legacy | **Default for anything new** |

Use **GPT** for new disks unless you have a specific legacy-BIOS reason. Tools: [`parted`, `fdisk`, `gdisk`](../../commands/storage/parted_fdisk_gdisk.md).

## From Disk to Mounted Filesystem

The typical stack, bottom to top:

```
physical disk        /dev/sdb
  → partition        /dev/sdb1        (parted / fdisk / gdisk)
    → [optional LVM] PV → VG → LV      (see lvm.md)
      → filesystem   mkfs.ext4 / mkfs.xfs
        → mount      /etc/fstab or mount command
```

## Related

- [LVM](./lvm.md), [filesystems](./filesystems.md), [/etc/fstab](./fstab.md)
- Commands: [`lsblk`, `blkid`](../../commands/storage/lsblk_blkid.md), [`parted`, `fdisk`, `gdisk`](../../commands/storage/parted_fdisk_gdisk.md)
