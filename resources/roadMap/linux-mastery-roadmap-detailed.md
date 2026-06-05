# Linux Mastery Roadmap — DevOps / Platform / SRE

### Phase overview

| Phase | Topic | Weeks | Hours |
|---|---|---|---|
| 1 | Linux Fundamentals | 1–8 | ~40 |
| 2 | Shell Scripting & Automation | 9–14 | ~30 |
| 3 | Networking | 15–20 | ~30 |
| 4 | System Services & systemd | 21–25 | ~25 |
| 5 | Storage & Filesystems | 26–29 | ~20 |
| 6 | Security & Hardening | 30–34 | ~25 |
| 7 | Performance & Troubleshooting | 35–40 | ~30 |
| 8 | Containers & Kubernetes Foundations | 41–46 | ~30 |
| 9 | Observability | 47–50 | ~20 |
| 10 | IaC & CI/CD | 51–56 | ~30 |
| 11 | Portfolio Capstone | 57–60 | ~20 |

---

# Phase 1 — Linux Fundamentals (Weeks 1–8)

**Phase outcome:** You can navigate any Linux box confidently, manipulate files and text, manage processes and users, and install software. You feel at home in the terminal.

## Week 1 — Setup & First Steps

**Objectives**
- Have a working Ubuntu lab environment.
- Understand what a shell is and how the prompt is structured.
- Navigate the filesystem.

**Topics**
- What Linux is, distributions (Ubuntu/Debian/RHEL/Arch), kernel vs userspace.
- The shell (bash), prompt anatomy (`user@host:~$`).
- Filesystem hierarchy: `/`, `/home`, `/etc`, `/var`, `/usr`, `/tmp`, `/proc`, `/sys`, `/opt`.
- Commands: `pwd`, `ls`, `cd`, `tree`, `man`, `help`, `--help`, `clear`, `history`.

