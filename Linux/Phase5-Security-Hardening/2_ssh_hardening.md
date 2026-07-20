# SSH Hardening

**Roadmap: Week 31.** Lock down `sshd`, use key-based auth exclusively, and rate-limit brute force.

Builds on the Phase 2 SSH material ([5_ssh_remote_access.md](../Phase2-Networking/5_ssh_remote_access.md), [SSH deep dive](../concepts/ssh/ssh_deepdive.md)). Full command reference: [SSH server hardening](../commands/ssh/sshd_hardening.md).

## Essential sshd_config

`/etc/ssh/sshd_config`:

```sshd_config
PasswordAuthentication no      # keys only — eliminates password brute force
PermitRootLogin no             # log in as a user, then sudo
Port 2222                      # non-standard port cuts log noise
AllowUsers deployer admin      # allowlist who may SSH at all
MaxAuthTries 3                 # auth attempts per connection
```

Apply and verify the **effective** config:

```bash
sudo sshd -t                                 # syntax check FIRST
sudo systemctl restart ssh
sudo sshd -T | grep -E 'passwordauthentication|permitrootlogin'
```

> ⚠️ Keep a second SSH session open while editing — if you lock yourself out you can recover from it.

- `MaxAuthTries` = attempts **within one connection**; `MaxSessions` = concurrent sessions **multiplexed over one connection** — different axes.
- `PermitRootLogin`: `no` (best) > `prohibit-password` (key-only root) > `yes` (worst).

## Restricting Keys

In `~/.ssh/authorized_keys`, prefix a key to constrain it:

```
from="1.2.3.4",command="/usr/local/bin/restricted",no-pty ssh-ed25519 AAAA... user@host
```

`from=` locks the source IP, `command=` forces a single command, `no-pty` blocks an interactive shell.

## Rate-Limiting Brute Force — fail2ban

`fail2ban` bans IPs after repeated auth failures. See [`fail2ban`](../commands/security/fail2ban.md).

```ini
# /etc/fail2ban/jail.local
[sshd]
enabled  = true
port     = 2222
maxretry = 3
findtime = 10m
bantime  = 1h
```

```bash
sudo fail2ban-client status sshd     # see banned IPs
```

## 2FA (optional)

`libpam-google-authenticator` adds TOTP as a second factor via PAM (`/etc/pam.d/sshd` + `ChallengeResponseAuthentication yes`). Worth doing once on a non-prod VM to see the pattern, then backing out.

## Practice (roadmap exercises)

1. Disable password auth and root login; verify with `sshd -T | grep -E 'passwordauthentication|permitrootlogin'`.
2. `AllowUsers deployer admin`; confirm other users are rejected.
3. Install fail2ban, SSH jail = 3-strikes-in-10-min → 1-hour ban; trigger it and watch the ban.
4. Lock a key with `from=…,command=…,no-pty`; test.
5. Set up TOTP 2FA on a non-prod VM, get it working, then back it out.

**Mini-project:** take your Phase 2 bastion, apply every hardening from this week, and write a "bastion hardening checklist" other engineers could follow.

**Self-check:** `MaxAuthTries` vs `MaxSessions`? Why is `PermitRootLogin without-password` safer than `yes` but worse than `no`?

## Related

- Commands: [SSH server hardening](../commands/ssh/sshd_hardening.md), [`fail2ban`](../commands/security/fail2ban.md)
- Concepts: [SSH deep dive](../concepts/ssh/ssh_deepdive.md), [PAM](../concepts/security/pam.md)
