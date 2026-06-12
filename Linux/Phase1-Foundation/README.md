# Phase 1 - Linux Foundation

Primary roadmap sources:

- [linux-mastery-roadmap.md](../../resources/roadMap/linux-mastery-roadmap.md)
- [linux-mastery-roadmap-detailed.md](../../resources/roadMap/linux-mastery-roadmap-detailed.md)

## Quick Index

| Order | Topic | Main Note |
|---:|:------|:----------|
| 0 | System and environment info | [0_info.md](./0_info.md) |
| 1 | Filesystem and first steps | [1_file_system.md](./1_file_system.md) |
| 2 | CLI basics | [2_cli_basics.md](./2_cli_basics.md) |
| 3 | File operations | [3_files_operations.md](./3_files_operations.md) |
| 4 | Text processing | [4_text_processing.md](./4_text_processing.md) |
| 5 | Process management and job control | [5_process_management_job_control.md](./5_process_management_job_control.md) |
| 6 | Users, groups, and permissions | [6_users_groups_permissions.md](./6_users_groups_permissions.md) |
| 7 | Package management | [7_package_management.md](./7_package_management.md) |
| 8 | Editors, shell config, and tmux | [8_editor_shell.md](./8_editor_shell.md) |

## Reference Indexes

| Area | Index |
|:-----|:------|
| Commands | [Linux commands index](../commands/README.md) |
| Concepts | [Linux concepts index](../concepts/README.md) |

---

## 0. System And Environment Info

Start here before studying commands. You should know which Linux distribution, kernel, shell, and user context you are working with.

Main note:

- [0_info.md](./0_info.md)

Practice:

```bash
cat /etc/os-release
uname -a
whoami
id
echo "$SHELL"
```

Related commands:

- [`id`](../commands/users-groups-permissions/groupadd_groups_id_w.md)

---

## 1. Filesystem And First Steps

Learn the Linux directory layout and how to move around the system.

Main notes:

- [1_file_system.md](./1_file_system.md)
- [2_cli_basics.md](./2_cli_basics.md)

Concepts:

- [Prompt anatomy](../concepts/shell-terminal/prompts.md)
- [Configuration files](../concepts/system-configuration/configurations.md)

Commands:

- [`pwd`, `ls`, `cd`](../commands/basic-navigation/ls_cd_pwd.md)
- [`tree`](../commands/file-search-navigation/tree.md)
- [`man`, `help`, `--help`, `history`, `clear`, `reset`](../commands/help-session/man_help_history.md)

---

## 2. Files: Create, Read, Move, Delete

Learn how to create, inspect, copy, rename, move, and remove files safely.

Main note:

- [3_files_operations.md](./3_files_operations.md)

Concepts:

- [Globbing and wildcards](../concepts/shell-terminal/globbing_wildcards.md)
- [Hard links and soft links](../concepts/filesystem-permissions/links.md)

Commands:

- [`touch`, `mkdir`, `cp`, `mv`, `rm`, `rmdir`](../commands/file-operations/touch_mkdir_cp_mv_rm.md)
- [`ln`](../commands/file-operations/ln.md)
- [`cat`, `less`, `head`, `tail`](../commands/file-viewing/cat_less_head_tail.md)
- [`file`, `stat`, `du`, `df`](../commands/file-metadata/file_stat_du_df.md)
- [`tree`](../commands/file-search-navigation/tree.md)
- [`wc`](../commands/text-processing/wc.md)

---

## 3. Text Processing

Learn how to search, filter, transform, and summarize text streams.

Main note:

- [4_text_processing.md](./4_text_processing.md)

Concepts:

- [Redirection and pipes](../concepts/shell-terminal/redirection.md)
- [Regular expressions](../concepts/text-patterns/regex.md)
- [Process substitution](../concepts/shell-terminal/process_substitution.md)

Commands:

- [`grep`](../commands/text-processing/grep.md)
- [`sed`](../commands/text-processing/sed.md)
- [`awk`](../commands/text-processing/awk.md)
- [`cut`](../commands/text-processing/cut.md)
- [`sort`](../commands/text-processing/sort.md)
- [`uniq`](../commands/text-processing/uniq.md)
- [`tr`](../commands/text-processing/tr.md)
- [`diff`](../commands/text-processing/diff.md)
- [`wc`](../commands/text-processing/wc.md)
- [`xargs`](../commands/text-processing/xargs.md)
- [`find`](../commands/file-search-navigation/find.md)

