# SSH Server Hardening (`sshd_config`)

Locking down the SSH **server** (`sshd`). Config: `/etc/ssh/sshd_config` (and drop-ins in `/etc/ssh/sshd_config.d/`). For the client side (keys, agent, tunnels) see [ssh keys, agent, config](./ssh_keys_agent_config.md) and the [SSH deep dive](../../concepts/ssh/ssh_deepdive.md).

## Essential Directives

```sshd_config
PasswordAuthentication no      # keys only — kills brute force
PermitRootLogin no             # no direct root login
Port 2222                      # non-standard port (obscurity, less log noise)
AllowUsers deployer admin      # only these users may SSH in
MaxAuthTries 3                 # attempts per connection before drop
MaxSessions 5                  # multiplexed sessions per connection
```

| Directive | Why |
|:----------|:----|
| `PasswordAuthentication no` | Passwords are guessable; keys are not |
| `PermitRootLogin no` | Force login as a user + sudo (accountability + smaller target) |
| `AllowUsers` / `AllowGroups` | Allowlist who can log in at all |
| `MaxAuthTries` | Auth attempts **within one TCP connection** |
| `MaxSessions` | Number of **sessions multiplexed over one connection** (different axis) |

> `PermitRootLogin` values: `yes` (worst) → `without-password`/`prohibit-password` (key-only root, better) → `no` (best — no root login at all).

## Apply & Verify

```bash
sudo sshd -t                   # syntax-check config BEFORE restarting
sudo systemctl restart ssh     # (or sshd)
sudo sshd -T                   # dump the EFFECTIVE running config
sudo sshd -T | grep -E 'passwordauthentication|permitrootlogin|maxauthtries'
```

> ⚠️ Keep a second SSH session open while changing sshd_config. If you lock yourself out, you can fix it from the still-open session.

## Locking Down a Key (`authorized_keys` options)

Prefix a key in `~/.ssh/authorized_keys` with restrictions:

```
from="1.2.3.4",command="/usr/local/bin/restricted",no-pty ssh-ed25519 AAAA... user@host
```

| Option | Effect |
|:-------|:-------|
| `from="1.2.3.4"` | Key only usable from that source IP/range |
| `command="…"` | Force this command; ignore what the client requested |
| `no-pty` | No interactive terminal (good for automation-only keys) |

## Related

- Rate-limiting brute force: [`fail2ban`](../security/fail2ban.md)
- Concept: [SSH deep dive](../../concepts/ssh/ssh_deepdive.md), [PAM](../../concepts/security/pam.md) (`/etc/pam.d/sshd`)
- [ssh keys, agent, config](./ssh_keys_agent_config.md)
