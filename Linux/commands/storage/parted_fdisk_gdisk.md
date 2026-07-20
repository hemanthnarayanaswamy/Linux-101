# The `parted`, `fdisk`, and `gdisk` Commands

Tools that create and edit **partition tables** on a disk. See [block devices and partitions](../../concepts/storage/block_devices_partitions.md) for MBR vs GPT.

> ⚠️ These write directly to the disk's partition table. Operating on the wrong device destroys data. Double-check with `lsblk` first, and practice in a VM with a snapshot.

## Which Tool

| Tool | Best for | Interface |
|:-----|:---------|:----------|
| `parted` | Both MBR and GPT; scriptable | Interactive or one-shot CLI |
| `fdisk` | MBR (and modern versions: GPT too) | Interactive menu |
| `gdisk` | GPT specifically | Interactive menu (fdisk-like) |

## `parted` (GPT example, matches the roadmap)

```bash
sudo parted /dev/sdb              # interactive
(parted) mklabel gpt              # create a GPT partition table
(parted) mkpart primary ext4 1MiB 100%   # one partition spanning the disk
(parted) print                    # show the layout
(parted) quit
```

Non-interactive equivalent:

```bash
sudo parted -s /dev/sdb mklabel gpt
sudo parted -s /dev/sdb mkpart primary ext4 1MiB 100%
```

## `fdisk`

```bash
sudo fdisk /dev/sdb
# n = new partition, p = print, d = delete, t = type, w = write & quit, q = quit no-save
sudo fdisk -l          # list all partition tables (non-interactive)
```

## `gdisk`

Same menu style as fdisk but GPT-native:

```bash
sudo gdisk /dev/sdb
# n = new, p = print, w = write, q = quit
```

## After Partitioning

The kernel may need to re-read the table, then you create a filesystem:

```bash
sudo partprobe /dev/sdb                  # re-read partition table
sudo mkfs.ext4 /dev/sdb1                  # see mkfs_fsck_tune2fs.md
```

## Related

- Concept: [block devices and partitions](../../concepts/storage/block_devices_partitions.md)
- [`lsblk`, `blkid`](./lsblk_blkid.md), [LVM tools](./lvm.md), [`mkfs`, `fsck`, `tune2fs`](./mkfs_fsck_tune2fs.md)
