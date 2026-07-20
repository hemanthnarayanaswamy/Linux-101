# Linux Commands Index

Commands are grouped by common Linux usage area.

## Basic Navigation

| Command | Notes |
|:--------|:------|
| `pwd`, `ls`, `cd` | [ls_cd_pwd.md](./basic-navigation/ls_cd_pwd.md) |

## Help and Session

| Command | Notes |
|:--------|:------|
| `man`, `help`, `--help`, `history`, `clear`, `reset` | [man_help_history.md](./help-session/man_help_history.md) |

## File Operations

| Command | Notes |
|:--------|:------|
| `touch`, `mkdir`, `cp`, `mv`, `rm`, `rmdir` | [touch_mkdir_cp_mv_rm.md](./file-operations/touch_mkdir_cp_mv_rm.md) |
| `ln` | [ln.md](./file-operations/ln.md) |

## File Viewing

| Command | Notes |
|:--------|:------|
| `cat`, `less`, `head`, `tail` | [cat_less_head_tail.md](./file-viewing/cat_less_head_tail.md) |

## File Metadata and Disk Usage

| Command | Notes |
|:--------|:------|
| `file`, `stat`, `du`, `df` | [file_stat_du_df.md](./file-metadata/file_stat_du_df.md) |

## File Search and Navigation

| Command | Notes |
|:--------|:------|
| `find` | [find.md](./file-search-navigation/find.md) |
| `locate` | [locate.md](./file-search-navigation/locate.md) |
| `tree` | [tree.md](./file-search-navigation/tree.md) |

## Network Diagnostics

| Command | Notes |
|:--------|:------|
| `ping` | [ping.md](./network-diagnostics/ping.md) |
| `traceroute` | [traceroute.md](./network-diagnostics/traceroute.md) |
| `mtr` | [mtr.md](./network-diagnostics/mtr.md) |
| `tcpdump` | [tcpdump.md](./network-diagnostics/tcpdump.md) |
| `nc` | [nc.md](./network-diagnostics/nc.md) |
| `nmap` | [nmap.md](./network-diagnostics/nmap.md) — TODO |

## Network Interfaces and Configuration

| Command | Notes |
|:--------|:------|
| `ip` | [ip.md](./network-interfaces/ip.md) |
| `ss` | [ss.md](./network-interfaces/ss.md) |
| `nmcli`, netplan | [nmcli_netplan.md](./network-interfaces/nmcli_netplan.md) — TODO |

## DNS and Name Resolution

| Command | Notes |
|:--------|:------|
| `dig`, `nslookup`, `host` | [dig_nslookup_host.md](./dns/dig_nslookup_host.md) |

## SSH and Remote Access

| Command | Notes |
|:--------|:------|
| `sshpass` | [ssh_sshpass.md](./ssh/ssh_sshpass.md) |
| `ssh-keygen`, `ssh-agent`, `ssh-add`, `~/.ssh/config`, `ProxyJump` | [ssh_keys_agent_config.md](./ssh/ssh_keys_agent_config.md) — TODO |
| `scp`, `rsync` | [scp_rsync.md](./ssh/scp_rsync.md) — TODO |

## Firewall

| Command | Notes |
|:--------|:------|
| `ufw` | [ufw.md](./firewall/ufw.md) |
| `nftables` | [nftables.md](./firewall/nftables.md) — TODO |
| `iptables` | [iptables.md](./firewall/iptables.md) — TODO |

## systemd and Service Management

| Command | Notes |
|:--------|:------|
| `systemctl` | [systemctl.md](./systemd/systemctl.md) |
| `journalctl` | [journalctl.md](./systemd/journalctl.md) |
| `systemd-analyze` | [systemd-analyze.md](./systemd/systemd-analyze.md) |

## Logging

| Command | Notes |
|:--------|:------|
| `logrotate` | [logrotate.md](./logging/logrotate.md) |
| `dmesg` | [dmesg.md](./logging/dmesg.md) |

## Storage and Filesystems

| Command | Notes |
|:--------|:------|
| `lsblk`, `blkid` | [lsblk_blkid.md](./storage/lsblk_blkid.md) |
| `parted`, `fdisk`, `gdisk` | [parted_fdisk_gdisk.md](./storage/parted_fdisk_gdisk.md) |
| `pvcreate`, `vgcreate`, `lvcreate`, `lvextend`, `vgextend` | [lvm.md](./storage/lvm.md) |
| `mkfs`, `fsck`, `tune2fs`, `resize2fs` | [mkfs_fsck_tune2fs.md](./storage/mkfs_fsck_tune2fs.md) |
| `mount`, `umount` | [mount_umount.md](./storage/mount_umount.md) |
| `exportfs`, `showmount` (NFS) | [nfs.md](./storage/nfs.md) |

## Security and Hardening

