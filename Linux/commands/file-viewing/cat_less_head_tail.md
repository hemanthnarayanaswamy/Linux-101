# `cat`, `less`, `head`, and `tail`

These commands read file content from the terminal.

## `cat`

Prints a whole file.

```bash
cat file.txt
cat /etc/os-release
```

Best for short files.

## `less`

Opens a file page by page.

```bash
less /var/log/syslog
```

Useful keys:

| Key | Meaning |
|:----|:--------|
| `Space` | Next page |
| `b` | Previous page |
| `/text` | Search |
| `n` | Next search match |
| `q` | Quit |

## `head`

Shows the start of a file.

```bash
head file.txt
head -n 50 file.txt
```

## `tail`

Shows the end of a file.

```bash
tail file.txt
tail -n 100 file.txt
tail -f /var/log/syslog
```

`tail -f` follows new lines as they are written. It is useful for watching logs live.
