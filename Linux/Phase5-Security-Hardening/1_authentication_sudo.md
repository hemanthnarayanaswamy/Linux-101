# Authentication & sudo

**Roadmap: Week 30.** Manage how users authenticate, configure sudo precisely, and understand PAM at a useful depth.

## sudo and /etc/sudoers

`sudo` grants specific users the ability to run commands as another user (usually root). Rules live in `/etc/sudoers` and drop-ins under `/etc/sudoers.d/`.

> **Always edit with [`visudo`](../commands/users-groups-permissions/sudo_visudo.md)** — it syntax-checks before saving. A broken sudoers file can lock everyone out of root.

Syntax essentials:

```sudoers
# user  host = (runas) commands
deployer ALL = (root) NOPASSWD: /bin/systemctl restart myapp

# Aliases keep it readable
User_Alias  ADMINS = alice, bob
Cmnd_Alias  SERVICES = /bin/systemctl restart myapp, /bin/systemctl status myapp
ADMINS ALL = (root) SERVICES

# Defaults
Defaults logfile=/var/log/sudo.log       # audit every sudo call
```

Prefer **drop-in files** in `/etc/sudoers.d/` (one per role, git-managed) over editing the main file:

```bash
sudo visudo -f /etc/sudoers.d/deployer
```

- **`NOPASSWD`** skips the password prompt for the listed commands. Convenient for automation, but **dangerous**: if the command allows shell escapes (e.g. an editor, or a broad wildcard), it becomes a path to full root. Scope it to exact commands.

## PAM (Pluggable Authentication Modules)

PAM is the framework that decides *how* authentication happens for each service. See the concept note: [PAM](../concepts/security/pam.md).

- Per-service config in `/etc/pam.d/` (`sshd`, `sudo`, `login`).
- Four stack types: `auth`, `account`, `password`, `session`.
- Two modules the roadmap targets:
  - **`pam_faillock`** — lock an account after N failed attempts.
    ```
    auth required pam_faillock.so preauth deny=3 unlock_time=600   # 3 fails → 10 min lock
    ```
  - **`pam_pwquality`** — enforce password complexity.
    ```
    password required pam_pwquality.so minlen=12 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1
    ```

## Practice (roadmap exercises)

1. Create `deployer` who can run only `systemctl restart myapp`, no password prompt.
2. Configure `pam_faillock`: 3 failed attempts → 10-minute lockout. Test it.
3. Require password complexity via `pam_pwquality`: 12 chars, mixed case, digit, symbol.
4. Read `/etc/pam.d/sshd`; identify the auth modules in order and explain each.
5. Add `Defaults logfile=/var/log/sudo.log`, use sudo, read the log.

**Mini-project:** a git-managed `/etc/sudoers.d/` setup for 5 mock roles (admin, deployer, monitor, dba, readonly), each with role-appropriate privileges and comments.

**Self-check:** Why never edit `/etc/sudoers` without `visudo`? What does `NOPASSWD` mean and when is it dangerous?

## Related

- Concept: [PAM](../concepts/security/pam.md)
- Commands: [`sudo`, `visudo`](../commands/users-groups-permissions/sudo_visudo.md), [`passwd`](../commands/users-groups-permissions/passwd.md)
