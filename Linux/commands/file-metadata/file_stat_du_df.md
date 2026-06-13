# `file`, `stat`, `du`, and `df`

These commands tell you what a file is and how much space filesystems use.

## `file`

Detects file type.

```bash
file script.sh
file image.png
file /bin/ls
```

## `stat`

Shows detailed metadata.

```bash
stat file.txt
```

Important fields:

| Field | Meaning |
|:------|:--------|
| Size | File size in bytes |
| Inode | Filesystem object ID |
| Access | Permission mode |
| Modify | File content changed |
| Change | Metadata changed |

## `du`

Shows disk usage for files/directories.

```bash
du -sh .
du -sh /var/log/*
```

## `df`

Shows filesystem capacity.

```bash
df -h
df -i
```

Use `df -h` for disk space and `df -i` for inode usage.
