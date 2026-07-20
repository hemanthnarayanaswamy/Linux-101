# Phase 3 - System Services & systemd

This is Phase 4 (System Services & systemd) of the roadmap, kept as Phase 3 in this repo because the roadmap's Phase 2 (Shell Scripting & Automation) was skipped.

**Phase outcome:** write, debug, and reason about systemd units, manage logs, and understand the boot process.

Primary roadmap sources:

- [linux-mastery-roadmap.md](../../resources/roadMap/linux-mastery-roadmap.md)
- [linux-mastery-roadmap-detailed.md](../../resources/roadMap/linux-mastery-roadmap-detailed.md)

## Quick Index

| Order | Topic | Main Note |
|---:|:------|:----------|
| 1 | systemd architecture | [1_systemd_architecture.md](./1_systemd_architecture.md) |
| 2 | Writing your own services | [2_writing_services.md](./2_writing_services.md) |
| 3 | Logging: journald, rsyslog, log rotation | [3_logging.md](./3_logging.md) |
| 4 | Boot process | [4_boot_process.md](./4_boot_process.md) |

## Reference Indexes

| Area | Index |
|:-----|:------|
| Commands | [Linux commands index](../commands/README.md) |
| Concepts | [Linux concepts index](../concepts/README.md) |

---

## 1. systemd Architecture

What systemd is (PID 1, the init system), why it replaced SysV init, unit types, where units live, and dependencies.

Main note:

- [1_systemd_architecture.md](./1_systemd_architecture.md)

Concepts:

- [systemd units](../concepts/systemd/units.md)
- [Targets and runlevels](../concepts/systemd/targets_runlevels.md)

Commands:

- [`systemctl`](../commands/systemd/systemctl.md)

---

## 2. Writing Your Own Services

Unit anatomy (`[Unit]`/`[Service]`/`[Install]`), `Type=`, restart policies, environment files, and security hardening.

Main note:

- [2_writing_services.md](./2_writing_services.md)

Concepts:

- [systemd units](../concepts/systemd/units.md)

Commands:

- [`systemctl`](../commands/systemd/systemctl.md)
- [`journalctl`](../commands/systemd/journalctl.md)

---

## 3. Logging: journald, rsyslog, Log Rotation

Reading the journal, journald vs rsyslog, persistent journal, and rotating file-based logs.

Main note:

- [3_logging.md](./3_logging.md)

Concepts:

- [journald and rsyslog](../concepts/logging/journald_rsyslog.md)

Commands:

- [`journalctl`](../commands/systemd/journalctl.md)
- [`logrotate`](../commands/logging/logrotate.md)
- [`dmesg`](../commands/logging/dmesg.md)

---

## 4. Boot Process

BIOS/UEFI → GRUB → kernel → initramfs → systemd, analyzing boot time, kernel parameters, and recovery targets.

Main note:

- [4_boot_process.md](./4_boot_process.md)

Concepts:

- [The boot process](../concepts/systemd/boot_process.md)
- [Targets and runlevels](../concepts/systemd/targets_runlevels.md)

Commands:

- [`systemd-analyze`](../commands/systemd/systemd-analyze.md)
- [`dmesg`](../commands/logging/dmesg.md)

---

## Phase Capstone — "Self-Managing App Deployment"

Turn a Flask "hello world" into fully systemd-managed infrastructure:

- `myapp.service` — runs the app as a non-root user with hardening.
- `myapp-healthcheck.service` + `.timer` — probes `/health` every minute, logs to journal.
- `myapp-backup.service` + timer — daily backup of the app's state.
- Logrotate config for any file-based logs.
- `deploy.sh` — on a fresh VM installs deps, creates the user, drops in the units and app code, enables the timers.

Deliverable: a git repo with everything, a README, and a screencast of `deploy.sh` bringing the app up healthy on a fresh VM.
