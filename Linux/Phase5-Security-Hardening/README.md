# Phase 5 - Security & Hardening

This is Phase 6 (Security & Hardening) of the roadmap, kept as Phase 5 in this repo because the roadmap's Phase 2 (Shell Scripting & Automation) was skipped.

**Phase outcome:** lock down a production server — SSH, sudo, MAC, audit, intrusion detection.

Primary roadmap sources:

- [linux-mastery-roadmap.md](../../resources/roadMap/linux-mastery-roadmap.md)
- [linux-mastery-roadmap-detailed.md](../../resources/roadMap/linux-mastery-roadmap-detailed.md)

## Quick Index

| Order | Topic | Main Note |
|---:|:------|:----------|
| 1 | Authentication & sudo | [1_authentication_sudo.md](./1_authentication_sudo.md) |
| 2 | SSH hardening | [2_ssh_hardening.md](./2_ssh_hardening.md) |
| 3 | Mandatory access control (AppArmor/SELinux) | [3_mandatory_access_control.md](./3_mandatory_access_control.md) |
| 4 | Audit & intrusion detection | [4_audit_intrusion_detection.md](./4_audit_intrusion_detection.md) |

## Reference Indexes

| Area | Index |
|:-----|:------|
| Commands | [Linux commands index](../commands/README.md) |
| Concepts | [Linux concepts index](../concepts/README.md) |

---

## 1. Authentication & sudo

`/etc/sudoers` syntax and drop-ins, `visudo`, `NOPASSWD`, and PAM (stack types, `pam_faillock`, `pam_pwquality`).

Main note:

- [1_authentication_sudo.md](./1_authentication_sudo.md)

Concepts:

- [PAM](../concepts/security/pam.md)

Commands:

- [`sudo`, `visudo`](../commands/users-groups-permissions/sudo_visudo.md)
- [`passwd`](../commands/users-groups-permissions/passwd.md)

---

## 2. SSH Hardening

`sshd_config` essentials, key restrictions in `authorized_keys`, `sshd -T`, fail2ban, and optional 2FA.

Main note:

- [2_ssh_hardening.md](./2_ssh_hardening.md)

Concepts:

- [SSH deep dive](../concepts/ssh/ssh_deepdive.md)
- [PAM](../concepts/security/pam.md)

Commands:

- [SSH server hardening (`sshd_config`)](../commands/ssh/sshd_hardening.md)
- [`fail2ban`](../commands/security/fail2ban.md)
- [ssh keys, agent, config](../commands/ssh/ssh_keys_agent_config.md)

---

## 3. Mandatory Access Control: AppArmor / SELinux

DAC vs MAC, AppArmor profiles and modes (Ubuntu), SELinux contexts and modes (RHEL), diagnosing denials.

Main note:

- [3_mandatory_access_control.md](./3_mandatory_access_control.md)

Concepts:

- [DAC vs MAC](../concepts/security/dac_vs_mac.md)

Commands:

- [AppArmor](../commands/security/apparmor.md)
- [SELinux](../commands/security/selinux.md)

---

## 4. Audit & Intrusion Detection

auditd rules and searching, file integrity with AIDE, login tracking, and automated audits with Lynis.

Main note:

- [4_audit_intrusion_detection.md](./4_audit_intrusion_detection.md)

Concepts:

- [Auditing & intrusion detection](../concepts/security/auditing.md)

Commands:

- [`auditd` (`auditctl`, `ausearch`, `aureport`)](../commands/security/auditd.md)
- [AIDE & Lynis](../commands/security/aide_lynis.md)
- [Login tracking (`last`, `lastb`, `lastlog`)](../commands/security/login_tracking.md)

---

## Phase Capstone — "CIS-Style Hardened Server Build"

Build a hardened Ubuntu web-server VM against a CIS-Benchmark-style checklist:

- SSH hardening (§2).
- sudoers config (§1).
- AppArmor profiles enforced (§3).
- auditd rules (§4).
- Firewall ([Phase 2 §4](../Phase2-Networking/4_Firewalls.md)).
- AIDE baseline.
- Lynis score ≥ 75.

Deliverables: a git repo with every config + a bash (or Ansible) installer, Lynis before/after reports, and a runbook for re-applying after kernel updates.
