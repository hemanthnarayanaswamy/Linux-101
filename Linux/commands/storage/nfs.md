# NFS Commands (`exportfs`, `showmount`)

Setting up an NFS server and client. See the concept note [NFS, bind mounts, tmpfs](../../concepts/storage/nfs_bind_tmpfs.md).

## Server Setup

```bash
sudo apt install nfs-kernel-server
sudo mkdir -p /srv/share
```

Define exports in `/etc/exports` — one line per shared directory:

```
# <dir>       <client>(options)
/srv/share    192.168.56.0/24(rw,sync,no_subtree_check)
/srv/readonly 192.168.56.0/24(ro,sync,no_subtree_check)
```

Common options:

| Option | Meaning |
|:-------|:--------|
| `rw` / `ro` | read-write / read-only |
| `sync` | confirm writes only once on disk (safe, production default) |
| `async` | confirm immediately (faster, risk of loss on crash) |
| `no_subtree_check` | skip subtree checks — recommended, avoids edge-case failures |
| `root_squash` | map remote root → `nobody` (default, security) |

Apply changes:

```bash
sudo exportfs -ra              # re-export all, reload /etc/exports
sudo exportfs -v               # show what's currently exported
sudo systemctl restart nfs-kernel-server
```

## Client Setup

```bash
sudo apt install nfs-common
showmount -e server-ip          # list what the server exports
sudo mount -t nfs server-ip:/srv/share /mnt/share
```

Make it persistent in [`/etc/fstab`](../../concepts/storage/fstab.md):

```
server-ip:/srv/share  /mnt/share  nfs  defaults  0  0
```

## Troubleshooting a Stale Mount

If the server goes away, the client can hang on the mount:

```bash
sudo umount -l /mnt/share       # lazy unmount to recover
```

## Related

- Concept: [NFS, bind mounts, tmpfs](../../concepts/storage/nfs_bind_tmpfs.md)
- [`mount`, `umount`](./mount_umount.md), [/etc/fstab](../../concepts/storage/fstab.md)
