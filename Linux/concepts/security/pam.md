# PAM (Pluggable Authentication Modules)

PAM is the framework Linux uses to decide **how** authentication happens. Instead of each program (login, sshd, sudo) implementing its own password checks, they all call PAM, and PAM runs a configurable **stack** of modules.

## Where It Lives

- Per-service config: `/etc/pam.d/<service>` (e.g. `/etc/pam.d/sshd`, `/etc/pam.d/sudo`, `/etc/pam.d/login`).
- Shared fragments included by many services: `/etc/pam.d/common-auth`, `common-password`, etc. (Debian/Ubuntu).

## The Four Stack Types

Each line in a PAM config belongs to one **type** (management group):

| Type | Answers the question |
|:-----|:---------------------|
| `auth` | Are you who you say you are? (passwords, keys, tokens) |
| `account` | Is this account allowed to log in right now? (expiry, time, lockout) |
| `password` | Rules for *changing* a credential (complexity) |
| `session` | What to set up/tear down around the session (mounts, limits, logging) |

## Control Flags

The second field controls how a module's result affects the stack:

| Flag | Effect |
|:-----|:-------|
| `required` | Must pass; failure remembered but the stack continues (don't reveal which module failed) |
| `requisite` | Must pass; failure stops the stack immediately |
| `sufficient` | Success short-circuits the stack (if nothing `required` already failed) |
| `optional` | Result usually ignored unless it's the only module |

```
# /etc/pam.d/example
auth     required   pam_faillock.so preauth
auth     [success=1 default=bad] pam_unix.so
auth     [default=die]  pam_faillock.so authfail
account  required   pam_unix.so
password required   pam_pwquality.so retry=3
session  required   pam_limits.so
```

## Two Modules Worth Knowing

- **`pam_faillock`** — lock an account after N failed attempts for a period (brute-force defense).

  ```
  auth required pam_faillock.so preauth deny=3 unlock_time=600   # 3 tries → 10 min lockout
  ```
  Inspect/reset: `faillock --user alice`, `faillock --user alice --reset`.

- **`pam_pwquality`** — enforce password complexity (in `common-password` or `/etc/security/pwquality.conf`).

  ```
  password required pam_pwquality.so minlen=12 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1
  ```
  (12 chars, at least one upper, lower, digit, symbol.)

> ⚠️ A broken `/etc/pam.d/sshd` or `common-auth` can lock **everyone** out. Always keep a second root session open while editing PAM.

## Related

- Commands: [`sudo`, `visudo`](../../commands/users-groups-permissions/sudo_visudo.md), [`passwd`](../../commands/users-groups-permissions/passwd.md)
- [DAC vs MAC](./dac_vs_mac.md)
