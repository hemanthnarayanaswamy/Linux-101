# NFS, Bind Mounts, tmpfs

**Roadmap: Week 28.** Share storage across machines, and use bind mounts and tmpfs for special cases.

See the concept note [NFS, bind mounts, tmpfs](../concepts/storage/nfs_bind_tmpfs.md).

## NFS — Network File System

Share a directory from a server to clients over the network.

### Server

```bash
sudo apt install nfs-kernel-server
sudo mkdir -p /srv/share
```

`/etc/exports`:

```
/srv/share    192.168.56.0/24(rw,sync,no_subtree_check)
```

```bash
sudo exportfs -ra                       # apply exports
sudo systemctl restart nfs-kernel-server
```

### Client

```bash
showmount -e server-ip                  # what does it export?
sudo mount -t nfs server-ip:/srv/share /mnt/share
```

Persist in [`/etc/fstab`](../concepts/storage/fstab.md): `server-ip:/srv/share  /mnt/share  nfs  defaults  0  0`.

### sync vs async

`sync` confirms writes only once on disk (safe, **production default**); `async` confirms immediately (faster, risk of loss on crash).

### Stale mount recovery

If the server disappears, the client hangs on the mount:

```bash
sudo umount -l /mnt/share               # lazy unmount
```

Commands: [NFS (`exportfs`, `showmount`)](../commands/storage/nfs.md).

## Bind Mounts

Make an existing directory appear at a second path (same files, two locations):

```bash
sudo mount --bind /var/log /home/user/logs
sudo mount -o remount,ro,bind /home/user/logs   # read-only view
```

Good for: exposing a subtree into a chroot/container, giving a service a read-only view, relocating a path without moving data.

## tmpfs — RAM-Backed

```bash
sudo mount -t tmpfs -o size=256M tmpfs /mnt/ram   # contents vanish on unmount/reboot
```

Good for: scratch space, caches, sensitive data you don't want on disk.

## Practice (roadmap exercises)

1. NFS-export `/srv/share` to your subnet, mount from another VM, test read/write.
2. Make the mount persistent in `/etc/fstab`, reboot, verify.
3. Bind-mount `/var/log` to `/home/user/logs` read-only; `cat` a logfile through both paths.
4. Mount a 256MB tmpfs; watch files disappear on unmount.
5. Diagnose a stale NFS mount: stop the server, watch the client hang, recover with `umount -l`.

**Mini-project:** shared scratch space for a build farm — one NFS directory used by two client VMs as a build cache; document permissions, exports, mount options; test concurrent writes.

**Self-check:** Why is `sync` slower than `async`, and which does production want? What's a bind mount good for?

## Related

- Concept: [NFS, bind mounts, tmpfs](../concepts/storage/nfs_bind_tmpfs.md), [/etc/fstab](../concepts/storage/fstab.md)
- Commands: [NFS (`exportfs`, `showmount`)](../commands/storage/nfs.md), [`mount`, `umount`](../commands/storage/mount_umount.md)
