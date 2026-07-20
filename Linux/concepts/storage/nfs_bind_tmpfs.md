# NFS, Bind Mounts, and tmpfs

Three ways to present storage that aren't "a filesystem on a local disk."

## NFS (Network File System)

Share a directory from one machine (server) to others (clients) over the network — they mount it like a local filesystem.

```
Server (exports /srv/share)  ──network──►  Client (mounts it at /mnt/share)
```

- **Server:** package `nfs-kernel-server`; exports defined in `/etc/exports`; apply with `exportfs -ra`.
- **Client:** `mount -t nfs server:/srv/share /mnt/share`; make persistent in [`/etc/fstab`](./fstab.md) with type `nfs`.

### sync vs async

| Option | Behavior | Trade-off |
|:-------|:---------|:----------|
| `sync` | Server confirms a write only after it's on disk | Safer, slower — **production default** |
| `async` | Server confirms immediately, flushes later | Faster, but data loss risk on crash |

Production usually wants `sync`: it survives a server crash without silently losing writes clients think succeeded.

### Stale mounts

If the server disappears, clients accessing the mount can **hang** (NFS retries by design). Recover a stuck mount with a lazy unmount:

```bash
umount -l /mnt/share    # lazy: detach now, clean up when free
```

See [NFS commands](../../commands/storage/nfs.md).

## Bind Mounts

Make an existing directory appear at a second path — same underlying files, two locations.

```bash
mount --bind /var/log /home/user/logs        # both paths show the same files
mount -o remount,ro,bind /home/user/logs     # make the second view read-only
```

Uses: expose one subtree into a chroot/container, give a service a read-only view of shared data, or relocate a path without moving data.

## tmpfs

A filesystem that lives in **RAM** (and swap). Fast, and **everything in it vanishes on unmount/reboot**.

```bash
mount -t tmpfs -o size=1G tmpfs /mnt/ram
```

Uses: scratch space, caches, sensitive data you don't want written to disk. `/run` and often `/tmp` are already tmpfs on modern systems.

## Related

- [/etc/fstab](./fstab.md), [filesystems](./filesystems.md)
- Commands: [NFS (`exportfs`, `showmount`)](../../commands/storage/nfs.md), [`mount`, `umount`](../../commands/storage/mount_umount.md)
