# AppArmor Commands

AppArmor is Ubuntu's **MAC** system: per-program **profiles** (keyed on file paths) that confine what a binary may do. See [DAC vs MAC](../../concepts/security/dac_vs_mac.md).

## Status & Profiles

```bash
sudo aa-status               # profiles loaded, which are enforced vs complain
sudo apparmor_status         # same thing
```

Profiles live in `/etc/apparmor.d/` (e.g. `usr.sbin.nginx`). The path-based naming replaces `/` with `.`.

## Modes

Each profile is in one of two modes:

| Mode | Behavior | Command |
|:-----|:---------|:--------|
| **enforce** | Violations are blocked | `sudo aa-enforce /path/to/bin` |
| **complain** | Violations are **logged but allowed** (for building/debugging a profile) | `sudo aa-complain /path/to/bin` |

```bash
sudo aa-complain /usr/sbin/nginx     # audit-only: see what it would block
# ...exercise the app to generate log entries...
sudo aa-logprof                       # interactively update the profile from logs
sudo aa-enforce /usr/sbin/nginx      # lock it down
```

## Reading Denials

Blocked actions log as `apparmor="DENIED"`:

```bash
sudo journalctl -k | grep -i apparmor
sudo dmesg | grep -i denied
```

This is how you answer "is nginx failing because of file permissions or AppArmor?" — a DAC failure is a normal `EACCES`; a MAC failure shows up as an AppArmor `DENIED` line.

## Manage

```bash
sudo systemctl reload apparmor            # reload all profiles
sudo apparmor_parser -r /etc/apparmor.d/usr.sbin.nginx   # reload one
sudo ln -s /etc/apparmor.d/usr.sbin.nginx /etc/apparmor.d/disable/   # disable one
```

## Related

- Concept: [DAC vs MAC](../../concepts/security/dac_vs_mac.md)
- [SELinux](./selinux.md) (the RHEL equivalent)