**Exercises**
1. Install Ubuntu 24.04 in a VM. Take a snapshot named `clean-install` (you'll thank yourself).
2. From your home directory, run `ls -la`. List every file starting with `.` and write down what you think each does.
3. Use `cd` to walk to `/etc`, `/var/log`, `/proc/1/`, `/sys/class/net/`. After each, run `pwd` and `ls`. Write one sentence describing what lives there.
4. Run `man ls`. Find the flag that sorts files by modification time, newest last. Demonstrate it.
5. Run `history`, then re-run command #5 using `!5`.

**Mini-project: "Filesystem field guide"**
Create `~/linux-journal/week01/filesystem-map.md`. For each top-level directory under `/`, write 1–2 sentences explaining its purpose, in your own words. Commit to git.

**Self-check**
- What's the difference between an absolute and a relative path?
- What does `~` expand to? What about `-` after `cd`?

---

## Week 2 — Files: Reading, Creating, Moving

**Objectives**
- Comfortably create, copy, move, and delete files and directories.
- Read files multiple ways, including large ones safely.
- Understand wildcards.

**Topics**
- `touch`, `mkdir -p`, `cp -r`, `mv`, `rm -i`, `rmdir`, `ln -s`.
- Reading: `cat`, `less`, `head`, `tail`, `tail -f`, `wc`, `file`, `stat`.
- Globbing: `*`, `?`, `[abc]`, `{a,b,c}`.
- Hard links vs symlinks (lightly).

**Exercises**
1. Build this tree in `~/lab/week02/` using one `mkdir -p` per leaf, then verify with `tree`:
   ```
   project/
   ├── src/{app,lib,utils}/
   ├── tests/{unit,integration}/
   └── docs/
   ```
2. Create 30 numbered files: `touch file{01..30}.txt`. Move only the even-numbered ones into a new `even/` directory using a single glob.
3. `tail -f /var/log/syslog` in one terminal. In another, run `logger "hello from week 2"`. Observe.
4. Create a symlink: `ln -s /etc/hostname ~/myhost`. `cat ~/myhost`. Then `ls -l ~/myhost` — explain the arrow.
5. Run `stat /etc/passwd`. Identify: size, inode, permissions, last access, last modification, last status change. Write down what each means.

**Mini-project: "Safe rm wrapper"**
Write a short note (no scripting yet — just plan) describing why `rm -rf /` is dangerous and what aliases or shells (`trash-cli`, `safe-rm`) people use to avoid catastrophe. Save to journal.

**Self-check**
- What does `rm *` do in a directory containing a file named `-rf`? (The answer should worry you.)
- When does `cp` need `-r`?

---

## Week 3 — Text Processing: grep, sed, awk

**Objectives**
- Find any string in any file.
- Transform text streams in-place.
- Extract columns from logs without opening them in an editor.

**Topics**
- Pipes `|`, redirection `>`, `>>`, `<`, `2>`, `2>&1`, `&>`.
- `grep -E`, `-i`, `-v`, `-r`, `-n`, `-c`, `-A`, `-B`, `-C`.
- `sed 's/old/new/g'`, `sed -i`, line addressing.
- `awk '{print $1}'`, field separators, simple conditions.
- `cut`, `sort`, `uniq -c`, `tr`.
- Regex basics: `^`, `$`, `.`, `*`, `+`, `?`, `[]`, `()`, `|`.

**Exercises**
1. From `/var/log/syslog`, count how many distinct programs logged messages today. (`awk` or `cut` + `sort -u`.)
2. From `/etc/passwd`, list every username whose default shell is `/bin/bash`. One-liner.
3. Replace every occurrence of `localhost` with `myhost.local` in a *copy* of `/etc/hosts` using `sed`. Verify with `diff`.
4. From an Apache-style log (download a sample from `https://www.almhuette-raith.at/apache-log/access.log` — or generate fake ones), find the top 10 IPs by request count.
5. Use `grep -rEn 'TODO|FIXME' .` against any code repo. Pipe into `wc -l`.

**Mini-project: "Log triage one-liner kit"**
Create `~/linux-journal/week03/oneliners.md` with 10 reusable one-liners you'd actually use on the job (top IPs, error counts per hour, slowest endpoints, etc.). Each entry: command, sample input, sample output.

**Self-check**
- What's the difference between `>` and `>>`?
- Why does `grep foo bar` (without `-r`) sometimes act weird? (Hint: `bar` is treated as a file.)

---

## Week 4 — Processes & Job Control

**Objectives**
- See, signal, prioritize, and kill processes.
- Understand foreground/background and what a "job" is.

**Topics**
- `ps aux`, `ps -ef`, `ps --forest`, `pstree`, `pgrep`, `pidof`.
- `top`, `htop` (install it), `uptime`, load average.
- Signals: `SIGTERM` (15), `SIGKILL` (9), `SIGHUP` (1), `SIGINT` (2). `kill`, `killall`, `pkill`.
- Jobs: `&`, `Ctrl-Z`, `bg`, `fg`, `jobs`, `nohup`, `disown`.
- `nice`, `renice`, `ionice` (briefly).

**Exercises**
1. Run `sleep 600 &` three times. List the jobs with `jobs`. Bring one to foreground, suspend it, send it back. Kill all three by PID.
2. Run `htop`. Sort by memory. Identify the top 3 memory consumers. Filter to processes owned by your user only.
3. Start `yes > /dev/null &` (warning: this pegs a CPU — kill it after). Watch CPU rise in `htop`. Send `SIGTERM`, then verify it's gone with `pgrep yes`.
4. Run a process with `nice -n 19 sha256sum /dev/zero &`. Compare its CPU share to a default-priority `sha256sum /dev/zero &`. Kill both when done.
5. Open a long-running command, then disown it and close the terminal. Reopen — confirm it's still running.

**Mini-project: "ps cheat sheet"**
Document, with your own examples, 10 `ps` invocations you'll memorize: by user, by name, with parent PID, sorted by CPU/memory, in tree form, etc.

**Self-check**
- What does `kill -9` actually do, and why is it the *last* resort?
- What's load average and why is "1.00 on a 4-core box" healthy but "4.00" is concerning?

---

## Week 5 — Users, Groups, Permissions

**Objectives**
- Manage users and groups confidently.
- Read and set permissions in both symbolic and octal form.
- Understand setuid, setgid, sticky bit.

**Topics**
- `/etc/passwd`, `/etc/shadow`, `/etc/group`.
- `useradd`, `usermod`, `userdel`, `passwd`, `groupadd`, `groups`, `id`, `who`, `w`.
- Permissions: rwx for user/group/other, octal (`755`, `644`, `600`).
- `chmod`, `chown`, `chgrp`, `umask`.
- Special bits: setuid (`4xxx`), setgid (`2xxx`), sticky (`1xxx`). Why `/tmp` is `1777`.
- `sudo`, `/etc/sudoers`, `visudo`.

**Exercises**
1. Create users `alice` and `bob`, plus a group `devs`. Add both to `devs`. Verify with `id alice`.
2. Create `/srv/shared`, owned by `root:devs`, mode `2775`. Have alice create a file there, then check its group ownership — explain why setgid matters.
3. Create a file as your normal user, `chmod 600` it, then `su - bob` and try to read it. Observe the error.
4. Find every setuid binary on the system: `find / -perm -4000 -type f 2>/dev/null`. Pick three (e.g. `passwd`, `sudo`, `ping`) and explain *why* each needs setuid.
5. Edit sudoers with `visudo` to allow `alice` to run only `systemctl restart nginx` without a password. Test it (install nginx first if needed).

**Mini-project: "Multi-user shared workspace"**
Set up a directory structure where:
- `/srv/team/` is the team root.
- Subdirs `engineering/` and `design/` are accessible only to those respective groups.
- Files created inside inherit the correct group.
- A shared `inbox/` lets anyone drop files but only the owner can delete them.

Document your `chmod`/`chown` commands and explain *why* each bit is set.

**Self-check**
- What does `umask 022` produce for new files vs new directories?
- Why is `/etc/shadow` mode `640` and not `644`?

---

## Week 6 — Package Management

**Objectives**
- Install, update, remove, and search for software on Debian/Ubuntu.
- Understand repos, sources, and PPAs.
- Know enough about RPM-based distros (RHEL/CentOS/Rocky) to not panic.

**Topics**
- `apt update`, `apt upgrade`, `apt install`, `apt remove`, `apt purge`, `apt search`, `apt show`.
- `dpkg -l`, `dpkg -L`, `dpkg -S`, `dpkg -i`.
- `/etc/apt/sources.list`, `/etc/apt/sources.list.d/`.
- Adding a third-party repo: GPG key + sources entry (modern approach: keyrings in `/etc/apt/keyrings/`).
- RHEL family briefly: `dnf install`, `rpm -qa`, `rpm -ql`.
- Snap and Flatpak — what they are, when to avoid them.

**Exercises**
1. Search for `htop`. Show its full description and dependencies before installing.
2. Install `nginx`. Use `dpkg -L nginx` to list every file it placed on disk. Find the systemd unit file and the default site config.
3. Install Docker from Docker's official repo (not the distro one). Practice the modern keyring workflow — no `apt-key add` (deprecated).
4. Find which package owns `/usr/bin/curl` using `dpkg -S`.
5. Hold a package at its current version: `apt-mark hold nginx`. Then unhold it. Explain when you'd do this in production.

**Mini-project: "Reproducible workstation bootstrap"**
Create `~/linux-journal/week06/bootstrap.sh` (no need to run it as a script yet — just a documented list) that installs every tool you'd want on a fresh box: build-essential, git, curl, jq, htop, tmux, your editor, etc. Add comments explaining each.

**Self-check**
- What's the difference between `apt remove` and `apt purge`?
- Why do you `apt update` before `apt install`?

---

## Week 7 — Editors, Dotfiles, tmux

**Objectives**
- Edit files in a terminal-only environment without panicking.
- Understand and customize your shell environment.
- Use tmux to keep work alive across SSH disconnects.

**Topics**
- `nano` basics — fine for quick edits.
- **vim** essentials: modes (normal, insert, visual), `:wq`, `:q!`, `dd`, `yy`, `p`, `/search`, `:%s/old/new/g`. Don't try to master vim this week — just survive in it.
- Shell config: `~/.bashrc`, `~/.bash_profile`, `~/.profile`. Aliases, functions, `PATH`, `PS1`.
- `tmux`: sessions, windows, panes, prefix key, detach (`Ctrl-b d`), reattach (`tmux a`).
- Dotfiles repos — what they are, why people version them.

**Exercises**
1. In vim, open `/etc/services` (read-only). Search for `https`. Find what port it uses. Quit without saving.
2. Add three aliases to your `.bashrc`: `ll='ls -lah'`, `..='cd ..'`, and one of your choosing. `source ~/.bashrc`. Use them.
3. Customize your `PS1` to show: username, hostname, current directory, git branch (use a snippet — google "minimal git PS1"). Take a screenshot.
4. Start a tmux session named `work`. Split into two panes. Run `htop` in one, a shell in the other. Detach. Reattach. Kill the session cleanly.
5. Create a public `dotfiles` repo on GitHub. Push your `.bashrc`, `.vimrc` (even if minimal), `.tmux.conf`. Use symlinks from a clone, not copies.

**Mini-project: "Personal dotfiles repo"**
Your `dotfiles` repo with a one-line installer (`curl ... | bash` style — but read-it-first). Include README with setup instructions. This goes on your résumé.

**Self-check**
- What's the difference between a login shell and an interactive shell, and which file is read for each?
- How do you exit vim? (If you ever forget: `Esc` then `:q!`.)

---

## Week 8 — **Phase 1 Capstone**

**Project: "New developer onboarding box"**

You've been asked to prep a fresh Ubuntu VM for a new hire. Deliver:

1. A new VM (clean snapshot from Week 1, or fresh).
2. User `newdev` created with a strong password and added to `sudo`.
3. A shared `/srv/projects/` directory owned by group `developers`, with setgid so files inherit the group.
4. The following installed: build-essential, git, curl, wget, jq, htop, tmux, vim, nginx, docker (from official repo).
5. Custom `/etc/motd` greeting the user with system info.
6. `~/linux-journal/week08/runbook.md` documenting:
   - Every command you ran, in order.
   - Why you ran it.
   - How you'd verify each step worked.
   - What could go wrong and how to recover.

Take a final snapshot named `phase1-complete`. Push the runbook to your journal repo.

**You're ready for Phase 2 if you can:** open a fresh Ubuntu box, navigate it, install software, manage users, and read logs without googling basic syntax.

---

# Phase 2 — Shell Scripting & Automation (Weeks 9–14)

**Phase outcome:** You can write robust bash scripts to automate real tasks, with proper error handling, logging, and scheduling.

## Week 9 — Bash Basics: Variables, Quoting, Conditionals

**Objectives**
- Write and run a bash script.
- Master quoting (the #1 source of bash bugs).
- Use `if`, `[[ ]]`, exit codes.

**Topics**
- Shebang `#!/usr/bin/env bash`, `chmod +x`, `./script.sh`.
- Variables: `name=value` (no spaces), `$name`, `${name}`, `"$name"` vs `'$name'`.
- Quoting rules: single quotes are literal, double quotes interpolate, no quotes split on whitespace.
- Special vars: `$0`, `$1`–`$9`, `$#`, `$@`, `$*`, `$?`, `$$`.
- Conditionals: `if`, `elif`, `else`, `fi`. `[[ ]]` (prefer over `[ ]`).
- Exit codes: 0 = success, non-zero = failure. `&&`, `||`.

**Exercises**
1. Write `hello.sh` that takes a name argument and prints `Hello, <name>!`. Default to `World` if no arg.
2. Write `is-even.sh` that takes a number, prints `even` or `odd`, exits 0 or 1 accordingly. Verify with `echo $?`.
3. Reproduce this bug, then fix it: create a file called `my file.txt`, then write a script that does `rm $1` and call it with `./script.sh "my file.txt"`. (Quote `"$1"`.)
4. Write `check-port.sh <port>` that prints whether something is listening on the given TCP port (`ss -tlnp | grep`).
5. Write `disk-warn.sh` that prints `WARNING` if `/` is over 80% full, else `OK`. Use `df` + `awk`.

**Mini-project: "Backup script v1"**
`backup.sh <source-dir> <dest-dir>` — copies source into dest with a timestamp suffix. Validate inputs (dirs exist), exit with helpful error messages.

**Self-check**
- Why does `if [ $name = "foo" ]` break when `$name` is empty, and how does `[[ ]]` fix it?
- What does `set -e` do, and why is it controversial?

---

## Week 10 — Loops, Functions, Arrays

**Objectives**
- Loop over files, lines, ranges.
- Modularize code with functions.
- Use bash arrays correctly.

**Topics**
- `for x in ...; do ... done`, `for i in {1..10}`, C-style `for ((i=0; i<10; i++))`.
- `while read line; do ... done < file` — the canonical line-reader.
- `until`, `break`, `continue`.
- Functions: definition, `local`, return values via stdout (not `return` — that's exit codes).
- Arrays: `arr=(a b c)`, `${arr[0]}`, `${arr[@]}`, `${#arr[@]}`.

**Exercises**
1. Loop over every `.log` file in `/var/log/` and print filename + line count.
2. Read `/etc/passwd` line-by-line, print only usernames whose UID >= 1000 (real users).
3. Write a function `log()` that prefixes its argument with `[YYYY-MM-DD HH:MM:SS]` and writes to stderr.
4. Build an array of 5 hostnames. Loop and `ping -c1` each, collect successes into a new array, print the survivors.
5. Refactor your Week 9 backup script to use functions: `validate_args`, `do_backup`, `cleanup`.

**Mini-project: "Bulk image renamer"**
Script that takes a directory of photos and renames them to `YYYY-MM-DD_NNN.jpg` based on file mtime. Dry-run mode by default; `--apply` actually renames.

**Self-check**
- What's the difference between `${arr[*]}` and `${arr[@]}` when quoted?
- Why is `for f in $(ls)` an antipattern? (Hint: filenames with spaces.)

---

## Week 11 — Robust Scripting: Args, Errors, Logging

**Objectives**
- Parse flags like a real CLI tool.
- Fail loudly and helpfully.
- Log structured output.

**Topics**
- `getopts` for short flags; `case` parsing for long flags.
- `set -euo pipefail` and what each flag does.
- `trap 'cleanup' EXIT INT TERM` for cleanup on exit.
- `mktemp -d` for safe temp dirs.
- Stderr vs stdout, exit codes, `>&2`.
- Logging levels: INFO, WARN, ERROR. Sending to syslog with `logger`.

**Exercises**
1. Write `mytool.sh` that supports `-v` (verbose), `-n` (dry-run), `-h` (help), and a positional file argument. Use `getopts`.
2. Add `set -euo pipefail` to a script that has a typo (e.g., `$nme` instead of `$name`). Observe how it fails fast.
3. Write a script that creates a temp dir, does work in it, and cleans up via `trap` even if interrupted with Ctrl-C.
4. Write a `log()` function that supports levels (`log INFO "..."`, `log ERROR "..."`) and writes to both stderr and `logger -t mytool`.
5. Convert one of your earlier scripts to follow the "strict mode" template you've now learned.

**Mini-project: "Production-grade backup script v2"**
Rewrite the Week 9 backup tool with: getopts, dry-run, verbose, log levels, trap-based cleanup, and clear exit codes. Include a `--help` that's actually useful.

**Self-check**
- What does `pipefail` do and why does it matter?
- When does `trap ... EXIT` run?

---

## Week 12 — Real-World Scripts

**Objectives**
- Apply scripting to actual ops tasks.
- Practice reading/writing JSON and CSV from bash.
- Integrate `jq` and `curl`.

**Topics**
- `curl -fsSL`, `-H`, `-d`, `-X`, exit codes.
- `jq`: `.field`, `.[]`, `select()`, `-r`, `@csv`.
- Working with environment variables and `.env` files.
- Scripts that talk to APIs.

**Exercises**
1. Hit `https://api.github.com/users/<your-username>/repos`, extract repo names + stars with `jq`, sort by stars.
2. Write a healthcheck script that GETs a URL, fails if status isn't 200 or response time > 500ms.
3. Parse a CSV (use any sample) and emit JSON via a bash + `awk` + `jq` pipeline.
4. Write a script that reads `.env`, exports vars, and runs a command with them — basically a tiny `dotenv` clone.
5. Write `disk-report.sh` that emails (or writes to file) a daily report of disk usage on every mounted filesystem.

**Mini-project: "Server health snapshot tool"**
`health.sh` that produces a JSON report containing: hostname, uptime, load avg, memory %, root disk %, listening ports, top 5 CPU procs, top 5 mem procs. Output is parseable by `jq`. Bonus: support `--format human` for a pretty version.

**Self-check**
- How do you check if `curl` succeeded — by HTTP status, by exit code, or both?
- What's the difference between `jq '.foo'` and `jq -r '.foo'`?

---

## Week 13 — Scheduling: cron, systemd timers, at

**Objectives**
- Schedule recurring jobs reliably.
- Choose between cron and systemd timers.
- Diagnose "my cron job didn't run" — the eternal question.

**Topics**
- `crontab -e`, `crontab -l`, syntax (`min hr dom mon dow cmd`), `@reboot`, `@daily`.
- `/etc/cron.d/`, `/etc/cron.daily/`.
- Why cron jobs fail: PATH, no TTY, working directory, missing env.
- systemd timers: `.service` + `.timer` units, `OnCalendar=`, `Persistent=true`.
- `at` for one-shot scheduling.

**Exercises**
1. Schedule your `health.sh` from week 12 to run every 15 minutes via cron, appending to `/var/log/health.log`. Debug the inevitable PATH problem.
2. Convert that same job to a systemd timer pair (`health.service` + `health.timer`). Enable, start, check `systemctl list-timers`.
3. Write a `@reboot` cron job that logs boot time to a file. Reboot. Verify.
4. Use `at` to schedule "shutdown -r" 5 minutes from now. Cancel it before it fires with `atrm`.
5. Diagnose a deliberately broken cron job — one that uses a relative path or assumes `$HOME` exists.

**Mini-project: "Self-healing log rotator"**
A systemd timer that runs daily at 03:00, rotates `/var/log/health.log` (your file from earlier), keeps 7 days, gzips old ones, and emails (or just logs) on failure.

**Self-check**
- Why prefer systemd timers over cron in modern systems? (Hint: dependencies, logging, monitoring.)
- What's the timezone of cron?

---

## Week 14 — **Phase 2 Capstone**

**Project: "Server inventory CLI"**

A bash CLI tool — `srvctl` — installable via a single `make install`. Subcommands:

- `srvctl status` — JSON report of system health (your week 12 work, polished).
- `srvctl users` — list real users with last login.
- `srvctl ports` — list listening ports with owning process.
- `srvctl backup <dir>` — your week 11 backup tool.
- `srvctl --help` — auto-generated.

Requirements:
- Strict mode, getopts, log levels, dry-run support.
- Installable via `make install` to `/usr/local/bin/`.
- Companion systemd timer that runs `srvctl status` hourly and stores results.
- README with examples and a screencast (use `asciinema`).
- Public GitHub repo.

This is a portfolio piece. Write it like you'd want a hiring manager to see it.

---

# Phase 3 — Networking (Weeks 15–20)

**Phase outcome:** You can diagnose connectivity problems, configure interfaces, secure SSH, and write firewall rules.

## Week 15 — Networking Fundamentals

**Objectives**
- Build a mental model: OSI layers, TCP/IP, packets, ports.
- Read an IP address and understand subnet basics.

**Topics**
- OSI model (focus on L2/L3/L4/L7); TCP vs UDP.
- IPv4: addresses, CIDR, private ranges (10/8, 172.16/12, 192.168/16), loopback.
- Ports: well-known (<1024), ephemeral, TCP states.
- DNS at a high level (covered deeper in week 17).
- Three-way handshake; TCP connection lifecycle.

**Exercises**
1. Draw, by hand, the OSI model with one example protocol per layer. Photograph it into your journal.
2. Write down the CIDR range for a `/24`, `/16`, `/8`. How many hosts in each?
3. Use `ip addr` to identify your machine's IP, MAC address, and interface name.
4. Trace the path from your machine to `1.1.1.1` with `traceroute` (or `mtr`).
5. Use `nc -zv google.com 443` to test connectivity. Then try port 22 — note the difference.

**Mini-project: "My network diagram"**
Diagram (draw.io / excalidraw) your home network: router, switch (if any), every device, IP ranges, DNS, default gateway. Save to journal.

**Self-check**
- What's the difference between TCP and UDP?
- Why are 10.x.x.x addresses "private"?

---

## Week 16 — Linux Networking Tools

**Objectives**
- Replace deprecated tools (`ifconfig`, `netstat`, `route`) with modern ones.
- Configure interfaces and routes.

**Topics**
- `ip addr`, `ip link`, `ip route`, `ip neigh` (replacing `ifconfig`, `route`, `arp`).
- `ss -tulnp` (replacing `netstat`).
- `ping`, `traceroute`, `mtr`, `dig`, `host`.
- `nmcli` (NetworkManager) basics on Ubuntu desktop; `netplan` on server.
- `tcpdump` introduction: `tcpdump -i any port 80`.

**Exercises**
1. List all network interfaces and their states. Identify which is your default route's interface.
2. Find every TCP socket your machine is listening on, with the owning process. Compare TCP vs UDP.
3. Add a temporary secondary IP to your loopback interface: `ip addr add 10.99.99.1/32 dev lo`. Ping it. Remove it.
4. Capture 10 packets on port 53 with `tcpdump`, then trigger DNS lookups in another terminal. Read the output.
5. Edit your `/etc/netplan/*.yaml` (in a VM, with a snapshot first!) to set a static IP. Apply with `netplan try`. Revert.

**Mini-project: "ss/ip cheat sheet"**
A markdown reference of the 15 networking commands you'll actually use, with example output. Include the modern tool *and* its deprecated equivalent so you can recognize both.

**Self-check**
- What's the difference between `ss -tlnp` and `ss -tulnp`?
- How do you flush the routing table cache (and why might you not need to)?

---

## Week 17 — DNS & Name Resolution

**Objectives**
- Understand how a hostname becomes an IP.
- Diagnose DNS issues.
- Run a small DNS resolver.

**Topics**
- Resolution order: `/etc/hosts` → `/etc/resolv.conf` → upstream resolver → root → TLD → authoritative.
- Record types: A, AAAA, CNAME, MX, TXT, NS, SOA, PTR.
- `dig +trace`, `dig @8.8.8.8`, `dig -x` (reverse).
- `systemd-resolved` and the modern Ubuntu DNS stack.
- TTL, caching, propagation myths.

**Exercises**
1. `dig +trace google.com` — read every step of the trace. Identify the root, TLD, and authoritative servers.
2. `dig MX gmail.com` — list Google's mail servers in priority order.
3. Add an entry to `/etc/hosts` mapping `myapp.local` to `127.0.0.1`. Verify with `getent hosts myapp.local`.
4. Reverse-lookup `8.8.8.8` and `1.1.1.1` with `dig -x`.
5. Diagnose: change `/etc/resolv.conf` to a non-resolver (e.g., `nameserver 192.0.2.1`). Watch DNS fail. Restore.

**Mini-project: "Tiny local DNS resolver"**
Install `dnsmasq` on your VM. Configure it to:
- Resolve `*.lab` domains locally to specific IPs (e.g., `web.lab → 192.168.56.10`).
- Forward everything else to `1.1.1.1`.
- Cache results.

Document the config in your journal. Test with `dig @localhost web.lab`.

**Self-check**
- What's the difference between A and CNAME records?
- Why is `getent hosts` more accurate than `ping` for testing resolution?

---

## Week 18 — SSH Deep Dive

**Objectives**
- Use SSH like a pro: keys, agents, configs, jump hosts, tunnels.
- Harden SSH for production.

**Topics**
- Key types: ed25519 (preferred), RSA. `ssh-keygen -t ed25519`.
- `ssh-agent`, `ssh-add`, `IdentityFile`.
- `~/.ssh/config`: `Host`, `HostName`, `User`, `Port`, `IdentityFile`, `ProxyJump`.
- `authorized_keys`, `known_hosts`, host key verification.
- Port forwarding: local (`-L`), remote (`-R`), dynamic SOCKS (`-D`).
- `scp`, `rsync -av --progress` (and why rsync is usually better).

**Exercises**
1. Generate an ed25519 key pair. Copy the public key to a second VM with `ssh-copy-id`. Log in passwordless.
2. Write a `~/.ssh/config` block aliasing `prod-web` to `ssh -p 2222 deploy@10.0.0.5`. SSH in with just `ssh prod-web`.
3. Set up a jump host: `ssh -J jump@bastion target@internal`. Then convert that to a `ProxyJump` entry in your config.
4. Tunnel a remote service to localhost: `ssh -L 8080:localhost:80 user@remote`. Hit `http://localhost:8080` in a browser.
5. rsync a directory to a remote host with `--delete` and `--dry-run`. Then for real.

**Mini-project: "Bastion + private host" lab**
Two VMs in your lab:
- `bastion` — public-facing, accepts your SSH key.
- `internal` — only reachable through bastion.

Configure:
- `~/.ssh/config` to use ProxyJump for one-command access.
- SSH on a non-standard port for bastion.
- `internal` only accepts connections from `bastion`'s IP.
- Document the topology and commands in journal.

**Self-check**
- Why prefer ed25519 over RSA?
- What's the difference between `-L` and `-R` port forwarding?

---

## Week 19 — Firewalls: nftables, ufw, iptables

**Objectives**
- Write firewall rules that survive reboots.
- Understand stateful filtering.

**Topics**
- `ufw` for quick wins: `ufw allow 22/tcp`, `ufw enable`, `ufw status`.
- `nftables` (modern): tables, chains, rules; `nft list ruleset`.
- `iptables` (legacy but ubiquitous): you'll see this in production for years.
- Stateful matching: `ct state established,related accept`.
- Default-deny posture.

**Exercises**
1. Enable `ufw`, default-deny inbound, allow SSH only. Test from another VM.
2. Allow only your home IP to SSH; block everyone else.
3. Read `nft list ruleset`. Identify the equivalent of your ufw rules in the underlying nftables.
4. Write a raw nftables ruleset from scratch in a `.nft` file: allow loopback, allow established, allow SSH from a specific subnet, drop everything else inbound. Load with `nft -f`.
5. Convert one rule to its `iptables` equivalent so you can read both syntaxes.

**Mini-project: "Hardened web server firewall"**
On a VM running nginx:
- Allow 22/tcp from your IP only.
- Allow 80 and 443/tcp from anywhere.
- Rate-limit SSH attempts to 3/min.
- Drop all else inbound, allow all outbound.
- Persist across reboots (systemd or `nftables.service`).
- Document the rules with comments.

**Self-check**
- What does "stateful" mean in firewall context?
- Why is "allow established,related" almost always the first rule?

---

## Week 20 — **Phase 3 Capstone**

**Project: "Three-tier home lab network"**

In your VM environment, build:
- `bastion` — only public-facing host. SSH on port 2222 only.
- `web` — nginx serving a simple page. Reachable on 80/443 from internet, SSH only from `bastion`.
- `db` — PostgreSQL or MySQL. Reachable on 5432/3306 only from `web`. SSH only from `bastion`.
- All hosts use a local dnsmasq resolver from week 17 so they refer to each other by name (`web.lab`, `db.lab`).
- Each host has nftables rules persistent across reboot.
- Your `~/.ssh/config` lets you reach `web` and `db` with one command via `ProxyJump`.

Deliverables:
- Network diagram.
- Each host's firewall config in git.
- Step-by-step runbook to rebuild the lab from scratch.

---

# Phase 4 — System Services & systemd (Weeks 21–25)

**Phase outcome:** You can write, debug, and reason about systemd units, manage logs, and understand the boot process.

## Week 21 — systemd Architecture

**Objectives**
- Understand what systemd is and why it replaced SysV init.
- Read and interpret unit files.

**Topics**
- PID 1, the init system. Targets vs runlevels.
- Unit types: `service`, `socket`, `timer`, `mount`, `target`, `path`.
- `systemctl status`, `start`, `stop`, `restart`, `reload`, `enable`, `disable`, `mask`.
- Where units live: `/lib/systemd/system/`, `/etc/systemd/system/`, `~/.config/systemd/user/`.
- Dependencies: `Wants`, `Requires`, `After`, `Before`.

**Exercises**
1. List every loaded service and its state with `systemctl list-units --type=service`.
2. Pick a running service (e.g., `ssh`, `cron`). Read its unit file with `systemctl cat`. Identify ExecStart, dependencies, restart policy.
3. List all targets with `systemctl list-units --type=target`. Identify which one is the default (`systemctl get-default`).
4. Mask a service (`systemctl mask <something-harmless-like-cups>`). Try to start it. Unmask it.
5. Show the dependency tree of `multi-user.target` with `systemctl list-dependencies`.

**Mini-project: "systemd cheat sheet"**
A reference doc covering the 20 commands you'll use 95% of the time, with one-line examples each.

**Self-check**
- What's the difference between `enable` and `start`?
- What's a target?

---

## Week 22 — Writing Your Own Services

**Objectives**
- Write a unit file from scratch.
- Use restart policies, environment files, security hardening.

**Topics**
- Unit anatomy: `[Unit]`, `[Service]`, `[Install]`.
- `Type=`: `simple`, `forking`, `oneshot`, `notify`, `exec`.
- `Restart=on-failure`, `RestartSec=`, `StartLimitBurst=`.
- `User=`, `Group=`, `WorkingDirectory=`, `EnvironmentFile=`.
- Security: `ProtectSystem=`, `ProtectHome=`, `NoNewPrivileges=`, `PrivateTmp=`.
- `WantedBy=multi-user.target`.

**Exercises**
1. Write a unit file for a Python script that just prints to stdout in a loop. `systemctl start`, `status`, `journalctl -u`.
2. Add `Restart=on-failure`. Crash the script (raise an exception). Watch it restart automatically.
3. Run the service as a non-root user. Verify with `ps -u <user>`.
4. Add `ProtectSystem=strict` and `PrivateTmp=true`. Have the script try to write to `/etc/foo` — confirm it fails.
5. Convert one of your bash scripts (e.g., the health.sh from Phase 2) into a proper systemd service.

**Mini-project: "Production-style service unit"**
Take any small app (a Python Flask "hello world" works) and ship it as a service with:
- Dedicated system user.
- Environment file in `/etc/default/myapp`.
- Restart policy.
- Hardening directives (Protect*, NoNewPrivileges).
- Logs going to journal.

**Self-check**
- When would you use `Type=oneshot`?
- Why use `EnvironmentFile=` instead of inline `Environment=`?

---

## Week 23 — Logging: journald, rsyslog, log rotation

**Objectives**
- Read systemd journal logs effectively.
- Understand the relationship between journald and rsyslog.
- Rotate logs to avoid filling disks.

**Topics**
- `journalctl -u`, `-f`, `-S "1 hour ago"`, `--since today`, `-p err`.
- Persistent journal: `/var/log/journal/` vs `/run/log/journal/`.
- `journalctl --disk-usage`, `vacuum-time`, `vacuum-size`.
- rsyslog basics: `/etc/rsyslog.conf`, `/etc/rsyslog.d/`.
- `logrotate`: `/etc/logrotate.conf`, `/etc/logrotate.d/`.
- Structured logging: `MESSAGE_ID=`, `PRIORITY=`.

**Exercises**
1. Tail the journal for your week-22 service: `journalctl -u myapp -f`.
2. Find every error logged in the last 24 hours: `journalctl -p err --since "24 hours ago"`.
3. Make the journal persistent if it isn't already: create `/var/log/journal`, restart `systemd-journald`. Verify reboot persistence.
4. Limit journal disk use to 500MB: `SystemMaxUse=500M` in `/etc/systemd/journald.conf`.
5. Write a logrotate config for an arbitrary `/var/log/myapp.log`: rotate daily, keep 14, gzip, `notifempty`.

**Mini-project: "Log triage runbook"**
A markdown runbook that walks a junior engineer through diagnosing "the app is broken" using only journalctl and logrotate-managed logs. Include 5 example failure modes and how to find them.

**Self-check**
- How do you ship a journal entry with metadata?
- What does `journalctl -k` show?

---

## Week 24 — Boot Process

**Objectives**
- Understand what happens between power-on and login prompt.
- Diagnose boot failures.

**Topics**
- BIOS/UEFI → bootloader (GRUB) → kernel → initramfs → systemd.
- GRUB basics: `/etc/default/grub`, `update-grub`, kernel parameters.
- initramfs: what it is, when it matters.
- `systemd-analyze`, `systemd-analyze blame`, `systemd-analyze critical-chain`.
- Recovery: single-user mode, GRUB rescue.
- Boot targets: `rescue.target`, `emergency.target`.

**Exercises**
1. Run `systemd-analyze`. Note total boot time. Run `systemd-analyze blame` — identify the slowest service.
2. View kernel command-line: `cat /proc/cmdline`. Match each parameter to its purpose.
3. Add a kernel parameter (e.g., `quiet`) via `/etc/default/grub`, run `update-grub`, reboot, verify.
4. (VM only — snapshot first!) Boot into rescue mode by editing GRUB at boot: append `systemd.unit=rescue.target`. Get a root shell, then `exit` to continue normal boot.
5. Read `dmesg | head -100`. Identify hardware detection messages.

**Mini-project: "Boot-time analysis report"**
For your main lab VM, produce a short report:
- Total boot time, breakdown.
- Top 5 slowest units.
- One service you could reasonably disable to speed boot, with justification.
- Implement the disable, measure improvement.

**Self-check**
- What's initramfs and why do we need it?
- How would you recover a system whose `/etc/fstab` has a broken entry?

---

## Week 25 — **Phase 4 Capstone**

**Project: "Self-managing app deployment"**

Take the Flask "hello world" from week 22 and turn the whole deployment into systemd-managed infrastructure:

- A `myapp.service` running the app as a non-root user with hardening.
- A `myapp-healthcheck.service` + `myapp-healthcheck.timer` that probes `/health` every minute and logs to journal.
- A `myapp-backup.service` + timer that backs up the app's state daily.
- Logrotate config for any file-based logs.
- A `deploy.sh` script that, given a fresh VM, installs everything: deps, user, units, app code, enables timers.

Deliverable: GitHub repo with everything, README, and a screencast of you running `deploy.sh` against a fresh VM and the app coming up healthy.

---

# Phase 5 — Storage & Filesystems (Weeks 26–29)

**Phase outcome:** You can partition disks, manage LVM, choose appropriate filesystems, and configure NFS shares.

## Week 26 — Disks, Partitions, LVM

**Objectives**
- Partition a disk safely.
- Create and grow LVM volumes.

**Topics**
- Block devices: `/dev/sda`, `/dev/nvme0n1`. `lsblk`, `blkid`.
- Partition tables: MBR vs GPT. `parted`, `fdisk`, `gdisk`.
- LVM concepts: PV (physical volume), VG (volume group), LV (logical volume).
- `pvcreate`, `vgcreate`, `lvcreate`, `lvextend`, `vgextend`.
- `/etc/fstab`: UUID-based mounts, options, dump/pass.

**Exercises**
1. (VM only) Add a 5GB virtual disk. Identify it with `lsblk`. Partition with GPT using `parted`.
2. Create a PV on the new partition, a VG, and an LV of 1GB. Format ext4. Mount it.
3. Add to `/etc/fstab` using UUID (not `/dev/sdX`). Reboot. Verify it mounted.
4. Grow the LV by 1GB, then grow the filesystem online with `resize2fs`. Verify with `df -h`.
5. Add a second virtual disk to the VG. Extend an LV onto it.

**Mini-project: "Storage layout for a database server"**
Plan and implement a storage layout for a hypothetical Postgres server:
- `/` (10GB), `/var/log` (5GB), `/var/lib/postgresql` (20GB).
- All on LVM.
- Document why you chose this layout.

**Self-check**
- Why use UUIDs in fstab instead of device names?
- What's the difference between extending an LV and resizing the filesystem?

---

## Week 27 — Filesystems

**Objectives**
- Pick the right filesystem for the job.
- Check, repair, and tune filesystems.

**Topics**
- ext4: default, robust, well-understood.
- xfs: better at large files, can't shrink.
- btrfs/zfs: snapshots, compression, copy-on-write — when worth the complexity.
- Inodes: `df -i`, what "out of inodes but disk has space" means.
- `mkfs.ext4`, `mkfs.xfs`, `tune2fs`, `fsck`, `e2fsck`.
- Mount options: `noatime`, `nodiratime`, `defaults`, `ro`, `discard`.

**Exercises**
1. Format three loopback files as ext4, xfs, btrfs. Mount them. Compare `df` output.
2. Create lots of tiny files until you exhaust inodes on a small ext4 volume. Confirm the failure mode (`No space left on device` despite `df` showing free space).
3. Take a btrfs snapshot, modify files, restore from snapshot.
4. Run `fsck` on an unmounted filesystem. Read the output.
5. Mount with `noatime`, verify with `mount` output. Explain when this matters.

**Mini-project: "Filesystem decision tree"**
A document: given $constraints (small files vs large, snapshots needed, growth requirements, RHEL vs Ubuntu policy), which filesystem do you pick? Include real-world examples.

**Self-check**
- What's an inode?
- Why does xfs lack a shrink operation?

---

## Week 28 — NFS, Bind Mounts, tmpfs

**Objectives**
- Share storage across machines.
- Use bind mounts and tmpfs for special cases.

**Topics**
- NFS server (`nfs-kernel-server`) and client setup.
- `/etc/exports`, `exportfs -ra`, `showmount`.
- Mount options: `rw`, `sync` vs `async`, `no_subtree_check`.
- Bind mounts: `mount --bind /src /dst` and use cases.
- `tmpfs`: `mount -t tmpfs -o size=1G tmpfs /mnt/ram`.

**Exercises**
1. Set up an NFS server exporting `/srv/share` to your subnet. Mount it from another VM. Test read/write.
2. Make the mount persistent in `/etc/fstab`. Reboot. Verify.
3. Bind-mount `/var/log` to `/home/user/logs` (read-only via remount). `cat` a logfile through both paths.
4. Mount a 256MB tmpfs and see how files in it disappear on unmount.
5. Diagnose a stale NFS mount: stop the server, watch the client hang, recover with `umount -l`.

**Mini-project: "Shared scratch space for a build farm"**
NFS-served directory used by 2 client VMs as a shared build cache. Document permissions, exports, mount options. Test concurrent writes.

**Self-check**
- Why is `sync` slower than `async` on NFS, and which does production usually want?
- What's a bind mount good for?

---

## Week 29 — **Phase 5 Capstone**

**Project: "Resilient file server"**

Build a file server VM:
- 2 extra disks combined into an LVM VG.
- LV with xfs, mounted at `/srv/data`.
- Daily snapshot script (LVM snapshots) retained for 7 days.
- NFS exports to your lab subnet, with separate read-only and read-write shares.
- Monitoring script that warns when VG free space < 20%.
- Runbook: how to add another disk and grow the share without downtime.

---

# Phase 6 — Security & Hardening (Weeks 30–34)

**Phase outcome:** You can lock down a production server: SSH, sudo, MAC, audit, intrusion detection.

## Week 30 — Authentication & sudo

**Objectives**
- Manage how users authenticate.
- Configure sudo precisely.
- Understand PAM at a useful depth.

**Topics**
- `/etc/sudoers` syntax, `Defaults`, `Cmnd_Alias`, `User_Alias`.
- `/etc/sudoers.d/` drop-in files.
- `visudo` is mandatory; never edit sudoers directly.
- PAM: `/etc/pam.d/`, stack types (`auth`, `account`, `password`, `session`).
- Lockout policies: `pam_faillock`, `pam_pwquality`.

**Exercises**
1. Create user `deployer` who can run `systemctl restart myapp` but nothing else, no password prompt.
2. Configure `pam_faillock`: 3 failed attempts → 10-minute lockout. Test it.
3. Require password complexity via `pam_pwquality`: 12 chars, mixed case, digit, symbol.
4. Read `/etc/pam.d/sshd`. Identify the auth modules in order. Explain what each does.
5. Add audit logging for sudo: `Defaults logfile=/var/log/sudo.log`. Use sudo. Read the log.

**Mini-project: "Hardened sudoers config"**
A `/etc/sudoers.d/` setup for a team of 5 mock users, each with role-appropriate privileges (admin, deployer, monitor, dba, readonly). Write it as a git-managed config with comments.

**Self-check**
- Why never edit `/etc/sudoers` without `visudo`?
- What does `NOPASSWD` mean and when is it dangerous?

---

## Week 31 — SSH Hardening

**Objectives**
- Lock down SSHD properly.
- Use key-based auth exclusively.
- Detect and rate-limit brute force attempts.

**Topics**
- `/etc/ssh/sshd_config` essentials: `PasswordAuthentication no`, `PermitRootLogin no`, `Port`, `AllowUsers`, `MaxAuthTries`.
- Key types and `~/.ssh/authorized_keys` options (`from=`, `command=`, `no-pty`).
- `fail2ban` setup, jails, `fail2ban-client status sshd`.
- 2FA with `libpam-google-authenticator` (optional but cool).
- `sshd -T` to dump effective config.

**Exercises**
1. Disable password auth and root login. Verify with `sshd -T | grep -E 'passwordauthentication|permitrootlogin'`. Test from another box.
2. Restrict SSH to specific users: `AllowUsers deployer admin`. Test that other users are rejected.
3. Install fail2ban. Configure SSH jail with 3-strikes-in-10-min → 1-hour ban. Trigger it from another machine. Watch the ban.
4. Lock down a key: in `authorized_keys`, prefix with `from="1.2.3.4",command="/usr/local/bin/restricted",no-pty`. Test.
5. Set up TOTP 2FA on a non-prod VM. Get it working, then back it out — you've seen the pattern.

**Mini-project: "Bastion hardening checklist"**
Take your Week-20 bastion. Apply every hardening from this week. Produce a checklist (markdown) other engineers could follow.

**Self-check**
- What's `MaxAuthTries` and how does it differ from `MaxSessions`?
- Why is `PermitRootLogin without-password` slightly safer than `yes` but worse than `no`?

---

## Week 32 — Mandatory Access Control: AppArmor / SELinux

**Objectives**
- Understand what MAC adds beyond DAC (regular permissions).
- Read and modify AppArmor or SELinux policies enough to unblock yourself.

**Topics**
- DAC vs MAC. Why `chmod 700` isn't enough.
- AppArmor (Ubuntu default): profiles in `/etc/apparmor.d/`. `aa-status`, `aa-complain`, `aa-enforce`.
- SELinux (RHEL default): contexts (`ls -Z`), modes (Enforcing/Permissive/Disabled), `getenforce`, `setenforce`, `audit2allow`.
- Reading denials in journal/audit.log.

**Exercises**
1. Run `aa-status` on Ubuntu. List enforced profiles. Pick one (e.g., `nginx`) and read it.
2. Put a profile in complain mode, generate violations (do something the profile would block), see the logs.
3. (Optional, RHEL VM) Toggle SELinux to permissive. Run a service. Use `audit2allow` to generate a policy.
4. Examine SELinux contexts on `/var/www/html` files: `ls -Z`. Explain the type field.
5. Diagnose: nginx fails to read a file in a non-default location. Is it permissions or AppArmor? How do you check?

**Mini-project: "AppArmor profile for a custom binary"**
Take a small binary (your week-22 Flask app, packaged with pyinstaller — or any C "hello world"). Write an AppArmor profile that allows only what it needs. Test enforcement.

**Self-check**
- What's the practical difference between Enforcing and Permissive in SELinux?
- Why won't `chmod 777` always fix a problem on an SELinux-enforcing system?

---

## Week 33 — Audit & Intrusion Detection

**Objectives**
- Track who did what on a system.
- Detect changes to critical files.

**Topics**
- `auditd`: rules in `/etc/audit/rules.d/`, `auditctl`, `ausearch`, `aureport`.
- File integrity: `aide`, `tripwire` (lightweight: `aide`).
- Last-login tracking: `last`, `lastlog`, `wtmp`, `btmp`.
- `lynis` for automated audits.

**Exercises**
1. Add an audit rule that logs every read/write of `/etc/shadow`. Trigger it. Find the events with `ausearch`.
2. Add a watch on `/etc/passwd` for any change. Modify it. Confirm the audit event.
3. Install AIDE. Initialize a baseline. Modify a file under `/etc`. Run `aide --check`. See the alert.
4. Run `lynis audit system`. Read the report. Pick the top 3 findings and remediate at least one.
5. `last -F | head` and `lastb | head`. Identify failed logins.

**Mini-project: "Compliance dashboard"**
A bash script (running daily via systemd timer) that:
- Runs `lynis audit system --quick`.
- Runs `aide --check`.
- Greps audit log for recent privilege escalations.
- Emits a single JSON report.

**Self-check**
- What's the difference between auditd and journald?
- Why store AIDE's database off-host?

---

## Week 34 — **Phase 6 Capstone**

**Project: "CIS-style hardened server build"**

Pick a server role (web server). Build a hardened Ubuntu VM following a checklist drawn from CIS Benchmarks (free for Ubuntu). Include:

- SSH hardening (week 31).
- sudoers config (week 30).
- AppArmor profiles enforced (week 32).
- auditd rules (week 33).
- Firewall (week 19).
- AIDE baseline.
- Lynis score >= 75.

Deliverables:
- GitHub repo with every config + Ansible-or-shell installer (don't worry if you don't know Ansible yet — bash is fine).
- Lynis "before/after" reports.
- Runbook for re-applying after kernel updates.

---

# Phase 7 — Performance & Troubleshooting (Weeks 35–40)

**Phase outcome:** You can systematically diagnose any "the server is slow" complaint and pinpoint the cause.

## Week 35 — Performance Methodology

**Objectives**
- Have a *system* for performance debugging, not just tools.
- Memorize the USE method.

**Topics**
- Brendan Gregg's USE method (Utilization, Saturation, Errors).
- The "60-second performance checklist": `uptime`, `dmesg`, `vmstat`, `mpstat`, `pidstat`, `iostat`, `free`, `sar`, `top`.
- Resource boundaries: CPU, memory, disk, network.
- Establishing a baseline before declaring a problem.

**Exercises**
1. Walk through Gregg's 60-second checklist on your lab box. Write down expected ranges for each metric.
2. Use `stress-ng --cpu 4 --timeout 60s` to load CPU. Run the checklist again. Identify which metrics moved.
3. Repeat with `stress-ng --vm 2 --vm-bytes 1G --timeout 60s` for memory.
4. Repeat with `stress-ng --io 4 --timeout 60s` for I/O.
5. Repeat with network — use `iperf3` between two VMs.

**Mini-project: "USE-method runbook"**
A markdown decision tree: given symptom X (high latency / high CPU / OOM / slow disk), which tools do you reach for and in what order?

**Self-check**
- What's the difference between utilization and saturation?
- Why is "the server is slow" a useless ticket?

---

## Week 36 — CPU & Memory

**Objectives**
- Diagnose CPU saturation, context-switch storms, memory pressure, OOM.

**Topics**
- `top`/`htop` columns: %CPU, %MEM, RES vs VIRT vs SHR, S (state).
- `vmstat 1`: r, b, si, so, us, sy, id, wa, st.
- `mpstat -P ALL 1`, `pidstat 1`.
- `/proc/meminfo`: MemTotal, MemAvailable, Buffers, Cached, SwapTotal, SwapFree.
- OOM killer: `dmesg | grep -i kill`, `/proc/<pid>/oom_score`.
- Swap: when it's fine, when it's a sign of disaster.

**Exercises**
1. Run `vmstat 1 30` while idle. Then while compiling something big. Compare.
2. Use `pidstat -u 1` to find the top CPU consumer. Cross-reference with `htop`.
3. Allocate memory with `stress-ng --vm-bytes 90% --vm 1` until OOM kicks in. Find the OOM event in dmesg.
4. Check `/proc/<pid>/status` for any process: VmRSS, VmSwap. Explain each.
5. Inspect `/proc/cpuinfo` and `lscpu`. Identify physical cores vs threads.

**Mini-project: "CPU/memory triage script"**
A script that, given a hostname, dumps a snapshot of all the CPU and memory diagnostics you'd want to see in a postmortem.

**Self-check**
- What's the difference between RSS and VSZ?
- Is high `%wa` (iowait) a CPU problem or an I/O problem?

---

## Week 37 — Disk I/O

**Objectives**
- Diagnose disk-bound systems.
- Understand the I/O stack from app down to block layer.

**Topics**
- `iostat -xz 1`: `%util`, `await`, `r/s`, `w/s`, `rkB/s`, `wkB/s`.
- `iotop` for per-process I/O.
- `pidstat -d 1`.
- The block layer: schedulers (`mq-deadline`, `none`, `bfq`).
- `lsof` for "what's using this file/disk?"
- `dstat` as a swiss-army monitor.

**Exercises**
1. Run `iostat -xz 1` while idle. Then run `dd if=/dev/zero of=/tmp/big bs=1M count=2000`. Compare.
2. Find the process doing the most I/O on your system right now: `iotop -oP`.
3. List all open files on `/var`: `lsof +D /var | head`.
4. Check the I/O scheduler for your disk: `cat /sys/block/sda/queue/scheduler`. Change it temporarily.
5. Diagnose: a process is "stuck" — is it CPU, memory, disk, or network? Use the methodology, don't guess.

**Mini-project: "I/O-bound failure simulation"**
Create a scenario in your lab: a disk-thrashing background process that's degrading a foreground service. Use the tools to identify and prove the culprit. Write up the postmortem.

**Self-check**
- What's `await` in iostat output and when should you worry?
- What's the practical difference between mq-deadline and none?

---

## Week 38 — Network Performance

**Objectives**
- Measure network throughput and latency.
- Diagnose connection issues at multiple layers.

**Topics**
- `iperf3` for bandwidth tests.
- `mtr` and `traceroute` for path issues.
- `ss -i` for socket-level details.
- `tcpdump` filters: host, port, tcp flags.
- `nstat`, `/proc/net/snmp` for protocol counters.
- TCP retransmits, RTT, congestion.

**Exercises**
1. iperf3 between two VMs over your lab network. Note throughput. Repeat with `-u` for UDP.
2. Find current TCP retransmit rate: `nstat | grep -i retrans`. Trigger some by introducing latency with `tc`.
3. Use `tc qdisc add dev eth0 root netem delay 100ms loss 1%` to simulate a bad link. Re-run iperf3 — observe the cliff.
4. Capture an HTTP request with tcpdump: `tcpdump -i any -A 'tcp port 80'`. Read it.
5. Run `ss -tin` while a transfer is happening. Look at cwnd, rtt, retrans.

**Mini-project: "Network forensics report"**
For a deliberately slow connection (use `tc` to make it bad), produce a report identifying:
- Latency.
- Loss rate.
- Throughput.
- Retransmit count.
- Probable cause (loss vs latency vs congestion).

**Self-check**
- What does TCP cwnd represent?
- Why does packet loss kill throughput more than added latency, on long links?

---

## Week 39 — strace, perf, eBPF intro

**Objectives**
- Trace syscalls of misbehaving programs.
- Use perf for CPU profiling.
- Get a taste of eBPF / `bpftrace`.

**Topics**
- `strace -p <pid>`, `strace -e openat,read,write`, `strace -c` (summary).
- `ltrace` for library calls.
- `perf top`, `perf record -g`, `perf report`, flame graphs (Brendan Gregg).
- `bpftrace` one-liners (`bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s\n", str(args->filename)); }'`).
- `lsof`, `/proc/<pid>/fd/`, `/proc/<pid>/maps`.

**Exercises**
1. strace `ls /tmp` with `-c`. Identify the top syscall by count and time.
2. strace a running web server briefly: `strace -fp <pid> -e openat 2>&1 | head -50`. See what files it's opening.
3. `perf top` while loading the system. Identify the hottest function in the kernel.
4. Generate a flame graph for a CPU-heavy workload using Brendan Gregg's FlameGraph repo.
5. bpftrace one-liner: count `openat()` calls per process for 10 seconds. (This requires bpftrace installed.)

**Mini-project: "Mystery binary analysis"**
Take any random binary on your system (e.g., `/usr/bin/sl` if you install it, or a tiny program you write). Use strace, ltrace, and `/proc/<pid>/` to write a one-page reverse-engineering report: what files does it open, what syscalls does it make, what libs?

**Self-check**
- When does strace slow a program down, and how much?
- What's a flame graph showing on the X axis vs Y axis?

---

## Week 40 — **Phase 7 Capstone**

**Project: "Performance war room exercise"**

Set up a lab VM running:
- nginx + a simple web app.
- A "load generator" (siege, wrk, or `ab`) hitting it from another VM.
- A deliberate problem injected (pick one weekly and rotate): CPU starvation, memory leak, slow disk, packet loss.

For each problem:
1. Reproduce.
2. Use the methodology to diagnose.
3. Identify root cause.
4. Fix it.
5. Write a postmortem (timeline, root cause, fix, prevention).

Deliverable: 4 postmortems in a public repo. These look amazing in interviews.

---

# Phase 8 — Containers & Kubernetes Foundations (Weeks 41–46)

**Phase outcome:** You understand containers from the kernel up, can build production-grade images, and can run a basic Kubernetes workload.

## Week 41 — Container Internals

**Objectives**
- Demystify containers — they're processes, not VMs.
- Understand namespaces and cgroups directly.

**Topics**
- Linux namespaces: `pid`, `net`, `mnt`, `uts`, `ipc`, `user`, `cgroup`.
- `unshare` to create namespaces by hand.
- cgroups v2: `/sys/fs/cgroup/`. Memory and CPU limits.
- chroot vs containers (chroot is *not* a security boundary).
- OCI: image spec, runtime spec.

**Exercises**
1. `unshare --pid --fork --mount-proc bash` — you're now in your own PID namespace. `ps` will show only this shell as PID 1.
2. Create a network namespace: `ip netns add lab`. `ip netns exec lab ip link list`. Note `lo` is down.
3. Connect two netns with a veth pair, assign IPs, ping. (Multiple steps — work through it.)
4. Create a cgroup, set `memory.max=100M`, run `stress-ng --vm-bytes 200M`. Watch it OOM.
5. Read `/proc/self/cgroup` from inside vs outside a container. Compare.

**Mini-project: "Container from scratch"**
A bash script that creates a minimal container using `unshare`, a chroot tarball (e.g., Alpine minirootfs), and cgroup limits. No Docker. You'll really get it.

**Self-check**
- What namespace would you skip if you wanted a process to share network with the host?
- Are containers a security boundary? (Hint: weakly.)

---

## Week 42 — Docker Fundamentals

**Objectives**
- Use Docker confidently for development and production.
- Read what others' Dockerfiles do.

**Topics**
- `docker run` flags: `-d`, `-p`, `-v`, `-e`, `--rm`, `--name`, `--restart`.
- `docker ps`, `logs`, `exec`, `inspect`, `stats`.
- Images vs containers vs volumes. `docker system df`, `docker system prune`.
- Networks: bridge, host, none. `docker network create`.
- `docker compose` basics.

**Exercises**
1. `docker run -d -p 8080:80 nginx`. Hit it. `docker exec` in. Read its config.
2. Run a Postgres container with a named volume. Stop. Remove. Restart with the same volume — data persists.
3. Create a custom bridge network, run two containers on it, have one resolve the other by name.
4. Use `docker stats` to watch CPU/mem of a running container under load.
5. Write a `compose.yaml` for nginx + a backend (your Flask app) + Postgres. `docker compose up`.

**Mini-project: "Local dev stack"**
A `compose.yaml` for any small app you care about: app + DB + reverse proxy + (optional) cache. Healthchecks defined for each. README with startup instructions.

**Self-check**
- What's the difference between a bind mount and a named volume?
- Why is `docker run --restart unless-stopped` usually what you want for long-running containers?

---

## Week 43 — Building Images

**Objectives**
- Write Dockerfiles that produce small, secure, fast-to-build images.
- Understand build context, layers, caching.

**Topics**
- `FROM`, `RUN`, `COPY`, `ADD`, `CMD`, `ENTRYPOINT`, `EXPOSE`, `ENV`, `WORKDIR`, `USER`, `HEALTHCHECK`.
- Multi-stage builds.
- `.dockerignore`.
- Image scanning: `docker scout cves`, `trivy`.
- Distroless and Alpine vs full distros.
- Non-root containers.

**Exercises**
1. Write a Dockerfile for a Python app. Get a working image.
2. Convert it to multi-stage: build dependencies in one stage, copy artifacts to a slim runtime stage. Halve the image size.
3. Run as non-root: add a `USER` directive. Make sure your app still works.
4. Add a `HEALTHCHECK`. Watch it in `docker ps`.
5. Scan with `trivy image yourapp:latest`. Fix the top 3 high-severity findings.

**Mini-project: "Production-quality image for a Flask app"**
Take your Flask app from earlier phases. Produce:
- A multi-stage Dockerfile.
- Image < 100MB.
- Non-root user.
- Healthcheck.
- Trivy scan with no high/critical CVEs.
- A `Makefile` with `make build`, `make run`, `make scan`, `make publish`.

**Self-check**
- Why does layer order matter for caching?
- What's the difference between `ENTRYPOINT` and `CMD`?

---

## Week 44 — Compose, Networks, Registries

**Objectives**
- Orchestrate multi-service apps locally.
- Push to and pull from registries.

**Topics**
- `compose.yaml` deeper: `depends_on` with healthchecks, profiles, env files, secrets.
- Networks across compose projects.
- Docker Hub vs GHCR vs self-hosted registries.
- Image tagging strategies (`latest` is a trap; use immutable tags).
- Multi-arch builds with `buildx`.

**Exercises**
1. Write a compose stack with proper `depends_on: condition: service_healthy`.
2. Push your week-43 image to GHCR using a Personal Access Token. Pull from another machine.
3. Run a local registry: `docker run -d -p 5000:5000 registry`. Push to it. Pull from a second VM.
4. Build a multi-arch image (`amd64`, `arm64`) with `docker buildx`.
5. Tag an image with a semver, a git SHA, and `latest`. Discuss in your notes which to use when.

**Mini-project: "CI-style local pipeline"**
A `Makefile` (or shell script) that, given a code change, builds, tests, scans, tags with git SHA, and pushes to GHCR. Run end-to-end locally.

**Self-check**
- Why is `latest` considered an antipattern in production?
- What does `docker compose --profile dev up` do?

---

## Week 45 — Kubernetes Basics

**Objectives**
- Run a real workload on Kubernetes.
- Understand pods, deployments, services.

**Topics**
- What Kubernetes is and isn't.
- Setup options: `minikube`, `kind`, `k3s`. Recommended for this week: `kind`.
- `kubectl` basics: `get`, `describe`, `logs`, `exec`, `apply`, `delete`, `port-forward`.
- Core objects: Pod, Deployment, Service (ClusterIP, NodePort, LoadBalancer), ConfigMap, Secret, Namespace.
- YAML manifests; `kubectl apply -f`.

**Exercises**
1. Install `kind`. Create a 1-node cluster. `kubectl get nodes`.
2. Apply an nginx Deployment + Service. Port-forward. Hit it.
3. Scale the deployment to 3 replicas. Watch `kubectl get pods`. Kill one — see it self-heal.
4. Create a ConfigMap and a Secret. Mount them as env vars and as files in a pod.
5. Create a namespace. Move your work into it. Practice `-n` and contexts.

**Mini-project: "Flask app on Kubernetes"**
Your Flask app, with:
- Deployment (3 replicas, resource requests/limits, liveness/readiness probes).
- Service.
- ConfigMap for non-secret config.
- Secret for fake DB password.
- Ingress (use `nginx-ingress` in kind).

Deliver: a `k8s/` folder of YAMLs, README with setup steps, screenshot of the app working through Ingress.

**Self-check**
- What's the difference between a Pod and a Deployment?
- Why do you need both liveness and readiness probes?

---

## Week 46 — **Phase 8 Capstone**

**Project: "Containerized 3-service app on K8s"**

Take your Week-25 systemd app and replatform it onto Kubernetes:
- Frontend (static site, served by nginx).
- Backend (your Flask app).
- Database (Postgres with a PVC).
- All deployed with a single `kubectl apply -k` (use Kustomize) or Helm if you prefer.
- Liveness/readiness probes everywhere.
- Resource limits everywhere.
- Network policies restricting backend ↔ DB only.

Deliverable: GitHub repo, README, screencast of fresh kind cluster → app working.

---

# Phase 9 — Observability (Weeks 47–50)

**Phase outcome:** You can answer "what is the system doing?" with logs, metrics, and traces.

## Week 47 — Centralized Logging

**Objectives**
- Get logs off boxes and into a queryable place.
- Search logs effectively.

**Topics**
- Stack options: ELK (Elasticsearch+Logstash+Kibana), Loki+Grafana, Vector.
- Recommended for learning: Loki+Promtail+Grafana (lightweight).
- Log shipping concepts: collection, parsing, indexing, querying.
- Structured logging: JSON over plaintext.
- LogQL basics.

**Exercises**
1. Stand up Loki+Grafana via docker-compose.
2. Configure Promtail (or Vector) on your lab VM to ship `journalctl` to Loki.
3. Query in Grafana: all errors from `myapp` in the last hour.
4. Convert your Flask app to emit JSON logs. Watch the structured fields appear in Grafana.
5. Build a Grafana dashboard panel: error rate per minute for myapp.

**Mini-project: "Logs from everywhere into one place"**
All your lab VMs ship logs to Loki. Build a Grafana dashboard with: error rate, top noisy services, recent SSH logins. Importable JSON in your repo.

**Self-check**
- Why is JSON logging worth the effort?
- What's the practical difference between Loki and Elasticsearch?

---

## Week 48 — Metrics: Prometheus & Grafana

**Objectives**
- Collect and query time-series metrics.
- Write PromQL.

**Topics**
- Prometheus model: scrape targets, labels, time series.
- `node_exporter` for host metrics.
- Application instrumentation (Python `prometheus_client`).
- PromQL: `rate()`, `sum by(...)`, `histogram_quantile()`.
- Alertmanager basics.

**Exercises**
1. Run Prometheus + node_exporter via compose. Scrape your lab VM. See metrics in Grafana.
2. Write PromQL: 5-minute average CPU, p95 disk latency, memory pressure.
3. Instrument your Flask app with `prometheus_client`: request counter, latency histogram. Scrape it.
4. Build a "USE method" dashboard for a host (utilization, saturation, errors per resource).
5. Write an Alertmanager rule that fires when disk > 85%. Test it.

**Mini-project: "Golden-signals dashboard for the Flask app"**
Latency, traffic, errors, saturation. RED method (Rate, Errors, Duration). Importable Grafana JSON.

**Self-check**
- What's the difference between a counter and a gauge?
- Why is `rate(http_requests_total[5m])` better than the raw counter?

---

## Week 49 — Tracing

**Objectives**
- Understand distributed tracing.
- Instrument an app with OpenTelemetry.

**Topics**
- Spans, traces, context propagation.
- OpenTelemetry SDKs.
- Backends: Jaeger (local), Tempo (with Grafana), commercial (Honeycomb, etc.).
- Sampling strategies.

**Exercises**
1. Run Jaeger via docker. Send it some traces from a sample app.
2. Instrument your Flask app with OpenTelemetry. Auto-instrument requests + DB.
3. View a trace end-to-end. Identify the slowest span.
4. Add a custom span around a function you suspect is slow.
5. Set up tail-based sampling so only error traces are kept.

**Mini-project: "Find-the-slow-call exercise"**
Add deliberate latency to one route in your Flask app. Use traces to find which span is slow without reading the code. Write up the workflow.

**Self-check**
- What's the difference between a span and a log line?
- Why is sampling necessary at scale?

---

## Week 50 — **Phase 9 Capstone**

**Project: "Three pillars of observability for the K8s app"**

Building on Week 46:
- Logs: Loki + Promtail (or Grafana Agent) collecting from every pod.
- Metrics: Prometheus scraping node-exporter, kube-state-metrics, your app.
- Traces: OpenTelemetry → Tempo or Jaeger.
- Single Grafana with dashboards for each pillar plus a unified "service overview" page.
- One alert that actually fires on a real issue (kill a pod, see the alert).

---

# Phase 10 — IaC & CI/CD (Weeks 51–56)

**Phase outcome:** You can describe infrastructure as code and ship it through a pipeline.

## Week 51 — Git Deep Dive

**Objectives**
- Stop fearing git.
- Use branches, rebases, and merges fluently.

**Topics**
- Mental model: commits, refs, HEAD, working tree, index.
- Branching, merging, fast-forward vs three-way.
- `git rebase`, `rebase -i`, conflict resolution.
- `git stash`, `git cherry-pick`, `git revert`, `git reset` (`--soft`, `--mixed`, `--hard`).
- `git log` masters: `--oneline --graph --all`, `git log -p`, `git blame`.
- `.gitignore`, signed commits.
- PR/MR workflow.

**Exercises**
1. From scratch: init a repo, make 5 commits across 2 branches, merge with `--no-ff`, view the graph.
2. Practice interactive rebase: squash 3 commits, reorder 2, edit a message.
3. Resolve a deliberate merge conflict.
4. Recover a "lost" commit with `git reflog`.
5. Cherry-pick a commit from one branch to another.

**Mini-project: "Open-source contribution simulation"**
Fork an open-source repo (anything small), branch, make a real fix (typo in README counts), write a commit message that follows conventional commits, push, open a PR. Even if not merged, the workflow is the goal.

**Self-check**
- What's the difference between `git reset --hard` and `git revert`?
- When would you choose merge over rebase?

---

## Week 52 — Configuration Management: Ansible

**Objectives**
- Push idempotent configurations to many hosts.

**Topics**
- Inventory, plays, tasks, modules, handlers, roles.
- Idempotency principle.
- `ansible.cfg`, `hosts.yml`, `group_vars`.
- `ansible-vault` for secrets.
- Common modules: `apt`, `service`, `template`, `copy`, `file`, `user`, `lineinfile`.

**Exercises**
1. Inventory of 2 lab VMs. Ping module. `ansible all -m ping`.
2. Playbook that installs nginx, deploys a templated `index.html`, ensures service is enabled.
3. Convert it into a role with `defaults/`, `tasks/`, `templates/`, `handlers/`.
4. Encrypt a secret value with `ansible-vault`.
5. Re-run the playbook 3 times — verify nothing changes (idempotency).

**Mini-project: "Replace your bash bootstrap with Ansible"**
Take your Phase-1 capstone (week 8) — that "new dev box" bash script. Rewrite it as an Ansible role. Apply it to a fresh VM. Compare maintainability.

**Self-check**
- What does idempotent mean and why is it critical?
- What's the difference between a role and a playbook?

---

## Week 53 — Infrastructure as Code: Terraform

**Objectives**
- Provision cloud resources declaratively.
- Manage state.

**Topics**
- Resources, providers, variables, outputs, modules.
- State: local vs remote (S3+DynamoDB or Terraform Cloud).
- `terraform init / plan / apply / destroy`.
- Drift, importing existing resources.
- Suggested provider for learning: a free tier on a cloud you have (AWS/GCP/Azure) or a local provider like Hetzner / DigitalOcean for cheap, or `kreuzwerker/docker` provider for local-only practice.

**Exercises**
1. Write Terraform that creates 2 Docker containers on your local box (using the docker provider). Apply, destroy.
2. Refactor into a module taking a `count` variable.
3. Set up remote state in an S3 bucket (or local backend with documentation).
4. Make a change manually outside Terraform, run `plan`, see drift.
5. `terraform import` an existing resource (a manually-created Docker container) into state.

**Mini-project: "Lab in Terraform"**
Provision your full Phase-3 lab (bastion + web + db) in code. If you can afford $5 of cloud spend for a few hours, do it on a real cloud. Otherwise use the docker provider locally with `compose.yaml`-equivalents.

**Self-check**
- What's the state file and why does it matter?
- When do you use a module vs just resources?

---

## Week 54 — CI/CD Concepts

**Objectives**
- Understand pipelines, runners, artifacts, environments.

**Topics**
- CI vs CD vs CD (deployment vs delivery).
- Pipelines: stages, jobs, dependencies.
- Runners (shared vs self-hosted).
- Artifacts and caching.
- Secrets management in CI.
- Environments and approvals.
- GitHub Actions vs GitLab CI vs Jenkins — pick GitHub Actions for this week.

**Exercises**
1. A GitHub Actions workflow that runs on push: lint a Python project, run tests, fail loud.
2. Add caching for pip dependencies. Compare wall time.
3. Build a Docker image and push to GHCR on tag push.
4. Add a manual approval before "deploy" using GitHub Environments.
5. Use a self-hosted runner on your VM.

**Mini-project: "Pipeline for the Flask app"**
GitHub Actions workflow:
- On PR: lint, unit tests.
- On merge to `main`: build image, scan with trivy, push to GHCR with git-SHA tag.
- On version tag: also tag image with semver, deploy to "staging" (your kind cluster via runner).

**Self-check**
- What's the difference between an artifact and a cache?
- Why use immutable image tags in CD?

---

## Week 55 — Pipeline Design & GitOps

**Objectives**
- Design pipelines that fail safe.
- Try a GitOps tool.

**Topics**
- Trunk-based vs GitFlow.
- Progressive delivery: canary, blue-green.
- GitOps with Argo CD or Flux.
- Pipeline patterns: build once / deploy many; promotion through environments.
- Rollback strategies.

**Exercises**
1. Add a "promote to prod" workflow that re-tags the staging-tested image.
2. Install Argo CD into your kind cluster. Point it at a manifests repo.
3. Push a manifest change. Watch Argo CD sync.
4. Simulate a bad deploy: deliberately break the manifest. Practice rollback (git revert + sync).
5. Add a canary using a simple traffic-splitting trick (Argo Rollouts is overkill for week 55 — describe how it would extend your setup).

**Mini-project: "GitOps for the K8s app"**
Your week-46/50 K8s app, deployed exclusively via Argo CD from a `manifests-repo`. CI updates manifests with new image tags; CD picks it up.

**Self-check**
- What's GitOps — name three properties.
- Why "build once, deploy many"?

---

## Week 56 — **Phase 10 Capstone**

**Project: "End-to-end IaC + CI/CD"**

For the Flask + Postgres app from Phase 8:
- Terraform: provisions whatever cluster (kind locally is fine, or a real cloud).
- Ansible: any base configuration if applicable (e.g., bastion).
- GitHub Actions: lint → test → build → scan → push image; PR previews if you're feeling fancy.
- Argo CD: continuously syncs Kubernetes manifests from git.
- Observability stack from Phase 9 already in place.
- README walks a stranger through reproducing it from zero in < 1 hour.

This is your **flagship portfolio project**. Spend extra time making it look great.

---

# Phase 11 — Portfolio Capstone (Weeks 57–60)

**Phase outcome:** A polished personal infrastructure that demonstrates everything, plus a job-search-ready résumé.

## Week 57 — Plan & Polish the Showcase

- Re-read every project README. Tighten language, add architecture diagrams.
- Pick **3 flagship projects** for your résumé:
  1. Hardened server build (Phase 6 capstone).
  2. K8s Flask app with full observability (Phases 8 + 9).
  3. End-to-end IaC + CI/CD (Phase 10).
- Each gets: clear README, architecture diagram, screencast (asciinema for terminal, OBS for GUI), live demo if possible.

## Week 58 — Build "homelab-platform"

A meta-repo that ties everything together:
- Terraform brings up the infrastructure.
- Ansible bootstraps it.
- Argo CD takes over from there.
- One command from clone to working platform.

## Week 59 — Document & Write

- A blog post per flagship project (Medium / your own static site / dev.to).
- A "what I built" post tying the whole roadmap together.
- Update LinkedIn with skills, projects, articles.
- Update résumé: emphasize *what you built and the impact*, not tools listed.

## Week 60 — Interview Prep

- Walk through every flagship project as if explaining to an interviewer (record yourself).
- Practice with classic SRE interview questions: "what happens when you type `google.com` and press enter," "design a healthcheck," "what would you do if a server's load is 50."
- Mock interviews with a friend or a service like interviewing.io.
- Apply to roles. You're ready.

---

# Daily / weekly habits to keep forever

- **Read** something Linux-related daily. Hacker News, lobste.rs, Brendan Gregg's blog, Julia Evans' zines.
- **Write** weekly. Even if it's just notes-for-yourself. Writing crystallizes understanding.
- **Practice** in your lab. Break things on purpose. Recover.
- **Lurk** in the right communities: r/sysadmin, r/devops, r/kubernetes, the CNCF Slack.
- **Subscribe** to one good newsletter: SRE Weekly, DevOps Weekly, Last Week in Kubernetes.

# Books worth owning (not all at once)

- *The Linux Command Line* — William Shotts (free PDF). Read in Phase 1.
- *How Linux Works* — Brian Ward. Read in Phase 4.
- *Systems Performance* — Brendan Gregg. Read alongside Phase 7.
- *Site Reliability Engineering* — Google (free online).
- *The Phoenix Project* — Gene Kim. Fiction-but-true.
- *Designing Data-Intensive Applications* — Martin Kleppmann. Read in Phase 9–10.

# When in doubt

- `man <command>` first.
- `<command> --help` second.
- Search the Arch Wiki — best Linux documentation on the internet, even for Ubuntu users.
- Stack Overflow / Server Fault.
- Ask, when stuck for >30 minutes — but always show what you've tried.

---

**Final word:** This roadmap is a *guide*, not a contract. If a topic clicks faster, move on. If something resists, slow down and dig in. The point isn't to finish in 60 weeks — it's to genuinely *know* this material. Skipping for speed defeats the purpose.

Good luck. See you on the other side.
