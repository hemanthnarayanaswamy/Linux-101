# `chmod`, `chown`, and `chgrp`

These commands change permissions and ownership.

## `chmod`

Changes file mode permissions.

```bash
chmod 644 file.txt
chmod 755 script.sh
chmod u+x script.sh
chmod g+w shared/
```

Common modes:

| Mode | Meaning |
|:-----|:--------|
| `600` | Owner read/write only |
| `644` | Owner write, everyone read |
| `700` | Owner full access only |
| `755` | Owner full, others read/execute |

## `chown`

Changes owner and optionally group.

```bash
sudo chown alice file.txt
sudo chown alice:developers file.txt
sudo chown -R alice:developers project/
```

## `chgrp`

Changes group ownership.

```bash
sudo chgrp developers file.txt
sudo chgrp -R developers /srv/projects
```

Use recursive options carefully and verify the target path first.
