# `touch`, `mkdir`, `cp`, `mv`, `rm`, and `rmdir`

These are the basic file and directory operation commands.

## Create

```bash
touch file.txt
mkdir logs
mkdir -p project/src/app
```

| Command | Use |
|:--------|:----|
| `touch file` | Create empty file or update timestamp |
| `mkdir dir` | Create directory |
| `mkdir -p path/to/dir` | Create parent directories if needed |

## Copy

```bash
cp file.txt backup.txt
cp -r app app-backup
cp -p config.conf config.conf.bak
```

| Option | Meaning |
|:-------|:--------|
| `-r` | Copy directories recursively |
| `-p` | Preserve mode, ownership, timestamps |
| `-v` | Verbose output |

## Move or Rename

```bash
mv old.txt new.txt
mv file.txt /tmp/
mv app/ /opt/app/
```

`mv` renames when source and destination are in the same directory. It moves when the destination is another directory.

## Remove

```bash
rm file.txt
rm -i file.txt
rm -r old-directory
rmdir empty-directory
```

Be careful with:

```bash
rm -rf path
```

It removes recursively and forcefully. Verify the path before running it.
