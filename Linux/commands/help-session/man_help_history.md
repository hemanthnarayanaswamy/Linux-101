# `man`, `help`, `--help`, `history`, `clear`, and `reset`

These commands help you discover syntax, review previous commands, and clean up your terminal.

## Manual and Help

```bash
man ls
man 5 passwd
help cd
ls --help
```

| Command | Use |
|:--------|:----|
| `man <command>` | Full manual page |
| `help <builtin>` | Help for shell builtins like `cd` |
| `<command> --help` | Quick usage summary |

Use `man 5 <topic>` for file formats, for example:

```bash
man 5 passwd
man 5 crontab
```

## Command History

```bash
history
!!       # Run previous command
!5       # Run command number 5
!ls      # Run last command starting with ls
```

## Terminal Cleanup

```bash
clear
reset
```

`clear` clears the visible screen. `reset` is stronger and is useful when the terminal display gets corrupted.
