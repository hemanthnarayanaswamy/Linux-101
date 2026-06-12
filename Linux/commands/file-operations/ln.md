# `ln`

`ln` creates links.

## Hard Link

```bash
ln original.txt hardlink.txt
```

A hard link is another filename pointing to the same inode. If one name is deleted, the data remains available through the other name.

## Symbolic Link

```bash
ln -s /etc/hostname myhost
```

A symbolic link points to a path. If the target path disappears, the symlink becomes broken.

## Useful Options

| Option | Meaning |
|:-------|:--------|
| `-s` | Create symbolic link |
| `-f` | Remove existing destination first |
| `-n` | Treat symlink destination as a normal file |
| `-v` | Verbose output |

Common deployment pattern:

```bash
ln -sfn app-v2 current
```

This makes `current` point to the new release directory.
