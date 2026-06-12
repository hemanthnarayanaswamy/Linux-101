# `uniq`

`uniq` removes or counts adjacent duplicate lines.

Important: `uniq` only works on neighboring duplicate lines, so it is commonly used after `sort`.

## Common Usage

```bash
sort file.txt | uniq
sort file.txt | uniq -c
sort file.txt | uniq -d
```

| Option | Meaning |
|:-------|:--------|
| `-c` | Count occurrences |
| `-d` | Show only duplicates |
| `-u` | Show only unique lines |

## Log Example

Count repeated values:

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head
```

This is a common pattern for top IPs, top endpoints, and repeated errors.