---

## 4. Process Management And Job Control

Learn how Linux runs programs, tracks processes, handles signals, and manages foreground/background jobs.

Main note:

- [5_process_management_job_control.md](./5_process_management_job_control.md)

Concepts:

- [Process lifecycle and states](../concepts/process-management/process.md)
- [Signals](../concepts/process-management/signals.md)
- [Job control](../concepts/process-management/jobs_control.md)
- [Load average](../concepts/process-management/load_average.md)
- [Process priority](../concepts/process-management/Priority.md)

Commands:

- [`ps`, `pstree`](../commands/process-management/ps_pstree.md)
- [`pgrep`](../commands/process-management/pgrep.md)
- [`pidof`](../commands/process-management/pidof.md)
- [`top`, `htop`](../commands/system-monitoring/top_htop.md)
- [`uptime`](../commands/system-monitoring/uptime.md)
- [`kill`](../commands/process-management/kill.md)
- [`pkill`, `killall`](../commands/process-management/pkill_killall.md)
- [`jobs`](../commands/process-management/jobs.md)
- [`nohup`, `disown`](../commands/process-management/nohup_disown.md)
- [`nice`, `renice`, `ionice`](../commands/process-management/nice_renice_ionice.md)

---

## 5. Users, Groups, And Permissions

Learn identity, ownership, permission modes, special bits, sudo access, and shared directories.

Main note:

- [6_users_groups_permissions.md](./6_users_groups_permissions.md)

Concepts:

- [Special permission bits](../concepts/filesystem-permissions/special_bits.md)
- [Umask](../concepts/filesystem-permissions/umask.md)
- [Configuration files](../concepts/system-configuration/configurations.md)

Commands:

- [`who`](../commands/users-groups-permissions/who.md)
- [`groupadd`, `groups`, `id`, `w`](../commands/users-groups-permissions/groupadd_groups_id_w.md)
- [`useradd`](../commands/users-groups-permissions/useradd.md)
- [`usermod`](../commands/users-groups-permissions/usermod.md)
- [`userdel`](../commands/users-groups-permissions/userdel.md)
- [`passwd`](../commands/users-groups-permissions/passwd.md)
- [`chmod`, `chown`, `chgrp`](../commands/users-groups-permissions/chmod_chown_chgrp.md)
- [`umask`](../commands/users-groups-permissions/umask.md)
- [`sudo`, `visudo`](../commands/users-groups-permissions/sudo_visudo.md)
- [`find`](../commands/file-search-navigation/find.md)

---

## 6. Package Management

Learn how packages, repositories, metadata, dependencies, GPG keys, and universal package formats work.

Main note:

- [7_package_management.md](./7_package_management.md)

Concepts:

- [APT repositories, sources, and PPAs](../concepts/package-management/apt_repositories_sources_ppas.md)
- [Snap and Flatpak](../concepts/package-management/snap_flatpak.md)
- [Configuration files](../concepts/system-configuration/configurations.md)

Commands:

- [`apt`, `apt-mark`](../commands/package-management/apt.md)
- [`dpkg`](../commands/package-management/dpkg.md)
- [`dnf`, `rpm`](../commands/package-management/dnf_rpm.md)
- [`snap`, `flatpak`](../commands/package-management/snap_flatpak.md)
- [`jq`](../commands/structured-data/jq.md)

---

## 7. Editors, Shell Configuration, Dotfiles, And tmux

Learn terminal editing, shell startup files, aliases, functions, prompt customization, and persistent terminal sessions.

Main note:

- [8_editor_shell.md](./8_editor_shell.md)

Concepts:

- [Prompt anatomy](../concepts/shell-terminal/prompts.md)
- [Dotfiles](../concepts/shell-terminal/dotfiles.md)
- [Configuration files](../concepts/system-configuration/configurations.md)

Commands:

- [`nano`](../commands/shell-terminal-editors/nano.md)
- [`vim`](../commands/shell-terminal-editors/vim.md)
- [`source`](../commands/shell-scripting/source.md)
- [`tmux`](../commands/shell-terminal-editors/tmux.md)
- [`ln`](../commands/file-operations/ln.md)

---
