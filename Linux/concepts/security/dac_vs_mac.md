# DAC vs MAC (AppArmor & SELinux)

Two layers of access control decide whether a process may touch a resource. Standard Unix permissions are **DAC**; AppArmor/SELinux add **MAC** on top.

## DAC — Discretionary Access Control

The permission model you already know: owner/group/other, `chmod`, `chown`, ACLs.

- **Discretionary** = the *owner* of a resource decides who can access it. Root (and file owners) can hand out access at will.
- Enforced against the **user identity**. If you own a file, you can `chmod 777` it and anyone can read it.

Limitation: DAC trusts processes running as a user to only do what that user should. A compromised nginx running as `www-data` can read *anything* `www-data` can read — your whole web tree, and more.

## MAC — Mandatory Access Control

A system-wide policy, set by the administrator, that **even root cannot override** on a whim. Access is enforced against a **label/profile** attached to the *program*, not just the user.

- **Mandatory** = the policy is fixed by the security policy, not the file owner.
- Confines a process to *exactly* what it's declared to need. A compromised nginx can only touch the paths its profile allows — `chmod 777` on a file outside the profile still won't let it in.

> This is the key exam point: on a MAC-enforcing system, `chmod 777` may **not** fix an "access denied" — the MAC layer can still block it. Check the security layer, not just the mode bits.

## The Two Implementations

| | AppArmor | SELinux |
|:--|:--|:--|
| Default on | Ubuntu / Debian / SUSE | RHEL / Fedora / CentOS |
| Policy keyed on | **File paths** | **Labels/contexts** on files & processes |
| Profiles live in | `/etc/apparmor.d/` | policy modules + file contexts |
| Ease | Simpler to read/write | More powerful, steeper curve |
| Modes | enforce / complain (audit-only) | Enforcing / Permissive / Disabled |

### AppArmor essentials

```bash
sudo aa-status                 # profiles loaded + enforce/complain
sudo aa-complain /path/to/bin  # audit-only (log would-be denials, don't block)
sudo aa-enforce  /path/to/bin  # enforce the profile
```
Denials show up in the journal / `dmesg` as `apparmor="DENIED"`.

### SELinux essentials

```bash
getenforce                     # Enforcing / Permissive / Disabled
sudo setenforce 0              # switch to Permissive (temporary)
ls -Z /var/www/html            # show file security contexts (user:role:type:level)
sudo audit2allow -a            # turn recent denials into a candidate policy
```

- **Enforcing** = policy is applied and violations blocked.
- **Permissive** = violations are **logged but allowed** — for debugging/policy authoring, not security.

## Related

- Commands: [AppArmor](../../commands/security/apparmor.md), [SELinux](../../commands/security/selinux.md)
- Concepts: [special permission bits](../filesystem-permissions/special_bits.md), [PAM](./pam.md)
