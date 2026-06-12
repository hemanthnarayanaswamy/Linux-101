# `cut`

`cut` extracts columns or character ranges from text.

## Common Usage

```bash
cut -d ':' -f 1 /etc/passwd
cut -d ',' -f 1,3 data.csv
cut -c 1-10 file.txt
```

| Option | Meaning |
|:-------|:--------|
| `-d` | Delimiter |
| `-f` | Field number |
| `-c` | Character range |

## SRE/DevOps Examples

List usernames:

```bash
cut -d ':' -f 1 /etc/passwd
```

Extract first column from a space-normalized stream:

```bash
ps aux | tr -s ' ' | cut -d ' ' -f 1
```

For complex parsing, prefer `awk`.
