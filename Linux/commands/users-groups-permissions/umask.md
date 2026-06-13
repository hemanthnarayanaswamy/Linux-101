# `umask`

`umask` controls the default permissions removed from new files and directories.

Default maximums:

```text
files       666
directories 777
```

The umask subtracts permissions from those defaults.

## View Current Umask

```bash
umask
```

## Common Values

| Umask | New Files | New Directories | Meaning |
|:------|:----------|:----------------|:--------|
| `022` | `644` | `755` | Others can read, not write |
| `027` | `640` | `750` | Group can read, others blocked |
| `077` | `600` | `700` | Private by default |
| `002` | `664` | `775` | Group-friendly shared work |

Temporary change:

```bash
umask 027
```

Persistent changes usually go in shell or login configuration files.
