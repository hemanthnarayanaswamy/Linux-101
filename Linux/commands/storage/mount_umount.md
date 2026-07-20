# The `mount` and `umount` Commands

Attach a filesystem to a directory in the tree (`mount`) and detach it (`umount`). See [/etc/fstab](../../concepts/storage/fstab.md) for making mounts persistent.

## Basic Mounting

```bash
sudo mount /dev/sdb1 /mnt/data              # mount a device
sudo mount -t ext4 /dev/sdb1 /mnt/data      # specify the type
sudo mount -o ro,noatime /dev/sdb1 /mnt/data # with options
sudo umount /mnt/data                        # unmount
sudo umount -l /mnt/data                      # lazy unmount (for stuck/NFS mounts)
```

## Inspecting Mounts

```bash
mount                  # everything currently mounted (verbose)
findmnt                # mounts as a readable tree
findmnt /mnt/data      # just one mount
mount | grep sdb1      # check a device's options (e.g. confirm noatime)
```

## fstab Integration

```bash
sudo mount -a          # mount everything in /etc/fstab (test after editing it)
sudo mount /mnt/data   # mount a single fstab entry by its mountpoint
findmnt --verify       # sanity-check fstab before rebooting
```

## Remount (change options without unmounting)

```bash
sudo mount -o remount,ro /mnt/data          # flip to read-only in place
sudo mount -o remount,rw /                   # recover root in emergency mode
```

## Bind Mounts

Make an existing directory appear at a second path (see [NFS, bind, tmpfs](../../concepts/storage/nfs_bind_tmpfs.md)):

```bash
sudo mount --bind /var/log /home/user/logs
sudo mount -o remount,ro,bind /home/user/logs   # make the second view read-only
```

## tmpfs (RAM-backed)

```bash
sudo mount -t tmpfs -o size=256M tmpfs /mnt/ram   # files here vanish on unmount
```

## NFS

```bash
sudo mount -t nfs server:/srv/share /mnt/share    # see nfs.md
```

## Related

- Concepts: [/etc/fstab](../../concepts/storage/fstab.md), [NFS, bind mounts, tmpfs](../../concepts/storage/nfs_bind_tmpfs.md)
- [NFS commands](./nfs.md), [`lsblk`, `blkid`](./lsblk_blkid.md)
