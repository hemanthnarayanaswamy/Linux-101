# `sudo` and `visudo`

`sudo` runs commands with elevated privileges. `visudo` safely edits sudo policy.

## `sudo`

```bash
sudo apt update
sudo systemctl restart nginx
sudo -l
```

| Command | Meaning |
|:--------|:--------|
| `sudo command` | Run command as root |
| `sudo -u user command` | Run command as another user |
| `sudo -l` | List allowed sudo commands |

## `visudo`

Edit sudoers safely:

```bash
sudo visudo
```

`visudo` checks syntax before saving. A broken sudoers file can lock out admin access.

Example rule:

```text
alice ALL=(root) NOPASSWD: /bin/systemctl restart nginx
```

Prefer files under:

```text
/etc/sudoers.d/
```

for custom rules.
