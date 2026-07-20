# Disks, Partitions, LVM

**Roadmap: Week 26.** Partition a disk safely, and create and grow LVM volumes.

## Block Devices

Storage shows up under `/dev/` as block devices. See [block devices and partitions](../concepts/storage/block_devices_partitions.md).

```bash
lsblk         # disks → partitions → mounts, as a tree
lsblk -f      # + filesystem type + UUID
blkid         # UUID / TYPE of each device (for fstab)
```

`/dev/sda` (SATA/SCSI/USB) vs `/dev/nvme0n1` (NVMe); partitions are `sda1`, `nvme0n1p1`.

## Partition Tables: MBR vs GPT

| | MBR | GPT |
|:--|:--|:--|
| Max size / partitions | 2 TB / 4 primary | ~unlimited / 128 |
| Firmware | Legacy BIOS | UEFI |
| Use | Legacy only | **Default for new disks** |

Partition with [`parted`, `fdisk`, `gdisk`](../commands/storage/parted_fdisk_gdisk.md):

```bash
sudo parted -s /dev/sdb mklabel gpt
sudo parted -s /dev/sdb mkpart primary ext4 1MiB 100%
```

## LVM: PV → VG → LV

LVM sits between disks and filesystems so you can resize and span storage. See [LVM concept](../concepts/storage/lvm.md).

```
disk/partition → PV (pvcreate) → VG (vgcreate) → LV (lvcreate) → mkfs → mount
```

```bash
sudo pvcreate /dev/sdb1                    # physical volume
sudo vgcreate vg0 /dev/sdb1                # volume group
sudo lvcreate -L 1G -n data vg0            # logical volume → /dev/vg0/data
sudo mkfs.ext4 /dev/vg0/data
sudo mount /dev/vg0/data /mnt/data
```

Full command reference: [LVM tools](../commands/storage/lvm.md).

## /etc/fstab — Persistent Mounts by UUID

Always mount by **UUID**, not `/dev/sdX` (device names can change on reboot). See [/etc/fstab](../concepts/storage/fstab.md).

```
UUID=<from blkid>   /mnt/data   ext4   defaults,noatime   0   2
```

```bash
sudo mount -a          # test fstab WITHOUT rebooting (do this before you reboot)
```

## Growing Storage (the key skill)

Growing an LV and growing its filesystem are **two steps**:

```bash
sudo lvextend -L +1G /dev/vg0/data     # 1. grow the LV
sudo resize2fs /dev/vg0/data           # 2. grow the ext4 filesystem
# or in one go:  sudo lvextend -r -L +1G /dev/vg0/data
df -h /mnt/data                        # verify
```

Add a whole new disk to the pool:

```bash
sudo pvcreate /dev/sdc1
sudo vgextend vg0 /dev/sdc1            # VG gains the new disk's space
```

## Practice (roadmap exercises)

1. (VM) Add a 5GB disk → identify with `lsblk` → GPT partition with `parted`.
2. PV → VG → 1GB LV → format ext4 → mount.
3. Add to `/etc/fstab` by UUID → reboot → verify it mounted.
4. `lvextend +1G` then `resize2fs`, confirm with `df -h`.
5. Add a second disk to the VG and extend an LV onto it.

**Mini-project:** plan/implement a Postgres storage layout — `/` (10GB), `/var/log` (5GB), `/var/lib/postgresql` (20GB), all on LVM, and document why.

**Self-check:** Why UUIDs in fstab instead of device names? Difference between extending an LV and resizing the filesystem?

## Related

- Concepts: [block devices and partitions](../concepts/storage/block_devices_partitions.md), [LVM](../concepts/storage/lvm.md), [/etc/fstab](../concepts/storage/fstab.md)
- Commands: [`lsblk`, `blkid`](../commands/storage/lsblk_blkid.md), [`parted`, `fdisk`, `gdisk`](../commands/storage/parted_fdisk_gdisk.md), [LVM tools](../commands/storage/lvm.md)