| Command | Notes |
|:--------|:------|
| SSH server hardening (`sshd_config`, `sshd -T`) | [sshd_hardening.md](./ssh/sshd_hardening.md) |
| `fail2ban` | [fail2ban.md](./security/fail2ban.md) |
| AppArmor (`aa-status`, `aa-enforce`, `aa-complain`) | [apparmor.md](./security/apparmor.md) |
| SELinux (`getenforce`, `setenforce`, `audit2allow`) | [selinux.md](./security/selinux.md) |
| `auditctl`, `ausearch`, `aureport` | [auditd.md](./security/auditd.md) |
| AIDE, Lynis | [aide_lynis.md](./security/aide_lynis.md) |
| `last`, `lastb`, `lastlog` | [login_tracking.md](./security/login_tracking.md) |

## Performance and Troubleshooting

| Command | Notes |
|:--------|:------|
| `vmstat`, `mpstat`, `pidstat` | [vmstat_mpstat_pidstat.md](./performance/vmstat_mpstat_pidstat.md) |
| `free` | [free.md](./performance/free.md) |
| `iostat`, `iotop` | [iostat_iotop.md](./performance/iostat_iotop.md) |
| `sar`, `dstat` | [sar_dstat.md](./performance/sar_dstat.md) |
| `lsof` | [lsof.md](./performance/lsof.md) |
| `stress-ng` | [stress-ng.md](./performance/stress-ng.md) |
| `iperf3`, `tc`, `nstat` | [iperf3_tc.md](./performance/iperf3_tc.md) |
| `strace`, `ltrace` | [strace_ltrace.md](./performance/strace_ltrace.md) |
| `perf`, `bpftrace` | [perf_bpftrace.md](./performance/perf_bpftrace.md) |

## Package Management

| Command | Notes |
|:--------|:------|
| `apt`, `apt-mark` | [apt.md](./package-management/apt.md) |
| `dpkg` | [dpkg.md](./package-management/dpkg.md) |
| `dnf`, `rpm` | [dnf_rpm.md](./package-management/dnf_rpm.md) |
| `snap`, `flatpak` | [snap_flatpak.md](./package-management/snap_flatpak.md) |

## Process Management

| Command | Notes |
|:--------|:------|
| `jobs` | [jobs.md](./process-management/jobs.md) |
| `kill` | [kill.md](./process-management/kill.md) |
| `nice`, `renice`, `ionice` | [nice_renice_ionice.md](./process-management/nice_renice_ionice.md) |
| `nohup`, `disown` | [nohup_disown.md](./process-management/nohup_disown.md) |
| `pgrep` | [pgrep.md](./process-management/pgrep.md) |
| `pidof` | [pidof.md](./process-management/pidof.md) |
| `pkill`, `killall` | [pkill_killall.md](./process-management/pkill_killall.md) |
| `ps`, `pstree` | [ps_pstree.md](./process-management/ps_pstree.md) |

## Shell Scripting

| Command | Notes |
|:--------|:------|
| `read` | [read.md](./shell-scripting/read.md) |
| `source` | [source.md](./shell-scripting/source.md) |

## Shell, Terminal, and Editors

| Command | Notes |
|:--------|:------|
| `nano` | [nano.md](./shell-terminal-editors/nano.md) |
| `tmux` | [tmux.md](./shell-terminal-editors/tmux.md) |
| `vim` | [vim.md](./shell-terminal-editors/vim.md) |

## Structured Data

| Command | Notes |
|:--------|:------|
| `jq` | [jq.md](./structured-data/jq.md) |
| `yq` | [yq.md](./structured-data/yq.md) |

## System Monitoring

| Command | Notes |
|:--------|:------|
| `top`, `htop` | [top_htop.md](./system-monitoring/top_htop.md) |
| `uptime` | [uptime.md](./system-monitoring/uptime.md) |

## Text Processing

| Command | Notes |
|:--------|:------|
| `awk` | [awk.md](./text-processing/awk.md) |
| `cut` | [cut.md](./text-processing/cut.md) |
| `diff` | [diff.md](./text-processing/diff.md) |
| `grep` | [grep.md](./text-processing/grep.md) |
| `sed` | [sed.md](./text-processing/sed.md) |
| `sort` | [sort.md](./text-processing/sort.md) |
| `tr` | [tr.md](./text-processing/tr.md) |
| `uniq` | [uniq.md](./text-processing/uniq.md) |
| `wc` | [wc.md](./text-processing/wc.md) |
| `xargs` | [xargs.md](./text-processing/xargs.md) |

## Users, Groups, and Permissions

| Command | Notes |
|:--------|:------|
| `chmod`, `chown`, `chgrp` | [chmod_chown_chgrp.md](./users-groups-permissions/chmod_chown_chgrp.md) |
| `groupadd`, `groups`, `id`, `w` | [groupadd_groups_id_w.md](./users-groups-permissions/groupadd_groups_id_w.md) |
| `passwd` | [passwd.md](./users-groups-permissions/passwd.md) |
| `sudo`, `visudo` | [sudo_visudo.md](./users-groups-permissions/sudo_visudo.md) |
| `umask` | [umask.md](./users-groups-permissions/umask.md) |
| `useradd` | [useradd.md](./users-groups-permissions/useradd.md) |
| `userdel` | [userdel.md](./users-groups-permissions/userdel.md) |
| `usermod` | [usermod.md](./users-groups-permissions/usermod.md) |
| `who` | [who.md](./users-groups-permissions/who.md) |
