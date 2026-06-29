# Phase 1 Practice — Linux Foundations Mastery Exam

> Source material: [`Linux/Phase1-Foundation/`](../Linux/Phase1-Foundation/README.md), backed by
> [linux-mastery-roadmap.md](../resources/roadMap/linux-mastery-roadmap.md) and
> [linux-mastery-roadmap-detailed.md](../resources/roadMap/linux-mastery-roadmap-detailed.md).

---

## Table of Contents

0. [System & Environment Info](#0-system--environment-info)
1. [Filesystem Hierarchy & Navigation](#1-filesystem-hierarchy--navigation)
2. [File Operations, Links & Globbing](#2-file-operations-links--globbing)
3. [Text Processing & Regex](#3-text-processing--regex)
4. [Process Management & Job Control](#4-process-management--job-control)
5. [Users, Groups & Permissions](#5-users-groups--permissions)
6. [Package Management](#6-package-management)
7. [Editors, Shell Config & tmux](#7-editors-shell-config--tmux)
8. [Phase 1 Capstone — "Day One as the Linux Admin"](#8-phase-1-capstone--day-one-as-the-linux-admin)
9. [Oral Drill — Explain It Without a Terminal](#9-oral-drill--explain-it-without-a-terminal)

---

## 0. System & Environment Info

Notes: [0_info.md](../Linux/Phase1-Foundation/0_info.md)

### 🟢 Recall

1. What's the difference between `uname -a` and `uname -r`? Which fields does `-a` add on top of `-r`?
2. What does `hostnamectl` show that `hostname` alone doesn't?
3. Name two ways to find your distro name and version without opening a browser.
4. What's the difference between `whoami` and `id`? Which one tells you the groups you belong to?
5. What does `arch` report, and how does it relate to `uname -m`?
6. What does `env` print, and how is that different from `set` (no flags) in bash?

### 🛠 Hands-on

7. A new teammate SSHs into a box you've never seen and asks "what is this thing?" Produce, with a
   single command each: OS name+version, kernel version, CPU architecture, hostname, and how long
   it's been up. Write down the five commands.
```bash
hostnamectl
```
8. Read `/proc/cpuinfo` and `/proc/meminfo` directly (no `lscpu`/`free`). Find: number of logical
   CPUs, total RAM, and the CPU model name, using only `cat`/`grep`/`wc` on those two files.
```bash
cat /proc/cpuinfo | grep processor | wc -l
# 16

# To get the CPU Model
cat /proc/cpuinfo | grep -m 1 "model name"

cat /proc/meminfo
```
9. Run `uptime` and interpret the three load-average numbers against the CPU count you found above.
   Is the box currently under-loaded, fine, or overloaded? Justify the number, don't guess.
```bash
# Refer to the cpu.md or system_laod.md to know more about cpu and system load

uptime # to check the system load 1 5 15 min intervaled
```
### 🧩 Scenario

10. A script you inherited checks `cat /etc/issue` to detect the distro and breaks on a new server.
    What's wrong with using `/etc/issue` for this, and what file should it parse instead, and why is
    that file more reliable?
- `cat /etc/issue`: is a text file on Unix/Linux systems that displays a pre‑login message—typically system identification such as OS name, version, or kernel—before the login prompt appears.
- `cat /etc/issue.net` – Similar purpose but used for network logins (e.g., SSH, telnet). If configured, SSH displays `/etc/issue.net` instead of `/etc/issue`.
- To get the proper os information `cat /etc/os-release`

---

## 1. Filesystem Hierarchy & Navigation

Notes: [1_file_system.md](../Linux/Phase1-Foundation/1_file_system.md), [2_cli_basics.md](../Linux/Phase1-Foundation/2_cli_basics.md)

### 🟢 Recall

1. What's the difference between an absolute and a relative path? Give one example of each starting
   from `/var/log`.
2. What does `~` expand to? What does `cd -` do, and how is it different from `cd ..`?
3. Name the directory where you'd expect to find: a user's personal files, system-wide config, the
   kernel image, runtime PID/socket files, and device files. (Five answers, five different dirs.)
4. What is `/proc` and why does `cat /proc/cpuinfo` show real data even though the file is "empty" on
   disk?
5. Why does `/etc` exist as a separate tree from `/var`? State the one property that separates them.
6. What's the practical difference between `/tmp` and `/var/tmp`?
7. Why is `/root` not a subdirectory of `/home`, even though it's "a user's home directory"?
8. What's `/srv` for, conceptually, and why do many real servers put website files in `/var/www`
   instead?
9. Explain the historical reason `/bin` and `/usr/bin` both exist, and what `/bin -> /usr/bin` being a
   symlink tells you about a modern system.
10. What's the difference between `!!`, `!5`, and `!ls` in bash history expansion?

### 🛠 Hands-on

11. From your home directory, run `ls -la`. Pick three dotfiles you didn't create yourself (i.e.
    shipped by default) and explain, in your own words, what each one is for.
12. `cd` into `/etc`, `/var/log`, `/proc/1`, `/sys/class/net`, and `/dev`. After each, run `pwd` and
    `ls`, and write one sentence per directory describing what actually lives there on your box.
13. Find the `ls` flag that sorts by modification time, **newest last**. Demonstrate it on `/etc`.
14. Install `tree` if it's missing, then run `tree -d -L 1 /` and `tree -L 2 /etc`. Which top-level
    directories are actually symlinks on your system?
15. Run `history`, find a command you ran more than 10 entries ago, and re-execute it using `!N`
    without retyping it.

### 🧩 Scenario

16. A disk fills up unexpectedly overnight with no obvious user activity. Which top-level directory do
    you check first, and why is that the *first* place rather than `/home`?
17. You're told "this server's `/usr` is mounted read-only for security." Which two other directories
    almost certainly must stay writable for the system to boot and run normally, and why?
18. A junior admin runs `rm -rf /` by accident on a test box and is confused why nothing visibly broke
    for the first few seconds. Explain, using the FHS, roughly what category of breakage happens
    first vs. last.

---

## 2. File Operations, Links & Globbing

Notes: [3_files_operations.md](../Linux/Phase1-Foundation/3_files_operations.md)

### 🟢 Recall

1. List four ways to create a file from the shell without an editor (e.g. `touch`, `echo >`, ...).
2. What's the difference between `cp file1 file2` and `cp -p file1 file2`? Name the three attributes
   `-p` preserves.
3. When does `cp` require `-r`, and what happens if you forget it?
4. What's the difference between `rm -i`, `rm -f`, and plain `rm` when the target file doesn't have
   write permission for you?
5. What does `rmdir` refuse to do that `rm -r` will do without complaint?
6. Difference between a hard link and a symbolic link — list at least four distinguishing facts (inode
   sharing, crossing filesystems, linking to directories, behavior when the original is deleted).
7. What does the inode number tell you, and how do you view it for a file with `ls`?
8. What does the trailing `+` in `-rw-r--r--+ 1 user group ...` (from `ls -l`) indicate?
9. Difference between `du -sh dir` and `df -h` — what question does each one actually answer?
10. What does `stat` show you that `ls -l` doesn't?

### 🛠 Hands-on

11. Build this tree under `~/lab/phase1/` using a **single** `mkdir -p` call, then verify with `tree`:
    ```
    project/
    ├── src/{app,lib,utils}/
    ├── tests/{unit,integration}/
    └── docs/
    ```
12. Create 30 numbered files with `touch file{01..30}.txt`. Using a single glob (no loop, no `find`),
    move only the even-numbered ones into a new `even/` directory.
13. Create a symlink `ln -s /etc/hostname ~/myhost`, `cat` it, then `ls -l` it and explain the arrow
    and the size shown for the symlink itself versus the target.
14. Create a hard link to a file, edit the **original**, and show the change appears through the
    hard-linked name too. Then delete the original and show the data still exists via the link.
15. Try to hard-link a file across two different mounted filesystems (or `/` vs `/tmp` if `/tmp` is a
    separate mount on your box — check with `mount` or `df` first). What error do you get, and why is
    it a hard limitation rather than a bug?
16. Run `stat /etc/passwd`. Identify and write down: size, inode number, permission string, UID/GID of
    owner, last access time, last modification time, last status-change time. State in your own words
    what's different between "modify" and "change" time.
17. Create a file, `chmod 600` it, switch to a different user (or simulate with `sudo -u`), and try to
    `cat` it. What's the exact error, and which permission bit caused it?
18. Use `getfacl` on any file, then grant an extra user read+write access with `setfacl -m`, confirm
    with `ls -l` (look for the `+`) and `getfacl` again, then remove the ACL entry with `setfacl -x`
    and the whole ACL with `setfacl -b`.

### 🧩 Scenario

19. `rm *` is run inside a directory containing a file literally named `-rf`. What actually happens,
    and why should that worry you? How would you safely delete a file whose name starts with a dash?
20. You need to copy 5,000 small config files preserving timestamps and permissions exactly, into a
    backup directory, in one command. Which `cp` flags do you reach for and why?
21. A teammate says "I deleted the file but `df` still shows the space as used." Using what you know
    about inodes and links, give the most likely explanation (hint: think about what else can hold a
    reference to an inode besides a directory entry).

### 🚀 Challenge

22. Write one command that finds every regular file under `/etc` larger than 0 bytes whose name ends
    in `.conf`, and lists them sorted by size, largest first, showing only size and path. (You may
    combine `find`, `ls`, `du`, `sort` — whatever gets there.)

---

## 3. Text Processing & Regex

Notes: [4_text_processing.md](../Linux/Phase1-Foundation/4_text_processing.md), plus the linked `grep`/`sed`/`awk`/`cut`/`sort`/`uniq`/`tr`/`diff`/`wc`/`xargs`/`find` command pages and the [regex](../Linux/concepts/text-patterns/regex.md) / [redirection](../Linux/concepts/shell-terminal/redirection.md) / [process substitution](../Linux/concepts/shell-terminal/process_substitution.md) concept pages.

### 🟢 Recall — redirection & pipes

1. What's the difference between `>` and `>>`?
2. What does `2>&1` actually mean, read left to right? Why does `command > file 2>&1` work but
   `command 2>&1 > file` send stderr to the terminal instead of the file?
3. What's `&>` shorthand for?
4. What does `command > /dev/null 2>&1` accomplish, and why would you want it?
5. Why is `cmd | tee file` useful over `cmd > file`? What problem does it solve?
6. What is process substitution (`<(...)`)? Give one real use case where it beats a temp file.
7. What's a here-string (`<<<`)? What's a heredoc (`<<EOF`)? When would you reach for each?

### 🟢 Recall — grep / regex

8. What does `grep -v` do? `grep -w`? `grep -c`? `grep -l` vs `grep -L`?
9. Why does `grep foo bar` (no `-r`) sometimes act weird or error out? What's `bar` being treated as?
10. What's the difference between basic regex (default `grep`) and extended regex (`grep -E`)? Name
    one metacharacter that needs escaping in one but not the other.
11. In regex, what do `^`, `$`, `.`, `*`, `+`, `?`, `[...]`, `[^...]`, and `|` each mean?
12. What's a POSIX character class like `[[:digit:]]` or `[[:alpha:]]`, and why use one instead of
    `[0-9]` or `[a-zA-Z]`?
13. How would you `grep` for the literal string `192.168.1.1` without the `.` matching "any
    character"?

### 🟢 Recall — sed / awk / cut / sort / uniq / tr / wc / diff / xargs

14. What's the difference between `sed 's/x/y/'` and `sed 's/x/y/g'`?
15. What does `sed -i` do, and what's the one risk of using it without a backup?
16. How do you print only lines 5 through 10 of a file with `sed -n`? How would you do the same thing
    with `awk`?
17. In `awk`, what do `$0`, `$1`, `NF`, and `NR` each refer to?
18. How do you tell `awk` to use a colon as the field separator? Give the exact flag.
19. What does `cut -d',' -f1,3` do?
20. What's the difference between `sort` (default) and `sort -n`? Show one input where they disagree.
21. Why must input be sorted before piping into `uniq` to get correct duplicate counts?
22. What does `uniq -c` add to its output? What does `uniq -d` show that plain `uniq` doesn't?
23. What does `tr -d '\n'` do? What does `tr -s ' '` do?
24. What do the `<`, `>`, `c`, `a`, `d` symbols mean in classic `diff` output?
25. What does `wc -l`, `wc -w`, and `wc -c` each count? Which one differs from `wc -m` and how?
26. Why is `xargs` often preferred over `find -exec ... \;` for large numbers of files? What does
    `find -print0 | xargs -0 ...` protect you against that plain `xargs` doesn't?

### 🛠 Hands-on

27. From `/etc/passwd`, list every username whose default shell is `/bin/bash`, as a one-liner using
    `awk` with `:` as the field separator.
28. From `/etc/passwd`, list usernames with UID >= 1000 (i.e. real human accounts, not system
    accounts), sorted numerically by UID.
29. Take a copy of `/etc/hosts` and use `sed` to replace every occurrence of `localhost` with
    `myhost.local`, **in place**, then `diff` the copy against a fresh copy you saved beforehand to
    confirm exactly what changed.
30. Use `grep -rEn 'TODO|FIXME' .` against any repo you have locally (this one works fine). Pipe the
    result into `wc -l` to get a single count.
31. Pick any log-like file you have (`/var/log/*.log`, `dpkg.log`, or this repo's own files) and write
    a one-liner that prints the top 10 most frequent first-words-on-a-line, with counts, sorted
    descending. (Hint: `awk '{print $1}' | sort | uniq -c | sort -nr | head`.)
32. Write a `grep -E` pattern that pulls every IPv4-looking address out of a text blob you create with
    a mix of real IPs (`10.0.0.5`), fake ones (`999.1.1.1`), and noise text. Test it and note any false
    positives your pattern lets through (a "loose" IPv4 regex is a known trap — see if you can spot
    why and what a stricter version would need).
33. Write a `grep -E` pattern that extracts email-looking strings from a paragraph you write containing
    at least three valid emails and two near-misses (e.g. missing `@`, double dots).
34. Using `find` + `xargs`, gzip every `.txt` file under a test directory in one pipeline, safely
    handling filenames containing spaces.
35. Build a small CSV (5 rows, 3 columns) by hand. Use `cut` to extract column 2 only, then use `awk
    -F','` to do the same thing, and confirm both give identical output.
36. Use `tee` to run a command, see its output on screen, **and** save it to a file in one shot. Verify
    the file's contents match what you saw on screen.

### 🧩 Scenario

37. A teammate's `sed -i 's/foo/bar/' *.conf` corrupted every file because `foo` matched more than they
    expected. What two things should they have done first to avoid this (one about testing the regex,
    one about not editing in place blind)?
38. You need to count failed SSH login attempts per source IP from an auth log, sorted by count
    descending, top offender first. Walk through the full pipeline of commands you'd chain together,
    explaining what each stage does, even though you may not have a populated `auth.log` to test
    against right now.
39. Why does `grep "pattern" file1 file2` print the filename before each match, but `grep "pattern"
    file1` alone doesn't? What flag forces the filename to always show, and what flag forces it to
    never show?
40. You ran `command1 | command2` and `command2` failed, but the pipeline's exit code (`$?`) came back
    `0`. What's going on, and what shell option fixes this (just name it — implementing it is Phase 2)?

### 🚀 Challenge

41. Build a "log triage" one-liner kit of your own: write five one-liners (command + one-sentence
    purpose each) you'd actually keep on hand — e.g. top IPs by hits, error count per hour, slowest
    lines, distinct values in a column, lines without a match. Test each against a file you create.
42. Given a file of `key=value` lines (some with extra whitespace, some commented out with `#`), write
    a single pipeline that: strips comments and blank lines, trims whitespace around `=`, and outputs
    clean `key=value` pairs sorted by key.

---

## 4. Process Management & Job Control

Notes: [5_process_management_job_control.md](../Linux/Phase1-Foundation/5_process_management_job_control.md)

### 🟢 Recall — lifecycle & signals

1. Why is there no "create a process from nothing" syscall? What two calls does "running a program"
   actually decompose into, and what does each one do?
2. `fork()` returns twice. What does it return in the parent, and what does it return in the child,
   and how does that let each side know who it is?
3. What's copy-on-write, and why doesn't `fork()`-ing a 10 GB process actually copy 10 GB immediately?
4. What is `wait()`/`waitpid()` doing conceptually? What's the term for a parent collecting a child's
   exit status?
5. What exactly is a zombie process? Why can't `kill -9` remove one, and what's the *actual* fix?
6. What is an orphan process? Why don't orphans become permanent zombies?
7. What three things make PID 1 special?
8. What's a process group, and why does pressing Ctrl-C in a multi-stage pipeline (`a | b | c`) stop
   all three commands instead of just one?
9. What's a session, and what's special about the relationship between a session and its controlling
   terminal?
10. What signal does the kernel send when a terminal/SSH connection drops, and why does that kill your
    background jobs by default?
11. Name the signal number and default action for: SIGHUP, SIGINT, SIGKILL, SIGTERM, SIGTSTP,
    SIGCHLD, SIGCONT.
12. Which two signals can never be caught, blocked, or ignored by a process, and why does that matter
    operationally?
13. Why is `kill -9` considered a last resort rather than a first choice? What can it break that
    `kill -15` doesn't?
14. What process state is `D`, and why won't `kill -9` terminate a process stuck in it?

### 🟢 Recall — inspecting & controlling processes

15. Difference between `ps aux` and `ps -ef`? Why do both exist (historical reason)?
16. What does `pstree` show that `ps` doesn't make obvious?
17. What's the difference between `pgrep` and `pidof`?
18. What does `top`'s load average line tell you, and how do you decide if "1.00" is healthy or
    concerning — what's the missing piece of context you need?
19. What's the difference between `nice` and `renice`? What's the valid nice value range, and which
    direction is "higher priority"?
20. Who is allowed to set a *negative* nice value, and why is that restricted?
21. What does `ionice` control that `nice` doesn't?

### 🟢 Recall — job control

22. What's the difference between a foreground job and a background job in terms of terminal
    ownership?
23. What does Ctrl-Z actually send, and what's the difference between that and Ctrl-C?
24. What do `jobs`, `fg`, `bg`, and `disown` each do? Give the job-spec syntax for "the second job"
    and "the most recently backgrounded job."
25. What's the practical difference between `nohup` and `disown`? When would you reach for one over
    the other?
26. If you don't redirect output and just background a command (`./script.sh &`), where does its
    stdout/stderr go, and what's the gotcha if you log out?

### 🛠 Hands-on

27. Run `sleep 600 &` three times. List the jobs with `jobs -l`. Bring one to the foreground, suspend
    it with Ctrl-Z, send it back to the background, then kill all three by PID (not by job spec).
28. Install and open `htop`. Sort by memory, identify the top 3 consumers, then filter to processes
    owned by your own user only.
29. Run `yes > /dev/null &` (this will peg a CPU core — plan to kill it). Watch CPU usage rise in
    `top`/`htop`. Send it `SIGTERM`, then confirm with `pgrep yes` that it's actually gone.
30. Start `nice -n 19 sha256sum /dev/zero &` and, separately, a default-priority `sha256sum /dev/zero
    &`. Compare their CPU share in `top`. Kill both when you're done watching.
31. Start a long-running command in the background, `disown` it, then close and reopen your terminal
    (or start a fresh SSH session). Confirm with `ps` that it's still running.
32. Write down, from memory and then verified against `man ps`, 10 `ps` invocations you'd actually use
    day-to-day: by user, by name, with parent PID, sorted by CPU, sorted by memory, in tree form,
    custom columns, etc.
33. Find a real zombie or simulate one: write a tiny shell pipeline or use a known trick to produce a
    `Z`-state process (or research how one is typically produced if you can't safely make one right
    now), and explain, using `ps -el` or `/proc/<pid>/status`, how you'd confirm it's a zombie and
    identify its parent.

### 🧩 Scenario

34. You get paged because a server's load average is climbing. `uptime` shows `8.50, 6.20, 3.10` on a
    4-core box. Walk through, in order, what that trend (1-min > 5-min > 15-min, all above core count)
    tells you, and what you'd check next.
35. A process won't die. You've sent `SIGTERM`, waited, then sent `SIGKILL`, and it's still in `ps`
    output in state `D`. What's actually happening, and what's the real-world fix (hint: it's not a
    bigger hammer than `-9`)?
36. Explain why `kill -9` on a process holding a database write lock or a temp file is riskier than
    `kill -15` on the same process, in terms of what each signal does and doesn't let the process do.
37. A developer says "I ran my deploy script over SSH, disconnected, and now it's dead — but I used
    `&` to background it!" What's missing from their command, and what are the two different fixes
    (one proactive, one you can apply after the fact if the process were still alive)?

### 🚀 Challenge

38. In one terminal, build a pipeline of three stages that runs for a while (e.g. `yes | sort |
    uniq -c &`, or similar, something safe to kill). In another terminal/pane, use `ps -o
    pid,ppid,pgid,sid,tty,stat,cmd` to find the PGID and confirm all three stages share it, then kill
    the *whole group* with one `kill` invocation using a negative PID. Explain why a negative PID
    targets a group.

---

## 5. Users, Groups & Permissions

Notes: [6_users_groups_permissions.md](../Linux/Phase1-Foundation/6_users_groups_permissions.md)

### 🟢 Recall — identity & accounts

1. What's the difference between a UID and a username? Where does the kernel actually care about one
   versus the other?
2. Name the seven colon-separated fields in `/etc/passwd`, in order.
3. What does `/etc/shadow` store that `/etc/passwd` historically used to, and why was it split out?
4. What's a primary group versus a secondary group? Which one determines the group ownership of a
   newly created file?
5. What's `/etc/skel` for?
6. What's the difference between `vipw`/`vigr` and just opening `/etc/passwd`/`/etc/group` in `vim`
   directly? What risk does the former protect you from?
7. What does `/var/log/auth.log` (or your distro's equivalent) record?

### 🟢 Recall — permissions

8. What do the three permission groups (owner/group/other) and three bits (r/w/x) each control on a
   **file** versus on a **directory**? (Execute on a directory means something different from execute
   on a file — be specific.)
9. Convert `rwxr-x---` to octal. Convert `640` to symbolic form.
10. What's the difference between `chmod 755 file` and `chmod u+x,go+rx file` as starting points —
    when do they produce different results?
11. What does `umask 022` produce for a new file versus a new directory, starting from the defaults
    of `666` and `777`? Show the subtraction.
12. Why can `umask` only ever remove permissions, never add them?
13. Why is `/etc/shadow` mode `640` (or stricter) instead of `644`?
14. What's the difference between `chown` and `chgrp`? When would `chown user:group file` be
    preferable to two separate commands?

### 🟢 Recall — special bits & ACLs

15. What does the SUID bit do when set on an executable? Give one real binary that needs it and
    explain why.
16. What does SGID do differently when set on a **file** versus on a **directory**?
17. What does the sticky bit do, and why is `/tmp` mode `1777`?
18. In `ls -l` output, what's the difference between a lowercase `s`/`t` and an uppercase `S`/`T` in
    the permission string?
19. Write the `find` command (using `-perm`) that locates every SUID binary on the system.
20. What does a `+` at the end of a permission string (e.g. `-rw-r--r--+`) tell you, and which command
    do you run to see the actual extra entries?

### 🟢 Recall — sudo

21. Why should you always edit sudoers with `visudo` rather than a regular editor?
22. What does `NOPASSWD` mean in a sudoers rule, and why is it specifically dangerous if scoped too
    broadly?
23. What's the benefit of dropping rules into `/etc/sudoers.d/` instead of editing `/etc/sudoers`
    directly?

### 🛠 Hands-on

24. Create users `alice` and `bob`, plus a group `devs`. Add both users to `devs` as a secondary
    group. Verify membership with `id alice` and `groups bob`.
25. Create `/srv/shared`, owned by `root:devs`, mode `2775`. Have `alice` create a file inside it, then
    check the new file's group ownership. Explain, using what SGID does on a directory, why the file
    inherited `devs` instead of `alice`'s primary group.
26. Create a file as yourself, `chmod 600` it, then attempt to read it as a different user (`sudo -u
    bob cat ...` or similar). Capture the exact permission-denied error.
27. Run `find / -perm -4000 -type f 2>/dev/null` (redirecting stderr so permission-denied noise from
    directories you can't read doesn't flood the screen — and note *why* that redirect is needed).
    Pick three results (e.g. `passwd`, `sudo`, `ping`) and explain, for each, why it specifically needs
    SUID to function for a non-root user.
28. Set up a directory `/srv/team/` with two subdirectories, `engineering/` and `design/`, each
    readable/writable only by a matching group of the same name, plus a shared `inbox/` where anyone
    can drop a file but only the file's owner (not just anyone in the group) can delete it. Document
    every `chmod`/`chown`/`mkdir` command and explain which bit makes the "only owner can delete"
    behavior possible.
29. Use `visudo` to grant `alice` passwordless permission to run exactly one command (e.g. `systemctl
    status nginx` or any safe read-only command) and nothing else. Test as `alice` that the allowed
    command works without a password prompt and a different command still asks for one or is denied.
30. Pick a file, run `getfacl` on it, grant a second user explicit read+write access with `setfacl -m
    u:<user>:rw`, and confirm the change shows up both in `ls -l` (the `+`) and in `getfacl` output.

### 🧩 Scenario

31. A file is `-rw-r--r-- 1 root devs script.sh` and a member of `devs` complains they can't execute
    it even though "the group has access." Spot every reason this could fail, not just the obvious
    one.
32. Two engineers both need to edit files in a shared project directory, and you need new files
    created there to automatically belong to the project's group rather than each creator's personal
    group. Which directory mode bit solves this, and what do you set the directory's group ownership
    to first?
33. You're asked to let a deploy user restart exactly one systemd service via sudo, with no shell
    access otherwise. What sudoers line accomplishes this, and what's the security risk if the
    "exactly one command" path is something like `/usr/bin/systemctl restart *` instead of a fully
    qualified, argument-locked command?
34. `chmod 777` is applied to "fix" a permission-denied error on a config file, and it works — but a
    senior engineer flags it in review. What's actually wrong with this fix even though it solved the
    immediate symptom?

### 🚀 Challenge

35. Design and document, end-to-end, a multi-user shared workspace for a 5-person team with three
    roles — admin (full control), contributor (read/write, can't delete others' files), and viewer
    (read-only) — using only groups, ownership, octal permissions, and the sticky bit (no ACLs).
    Write the exact commands and explain each role's access in one sentence.

---

## 6. Package Management

Notes: [7_package_management.md](../Linux/Phase1-Foundation/7_package_management.md)

### 🟢 Recall — concepts

1. What four things does a package manager actually do for you beyond "copying files to disk"?
2. What's a repository, in APT terms? What's the difference between a repository and a "source"
   entry?
3. What's a dependency, and what does "dependency hell" mean in practice?
4. Name Ubuntu's four official repository components (Main, Universe, Restricted, Multiverse) and
   what distinguishes them.

### 🟢 Recall — apt / dpkg

5. What's the difference between `apt update` and `apt upgrade`? Why must you run the first before
   the second actually does anything useful?
6. What's the difference between `apt upgrade` and `apt full-upgrade`?
7. What's the difference between `apt remove` and `apt purge`?
8. What does `apt autoremove` clean up that `apt remove` leaves behind?
9. What does `apt-mark hold` do, and name one real production scenario where you'd want it.
10. What's the relationship between `apt` and `dpkg`? Which one actually unpacks and installs the
    `.deb` file, and which one resolves dependencies and talks to repositories?
11. How do you find which installed package owns a given file path? How do you list every file a
    given package put on disk?
12. What does `dpkg --configure -a` fix?

### 🟢 Recall — repos, GPG, RHEL family

13. What's the difference between `/etc/apt/sources.list` and `/etc/apt/sources.list.d/`?
14. What is a PPA? Who maintains a PPA versus an official Ubuntu repo?
15. Why does APT need a GPG key for a third-party repo at all — what attack does signature
    verification prevent?
16. Describe, in order, the three things you do to add a third-party repo the modern (non-deprecated
    `apt-key`) way.
17. On the RHEL/Rocky/Fedora side, what's the `dnf` equivalent of `apt install`? What's the `rpm`
    equivalent of `dpkg -L`?

### 🟢 Recall — snap / flatpak

18. What's fundamentally different about how a Snap package satisfies its dependencies compared to a
    `.deb` package?
19. Name three categories of software you should *not* install via Snap/Flatpak even if available, and
    explain why (hint: think about what needs tight system integration).

### 🛠 Hands-on

20. Search for `htop` with `apt search`, then view its full description and dependency list with `apt
    show` **before** installing it. Then install it.
21. Install `nginx`. Use `dpkg -L nginx` to list every file it placed on disk. Identify, from that
    list, the systemd unit file and the default site config file.
22. Find which package owns `/usr/bin/curl` using `dpkg -S`. Then find which package owns whatever
    binary `which awk` resolves to.
23. Hold `nginx` at its current version with `apt-mark hold`, confirm with `apt-mark showhold`, then
    unhold it. Write one sentence on a real scenario where you'd want a package held in production.
24. (If you have access to a Debian/Ubuntu box) add Docker's official APT repo using the modern
    keyring workflow: download the GPG key into `/etc/apt/keyrings/`, reference it with `signed-by=`
    in a `.list` or `.sources` file, then `apt update` and confirm the new repo's packages are visible
    with `apt-cache policy docker-ce` or `apt show docker-ce`. Remove the repo afterward and `apt
    update` again to confirm it's gone.
25. Deliberately break package state in a low-stakes way (e.g. interrupt an `apt install` with Ctrl-C
    if you're brave, or just read about the symptom if not) and explain what `dpkg --configure -a`
    and `apt --fix-broken install` each repair, and in what order you'd run them.

### 🧩 Scenario

26. `apt install somepackage` fails with a GPG signature error. Walk through your diagnosis: what are
    you checking first, second, third?
27. A production server needs `nginx` to *never* auto-update during routine `apt upgrade` runs, but
    you still want security patches for everything else. What command enforces this, and what's the
    operational trade-off you're accepting?
28. You're auditing a server and need to answer "is this exact file part of an official package, or
    was it dropped here by hand?" What command answers that question definitively?

---

## 7. Editors, Shell Config & tmux

Notes: [8_editor_shell.md](../Linux/Phase1-Foundation/8_editor_shell.md)

### 🟢 Recall — login vs. interactive, config file order

1. What's the difference between a login shell and a non-login shell? Give one real example of each.
2. What's the difference between an interactive shell and a non-interactive shell?
3. For an interactive **login** shell, what's the read order: `/etc/profile`, then which user file(s),
   in what fallback order if the first doesn't exist?
4. For an interactive **non-login** shell, which single file is read, and which files from question 3
   are explicitly *not* read?
5. You put an alias in `~/.bashrc` and then SSH in fresh — the alias isn't there. Why, and what's the
   standard one-line fix placed in `~/.bash_profile`?
6. What's `~/.bash_aliases` for, and why doesn't it work unless `~/.bashrc` explicitly sources it?
7. What does the `source` command actually do (in terms of process/shell context), and how is `source
   script.sh` different from `./script.sh`?

### 🟢 Recall — vim / nano

8. Name vim's four modes and the keystroke that enters each from Normal mode.
9. How do you save and quit in vim? How do you discard changes and quit? How do you search for text
   and jump to the next match?
10. What do `dd`, `yy`, `p`, and `:%s/old/new/g` each do in vim?
11. In `nano`, what does `Ctrl-O` do versus `Ctrl-X`? How do you search for text?

### 🟢 Recall — tmux

12. What's the difference between a tmux **session**, a **window**, and a **pane**?
13. What's the default tmux prefix key combination? How do you detach from a session without killing
    it, and how do you reattach later?
14. How do you split a tmux window horizontally versus vertically (the actual key sequence, not just
    "split it")?
15. Why is tmux valuable specifically for SSH work, beyond just "it has multiple panes"?

### 🛠 Hands-on

16. Open `/etc/services` in vim (read-only is fine). Search for `https`, identify its port number, and
    quit without saving any accidental changes.
17. Add three aliases to your `~/.bashrc`: `ll='ls -lah'`, `..='cd ..'`, and one of your own choosing.
    `source ~/.bashrc` and confirm all three work in your current shell without opening a new
    terminal.
18. Customize your `PS1` to show username, hostname, and current directory using the standard escape
    codes (`\u`, `\h`, `\w`). Make it permanent by putting it in the right config file for your shell
    type, and confirm it survives opening a brand-new terminal.
19. Start a tmux session named `work`. Split it into two panes. Run something long-lived (e.g. `top`)
    in one pane and a plain shell in the other. Detach with the prefix + `d`. Reattach with `tmux
    attach -t work`. Then kill the session cleanly with `tmux kill-session -t work`.
20. Write a one-line shell function (e.g. `mkcd() { mkdir -p "$1" && cd "$1"; }`) into your `~/.bashrc`,
    source it, and use it to confirm it both creates and enters a new directory in one step.

### 🧩 Scenario

21. A teammate's custom prompt and aliases work fine when they open a terminal locally on their laptop,
    but completely disappear the moment they SSH into a server. Diagnose, using what you know about
    login vs. non-login shells, the most likely root cause and the fix.
22. You're stuck in vim with unsaved changes you don't want, no idea what mode you're in, and the
    prompt looks frozen. What's the exact keystroke sequence that gets you out cleanly, discarding
    changes?
23. You SSH into a box, start a long build inside `tmux`, and your laptop's WiFi drops mid-build. What
    happens to the build, and why is this fundamentally different from running the same build directly
    over a raw SSH session without tmux?

### 🚀 Challenge

24. Set up a small personal dotfiles structure (even just `~/.bashrc`, `~/.vimrc`, `~/.tmux.conf` is
    enough): put real content in each (a few aliases, one vim setting, one tmux setting like
    `set -g mouse on`), and write a one-paragraph note on how you'd turn this into a versioned,
    symlink-based dotfiles repo (you don't have to push it anywhere — just demonstrate you understand
    the symlink-from-a-clone pattern over copy-pasting files).

---

## 8. Phase 1 Capstone — "Day One as the Linux Admin"

This ties every section above into one realistic admin task. Do it on a disposable VM/container —
treat it exactly like a real onboarding ticket, and keep a runbook as you go (command, why you ran
it, how you verified it worked). That runbook is the actual deliverable; the system state is just
proof it works.

**The ticket:** *"New engineer Priya is joining the platform team. Provision her access and a shared
team workspace, and leave the box in a state any other admin could audit in five minutes."*

1. Create user `priya` with a home directory and a sensible default shell. Set an expiring password
   that forces a change on first login.
2. Create group `platform`. Add `priya` to it as a secondary group, without disturbing her primary
   group.
3. Create `/srv/platform/`, group-owned by `platform`, with the setgid bit set, so every file dropped
   in there inherits the group automatically. Prove this by having `priya` create a file inside it and
   checking its group ownership.
4. Inside `/srv/platform/`, create a `dropbox/` subdirectory where any `platform` member can add files,
   but no one — including other group members — can delete a file they don't own. Justify the exact
   mode you chose.
5. Grant `priya` sudo rights to restart exactly one service (pick any installed or hypothetical
   service name) without a password, and nothing else. Test that she can run the allowed command and
   that an unrelated `sudo` command is rejected or prompts as expected.
6. Install `htop`, `tree`, and one package from a third-party repo of your choosing using the modern
   GPG-keyring workflow (skip this step only if you have no internet access in the lab — note that you
   skipped it and why).
7. Find every SUID binary on the box and write down, for three of them, a one-sentence justification
   of why that specific binary needs the bit.
8. Write a single `find`/`xargs`/`grep` pipeline that audits `/srv/platform/` for any file that is
   world-writable (a common security smell) and lists them.
9. Set `priya`'s default shell prompt (`PS1`) to show user, host, and working directory, and add one
   alias to her `~/.bashrc` that you think a platform engineer would actually want.
10. Produce a final audit summary (plain text or markdown is fine) covering: every user/group you
    created, every permission decision and *why*, every package installed and from where, and one
    thing you'd improve if you had to harden this further (tie it to a Phase 6 concept like sudo
    logging or AppArmor if you want to look ahead — not required to implement, just name it).

**Done when:** another person could read your runbook, rebuild this from a blank VM, and verify every
step without asking you a single clarifying question.

---

## 9. Oral Drill — Explain It Without a Terminal

Answer these out loud (record yourself, or explain to a rubber duck — actually do it, don't just
think "I know this"). No man pages, no notes, no terminal. If you can't get through one cleanly in
under a minute, that's your real signal to go back and re-drill that section.

1. Explain the difference between `>` and `>>`, what `2>&1` means, and why `cmd | tee file` is useful
   — in one breath, as if explaining to a new hire.
2. Walk through exactly what happens, step by step, from typing `ls -l` to seeing output on screen,
   touching on: shell parsing, `fork`, `exec`, and where stdout ends up. (Full kernel-level detail is
   Phase 3 — for now, get the shell-level mechanics right.)
3. A coworker asks "why can't I just `kill -9` this stuck process and move on?" Give them the honest,
   complete answer.
4. Explain hard links vs. symbolic links to someone who has never heard of an inode, using an
   analogy if it helps, then explain it again precisely, using "inode."
5. Explain `umask` well enough that someone could predict the permissions of a new file from a given
   umask value, without ever running the command.
6. Explain SUID, SGID, and the sticky bit — one real-world reason each one exists, not just the
   definition.
7. Explain the difference between a process, a process group, and a session, and why Ctrl-C stops a
   whole pipeline.
8. Explain why `apt update` doesn't install or upgrade anything by itself, and what would happen if
   you ran `apt install` without ever having run `apt update` on a brand-new box.
9. Explain the login-shell vs. non-login-shell config file split clearly enough that someone could
   debug "my alias isn't showing up over SSH" themselves afterward.
10. Explain zombie vs. orphan processes, and why one is "a parent's fault" and the other isn't a
    long-term problem.
11. Explain why `grep -r` exists — what does plain `grep "pattern" somedir` do instead, and why does
    that surprise people?
12. Explain the difference between `apt remove` and `apt purge`, and give a real reason you'd
    deliberately choose `remove` over `purge`.
13. You're asked in an interview: "the disk shows 100% full in `df`, but `du -sh /` only adds up to
    20% used. What's going on?" Give your best answer using only Phase 1 knowledge (think: open file
    handles to deleted files, and what else `df` counts that `du` doesn't always see).
14. Explain why `/tmp` being wiped on reboot is a feature, not a bug, and name one type of data you
    should never rely on `/tmp` to keep.
15. Explain what a controlling terminal is, and why closing your terminal window can kill background
    jobs you forgot to `nohup` or `disown`.


