# `pwd`, `ls`, and `cd`

These are the core navigation commands.

## `pwd`

Shows the current working directory.

```bash
pwd
```

Use it when you need to confirm exactly where you are before running a command that changes files.

## `ls`

Lists files and directories.

```bash
ls
ls -l
ls -la
ls -lh
ls -ltr
```

Common options:

| Option | Meaning |
|:-------|:--------|
| `-l` | Long listing with permissions, owner, size, time |
| `-a` | Show hidden files |
| `-h` | Human-readable sizes with `-l` |
| `-t` | Sort by modification time |
| `-r` | Reverse sort |

## `cd`

Changes directories.

```bash
cd /etc
cd ~
cd ..
cd -
```

| Path | Meaning |
|:-----|:--------|
| `/` | Root directory |
| `~` | Current user's home directory |
| `..` | Parent directory |
| `.` | Current directory |
| `-` | Previous directory |
