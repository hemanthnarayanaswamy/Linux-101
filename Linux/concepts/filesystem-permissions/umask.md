# Umask

`umask` controls default permissions for newly created files and directories.

Linux starts from default maximums:

```text
files       666
directories 777
```

Then it removes the permissions specified by the umask.

## Examples

| Umask | New File | New Directory | Meaning |
|:------|:---------|:--------------|:--------|
| `022` | `644` | `755` | Common default |
| `027` | `640` | `750` | Private from others |
| `077` | `600` | `700` | User-private |
| `002` | `664` | `775` | Good for shared groups |

## View Or Change

```bash
umask
umask 027
```

Temporary changes affect the current shell and child processes.

Persistent changes usually go in:

```text
~/.profile
~/.bashrc
/etc/profile
```

## Important Point

`umask` only removes permissions. It never adds permissions.

That is why new files usually do not become executable automatically.
