Linux System Administrator/DevOps Interview Questions
====================================================

A collection of linux sysadmin/devops interview questions. Feel free to contribute via pull requests, issues or email messages.


## <a name='toc'>Table of Contents</a>
- [Linux System Administrator/DevOps Interview Questions](#linux-system-administratordevops-interview-questions)
  - [Table of Contents](#table-of-contents)
      - [\[⬆\] General Questions:](#-general-questions)
      - [\[⬆\] Simple Linux Questions:](#-simple-linux-questions)
      - [\[⬆\] Medium Linux Questions:](#-medium-linux-questions)
      - [\[⬆\] Hard Linux Questions:](#-hard-linux-questions)
      - [\[⬆\] Expert Linux Questions:](#-expert-linux-questions)
      - [\[⬆\] Networking Questions:](#-networking-questions)
      - [\[⬆\] MySQL questions:](#-mysql-questions)
      - [\[⬆\] DevOps Questions:](#-devops-questions)
      - [\[⬆\] Fun Questions:](#-fun-questions)
      - [\[⬆\] Demo Time:](#-demo-time)
  - [📑 Section 1: 📗 Basic / Low Difficulty Questions](#-section-1--basic--low-difficulty-questions)
  - [📑 Section 2: 📘 Moderate / Mid-Level Questions](#-section-2--moderate--mid-level-questions)
  - [📑 Section 3: 📕 Advanced / High-Level Questions](#-section-3--advanced--high-level-questions)
  - [📑 Section 4: Scenario-Based / Practical Mock Questions](#-section-4-scenario-based--practical-mock-questions)
  - [📑 Section 5: Assignment-Based Questions](#-section-5-assignment-based-questions)
  - [📑 Section 6: Troubleshooting Case Studies](#-section-6-troubleshooting-case-studies)
  - [📑 Section 7: Kernel, Storage, Networking, and Performance (High-Difficulty)](#-section-7-kernel-storage-networking-and-performance-high-difficulty)
  - [📑 Section 8: Storage and Filesystem Management](#-section-8-storage-and-filesystem-management)
  - [📑 Section 9: Networking](#-section-9-networking)
  - [📑 Section 10: Performance Tuning and Monitoring](#-section-10-performance-tuning-and-monitoring)
  - [📑 Section 11: Additional Advanced Linux Operations \& Commands](#-section-11-additional-advanced-linux-operations--commands)
  - [📑 Section 12: DevOps and Cloud-Friendly Linux Operations](#-section-12-devops-and-cloud-friendly-linux-operations)
  - [📑 Section 13: Linux Security and Hardening](#-section-13-linux-security-and-hardening)
      - [\[⬆\] Other Great References:](#-other-great-references)


#### [[⬆]](#toc) <a name='general'>General Questions:</a>

* What did you learn yesterday/this week?
* Talk about your preferred development/administration environment. (OS, Editor, Browsers, Tools etc.)
* Tell me about the last major Linux project you finished.
* Tell me about the biggest mistake you've made in [some recent time period] and how you would do it differently today. What did you learn from this experience?
* Why we must choose you?
* What function does DNS play on a network?
* What is HTTP?
* What is an HTTP proxy and how does it work?
* Describe briefly how HTTPS works.
* What is SMTP? Give the basic scenario of how a mail message is delivered via SMTP.
* What is RAID? What is RAID0, RAID1, RAID5, RAID10?
* What is a level 0 backup? What is an incremental backup?
* Describe the general file system hierarchy of a Linux system.
* Which difference have between public and private SSH key?


#### [[⬆]](#toc) <a name='simple'>Simple Linux Questions:</a>

* What is the name and the UID of the administrator user?
* How to list all files, including hidden ones, in a directory?
* What is the Unix/Linux command to remove a directory and its contents?
* Which command will show you free/used memory? Does free memory exist on Linux?
* How to search for the string "my konfu is the best" in files of a directory recursively?
* How to connect to a remote server or what is SSH?
* How to get all environment variables and how can you use them?
* I get "command not found" when I run ```ifconfig -a```. What can be wrong?
* What happens if I type TAB-TAB?
* What command will show the available disk space on the Unix/Linux system?
* What commands do you know that can be used to check DNS records?
* What Unix/Linux commands will alter a files ownership, files permissions?
* What does ```chmod +x FILENAME``` do?
* What does the permission 0750 on a file mean?
* What does the permission 0750 on a directory mean?
* How to add a new system user without login permissions?
* How to add/remove a group from a user?
* What is a bash alias?
* How do you set the mail address of the root/a user?
* What does CTRL-c do?
* What does CTRL-d do?
* What does CTRL-z do?
* What is in /etc/services?
* How to redirect STDOUT and STDERR in bash? (> /dev/null 2>&1)
* What is the difference between UNIX and Linux.
* What is the difference between Telnet and SSH?
* Explain the three load averages and what do they indicate. What command can be used to view the load averages?
* Can you name a lower-case letter that is not a valid option for GNU ```ls```?
* What is a Linux kernel module?
* Walk me through the steps in booting into single user mode to troubleshoot a problem.
* Walk me through the steps you'd take to troubleshoot a 404 error on a web application you administer.
* What is ICMP protocol? Why do you need to use?

#### [[⬆]](#toc) <a name='medium'>Medium Linux Questions:</a>

* What do the following commands do and how would you use them?
 * ```tee```
 * ```awk```
 * ```tr```
 * ```cut```
 * ```tac```
 * ```curl```
 * ```wget```
 * ```watch```
 * ```head```
 * ```tail```
 * ```less```
 * ```cat```
 * ```touch```
 * ```sar```
 * ```netstat```
 * ```tcpdump```
 * ```lsof```
* What does an ```&``` after a command do?
* What does ```& disown``` after a command do?
* What is a packet filter and how does it work?
* What is Virtual Memory?
* What is swap and what is it used for?
* What is an A record, an NS record, a PTR record, a CNAME record, an MX record?
* Are there any other RRs and what are they used for?
* What is a Split-Horizon DNS?
* What is the sticky bit?
* What does the immutable bit do to a file?
* What is the difference between hardlinks and symlinks? What happens when you remove the source to a symlink/hardlink?
* What is an inode and what fields are stored in an inode?
* How to force/trigger a file system check on next reboot?
* What is SNMP and what is it used for?
* What is a runlevel and how to get the current runlevel?
* What is SSH port forwarding?
* What is the difference between local and remote port forwarding?
* What are the steps to add a user to a system without using useradd/adduser?
* What is MAJOR and MINOR numbers of special files?
* Describe the mknod command and when you'd use it.
* Describe a scenario when you get a "filesystem is full" error, but 'df' shows there is free space.
* Describe a scenario when deleting a file, but 'df' not showing the space being freed.
* Describe how 'ps' works.
* What happens to a child process that dies and has no parent process to wait for it and what’s bad about this?
* Explain briefly each one of the process states.
* How to know which process listens on a specific port?
* What is a zombie process and what could be the cause of it?
* You run a bash script and you want to see its output on your terminal and save it to a file at the same time. How could you do it?
* Explain what echo "1" > /proc/sys/net/ipv4/ip_forward does.
* Describe briefly the steps you need to take in order to create and install a valid certificate for the site https://foo.example.com.
* Can you have several HTTPS virtual hosts sharing the same IP?
* What is a wildcard certificate?
* Which Linux file types do you know?
* What is the difference between a process and a thread? And parent and child processes after a fork system call?
* What is the difference between exec and fork?
* What is "nohup" used for?
* What is the difference between these two commands?
 * ```myvar=hello```
 * ```export myvar=hello```
* How many NTP servers would you configure in your local ntp.conf?
* What does the column 'reach' mean in ```ntpq -p``` output?
* You need to upgrade kernel at 100-1000 servers, how you would do this?
* How can you get Host, Channel, ID, LUN of SCSI disk?
* How can you limit process memory usage?
* What is bash quick substitution/caret replace(^x^y)?
* Do you know of any alternative shells? If so, have you used any?
* What is a tarpipe (or, how would you go about copying everything, including hardlinks and special files, from one server to another)?
* How can you tell if the httpd package was already installed?
* How can you list the contents of a package?
* How can you determine which package is better: openssh-server-5.3p1-118.1.el6_8.x86_64 or openssh-server-6.6p1-1.el6.x86_64 ?
* Can you explain to me the difference between block based, and object based storage?

#### [[⬆]](#toc) <a name='hard'>Hard Linux Questions:</a>

* What is a tunnel and how you can bypass a http proxy?
* What is the difference between IDS and IPS?
* What shortcuts do you use on a regular basis?
* What is the Linux Standard Base?
* What is an atomic operation?
* Your freshly configured http server is not running after a restart, what can you do?
* What kind of keys are in ~/.ssh/authorized_keys and what it is this file used for?
* I've added my public ssh key into authorized_keys but I'm still getting a password prompt, what can be wrong?
* Did you ever create RPM's, DEB's or solaris pkg's?
* What does ```:(){ :|:& };:``` do on your system?
* How do you catch a Linux signal on a script?
* Can you catch a SIGKILL?
* What's happening when the Linux kernel is starting the OOM killer and how does it choose which process to kill first?
* Describe the linux boot process with as much detail as possible, starting from when the system is powered on and ending when you get a prompt.
* What's a chroot jail?
* When trying to umount a directory it says it's busy, how to find out which PID holds the directory?
* What's LD_PRELOAD and when it's used?
* You ran a binary and nothing happened. How would you debug this?
* What are cgroups? Can you specify a scenario where you could use them?
* How can you remove/delete a file with file-name consisting of only non-printable/non-type-able characters?
* How can you increase or decrease the priority of a process in Linux?


#### [[⬆]](#toc) <a name='expert'>Expert Linux Questions:</a>

* A running process gets ```EAGAIN: Resource temporarily unavailable``` on reading a socket. How can you close this bad socket/file descriptor without killing the process?
* What do you control with swapiness?
* How do you change TCP stack buffers? How do you calculate it?
* What is Huge Tables? Why isn't it enabled by default? Why and when use it?
* What is LUKS? How to use it?


#### [[⬆]](#toc) <a name='network'>Networking Questions:</a>

* What is localhost and why would ```ping localhost``` fail?
* What is the similarity between "ping" & "traceroute" ? How is traceroute able to find the hops.
* What is the command used to show all open ports and/or socket connections on a machine?
* Is 300.168.0.123 a valid IPv4 address?
* Which IP ranges/subnets are "private" or "non-routable" (RFC 1918)?
* What is a VLAN?
* What is ARP and what is it used for?
* What is the difference between TCP and UDP?
* What is the purpose of a default gateway?
* What is command used to show the routing table on a Linux box?
* A TCP connection on a network can be uniquely defined by 4 things. What are those things?
* When a client running a web browser connects to a web server, what is the source port and what is the destination port of the connection?
* How do you add an IPv6 address to a specific interface?
* You have added an IPv4 and IPv6 address to interface eth0. A ping to the v4 address is working but a ping to the v6 address gives you the response ```sendmsg: operation not permitted```. What could be wrong?
* What is SNAT and when should it be used?
* Explain how could you ssh login into a Linux system that DROPs all new incoming packets using a SSH tunnel.
* How do you stop a DDoS attack?
* How can you see content of an ip packet?
* What is IPoAC (RFC 1149)?
* What will happen when you bind port 0?



#### [[⬆]](#toc) <a name='mysql'>MySQL questions:</a>

* How do you create a user?
* How do you provide privileges to a user?
* What is the difference between a "left" and a "right" join?
* Explain briefly the differences between InnoDB and MyISAM.
* Describe briefly the steps you need to follow in order to create a simple master/slave cluster.
* Why should you run "mysql_secure_installation" after installing MySQL?
* How do you check which jobs are running?
* How would you take a backup of a MySQL database?

#### [[⬆]](#toc) <a name='devop'>DevOps Questions:</a>

* Can you describe your workflow when you create a script?
* What is GIT?
* What is a dynamically/statically linked file?
* What does "./configure && make && make install" do?
* What is puppet/chef/ansible used for?
* What is Nagios/Zenoss/NewRelic used for?
* What is Jenkins/TeamCity/GoCI used for?
* What is the difference between Containers and VMs?
* How do you create a new postgres user?
* What is a virtual IP address? What is a cluster?
* How do you print all strings of printable characters present in a file?
* How do you find shared library dependencies?
* What is Automake and Autoconf?
* ./configure shows an error that libfoobar is missing on your system, how could you fix this, what could be wrong?
* What are the advantages/disadvantages of script vs compiled program?
* What's the relationship between continuous delivery and DevOps?
* What are the important aspects of a system of continuous integration and deployment?
* How would you enable network file sharing within AWS that would allow EC2 instances in multiple availability zones to share data?

#### [[⬆]](#toc) <a name='fun'>Fun Questions:</a>

* A careless sysadmin executes the following command: ```chmod 444 /bin/chmod ``` - what do you do to fix this?
* I've lost my root password, what can I do?
* I've rebooted a remote server but after 10 minutes I'm still not able to ssh into it, what can be wrong?
* If you were stuck on a desert island with only 5 command-line utilities, which would you choose?
* You come across a random computer and it appears to be a command console for the universe. What is the first thing you type?
* Tell me about a creative way that you've used SSH?
* You have deleted by error a running script, what could you do to restore it?
* What will happen on 19 January 2038?
* How to reboot server when reboot command is not responding?


#### [[⬆]](#toc) <a name='demo'>Demo Time:</a>

* Unpack test.tar.gz without man pages or google.
* Remove all "*.pyc" files from testdir recursively?
* Search for "my konfu is the best" in all *.py files.
* Replace the occurrence of "my konfu is the best" with "I'm a linux jedi master" in all *.txt files.
* Test if port 443 on a machine with IP address X.X.X.X is reachable.
* Get http://myinternal.webserver.local/test.html via telnet.
* How to send an email without a mail client, just on the command line?
* Write a ```get_prim``` method in python/perl/bash/pseudo.
* Find all files which have been accessed within the last 30 days.
* Explain the following command ```(date ; ps -ef |tail -n +2 | awk '{print $1}' | sort | uniq | wc -l ) >> Activity.log```
* Write a script to list all the differences between two directories.
* In a log file with contents as ```<TIME> : [MESSAGE] : [ERROR_NO] - Human readable text``` display summary/count of specific error numbers that occurred every hour or a specific hour.

---
## 📑 Section 1: 📗 Basic / Low Difficulty Questions

| #  | Question                                      | Answer                                                                                                   |
| :- | :-------------------------------------------- | :------------------------------------------------------------------------------------------------------- |
| 1  | What is Linux?                                | An open-source Unix-like operating system kernel widely used in servers, desktops, and embedded systems. |
| 2  | How to check the present working directory?   | `pwd`                                                                                                    |
| 3  | Command to list files including hidden files? | `ls -a`                                                                                                  |
| 4  | How to create a directory?                    | `mkdir dirname`                                                                                          |
| 5  | How to view file content?                     | `cat filename`                                                                                           |
| 6  | How to display the first 10 lines of a file?  | `head -n 10 filename`                                                                                    |
| 7  | How to display the last 10 lines?             | `tail -n 10 filename`                                                                                    |
| 8  | How to copy files?                            | `cp source destination`                                                                                  |
| 9  | How to move/rename files?                     | `mv source destination`                                                                                  |
| 10 | How to delete a file?                         | `rm filename`      

---
## 📑 Section 2: 📘 Moderate / Mid-Level Questions

| #  | Question                                    | Answer                                                                                    |
| :- | :------------------------------------------ | :---------------------------------------------------------------------------------------- |
| 11 | What does `chmod 755 file` do?              | Sets permissions: owner (7) = rwx, group (5) = r-x, others (5) = r-x                      |
| 12 | How to find the size of a directory?        | `du -sh directory/`                                                                       |
| 13 | Difference between hard link and soft link? | Hard link: same inode, can’t span filesystems; Soft link: shortcut with a different inode |
| 14 | How to check CPU usage?                     | `top` or `htop`                                                                           |
| 15 | Command to list all running services?       | `systemctl list-units --type=service`                                                     |
| 16 | How to restart the networking service?      | `systemctl restart network` (RHEL/CentOS)                                                 |
| 17 | How to check system uptime?                 | `uptime`                                                                                  |
| 18 | How to display disk partitions?             | `lsblk`                                                                                   |
| 19 | How to mount a filesystem?                  | `mount /dev/sdX /mnt/point`                                                               |
| 20 | How to view system logs?                    | `journalctl` or `/var/log/messages`                                                       |

---

## 📑 Section 3: 📕 Advanced / High-Level Questions

| #  | Question                              | Answer                                                                  |
| :- | :------------------------------------ | :---------------------------------------------------------------------- |
| 21 | What is SELinux?                      | Security-Enhanced Linux; enforces security policies                     |
| 22 | How to disable SELinux temporarily?   | `setenforce 0`                                                          |
| 23 | What is the function of `/etc/fstab`? | Contains static info about filesystems for automatic mounting           |
| 24 | How to check available memory?        | `free -m`                                                               |
| 25 | How to limit user disk usage?         | Using disk quotas                                                       |
| 26 | How to manage user groups?            | `groupadd`, `usermod -aG group user`                                    |
| 27 | Difference between `su` and `sudo`?   | `su`: switch user shell; `sudo`: execute single command as another user |
| 28 | What is LVM?                          | Logical Volume Manager for flexible disk management                     |
| 29 | How to create a new LVM partition?    | `pvcreate`, `vgcreate`, `lvcreate`                                      |
| 30 | How to resize an LVM volume?          | `lvresize` + `resize2fs`                                                |

---

## 📑 Section 4: Scenario-Based / Practical Mock Questions

| #   | Question                                                   | Description                                                             |
| :-- | :--------------------------------------------------------- | :---------------------------------------------------------------------- |
| 31 | Disk space on `/` is full — how will you fix this?         | Check large files with `du -sh *`, delete unnecessary logs, clean cache |
| 32 | A user can’t SSH into the server — troubleshoot.           | Check network, SSH service, firewall, and `/etc/ssh/sshd_config`        |
| 33 | High CPU load — steps to troubleshoot.                     | Use `top`, `ps aux`, check for hung processes                           |
| 34 | A cron job isn't running — how to debug?                   | Check `crontab -l`, `/var/log/cron`, permissions                        |
| 35 | Configure a backup script to run every Sunday at midnight. | Create shell script, set cron entry `0 0 * * 0 /path/backup.sh`         |
| 36 | Migrate website from server A to B.                        | Rsync files, migrate DB, configure web server                           |
| 37 | Install and configure Docker on CentOS.                    | `yum install docker`, `systemctl start docker`, `docker run`            |
| 38 | Configure a firewall to allow SSH and HTTP only.           | `firewall-cmd --permanent --add-service=ssh`, `--add-service=http`      |
| 39 | Write a script to check and email disk usage if >90%.      | Use `df -h`, `awk`, and `mail` command                                  |
| 40 | Deploy Nginx as a reverse proxy for an application.        | Install Nginx, edit `/etc/nginx/nginx.conf`                             |

---

## 📑 Section 5: Assignment-Based Questions

| #   | Assignment                                         | Instructions                                         |          |           |
| :-- | :------------------------------------------------- | :--------------------------------------------------- | -------- | --------- |
| 41 | Create 5 user accounts with home directories       | Use `useradd -m username`                            |          |           |
| 42 | Set password expiry policy for users               | Use `chage` command                                  |          |           |
| 43 | Create a bash script to monitor load average       | Use `uptime` and log results                         |          |           |
| 44 | Schedule cleanup of `/tmp` at midnight daily       | Cron job: `0 0 * * * rm -rf /tmp/*`                  |          |           |
| 45 | Create a soft link and hard link for a file        | `ln file hardlink` and `ln -s file softlink`         |          |           |
| 46 | Install Apache and host a static website           | `yum install httpd` or `apt install apache2`         |          |           |
| 47 | Find top 5 largest files in `/var`                 | \`find /var -type f -exec du -h {} +                 | sort -rh | head -5   |
| 48 | Setup local YUM repository                         | Mount ISO, create `repo` file in `/etc/yum.repos.d/` |          |           |
| 49 | Generate system report with CPU, Memory, Disk info | Combine `lscpu`, `free -m`, `df -h` in a script      |          |           |
| 50 | Create Docker image with Apache installed          | Write Dockerfile, build image, run container         |          |           |

---

## 📑 Section 6: Troubleshooting Case Studies

| #   | Issue                          | Expected Troubleshooting                    |
| :-- | :----------------------------- | :------------------------------------------ |
| 51 | Server boot loop               | Check `/var/log/messages`, kernel logs      |
| 52 | Service failed to start        | `systemctl status service`, logs            |
| 53 | No internet on server          | Check `/etc/resolv.conf`, `ping 8.8.8.8`    |
| 54 | Disk performance issue         | Use `iotop`, `iostat`                       |
| 55 | SSH Key authentication fails   | Check `.ssh/authorized_keys`, permissions   |
| 56 | User account locked            | `passwd -S username`, `usermod -U username` |
| 57 | High swap usage                | Check processes, increase RAM or optimize   |
| 58 | Cron not executing scripts     | Permissions, path issues, cron logs         |
| 59 | Network service drops randomly | Check NIC driver, logs, `dmesg`             |
| 60 | SELinux blocking application   | `getenforce`, `audit2allow` tools           |


---

## 📑 Section 7: Kernel, Storage, Networking, and Performance (High-Difficulty)

| #   | Question                                                     | Answer                                                                                            |
| :-- | :----------------------------------------------------------- | :------------------------------------------------------------------------------------------------ |
| 61 | How do you list kernel modules currently loaded?             | `lsmod`                                                                                           |
| 62 | How to load and unload a kernel module?                      | `modprobe module_name`, `modprobe -r module_name`                                                 |
| 63 | Where are kernel parameters stored for runtime modification? | `/proc/sys/`                                                                                      |
| 64 | How to permanently update kernel parameters?                 | Edit `/etc/sysctl.conf` and run `sysctl -p`                                                       |
| 65 | Command to view kernel version?                              | `uname -r`                                                                                        |
| 66 | What is a kernel panic?                                      | A critical system error from which the OS cannot recover                                          |
| 67 | How to debug a kernel panic?                                 | Check `/var/log/messages`, `journalctl -xb`, console output                                       |
| 68 | How to compile a custom Linux kernel?                        | Download source, configure with `make menuconfig`, `make`, `make modules_install`, `make install` |
| 69 | What is `initrd` or `initramfs`?                             | Temporary root file system used during boot before actual root filesystem is mounted              |
| 70 | What is `strace` and how is it useful? | Diagnostic tool to trace system calls made by a process. Example: `strace -p <pid>` or `strace ./binary`                |                                      

---

## 📑 Section 8: Storage and Filesystem Management

| #   | Question                                      | Answer                                                    |
| :-- | :-------------------------------------------- | :-------------------------------------------------------- |
| 71 | How to check filesystem type of a partition?  | `df -T` or `blkid`                                        |
| 72 | How to create an ext4 filesystem?             | `mkfs.ext4 /dev/sdX`                                      |
| 73 | Command to check filesystem errors?           | `fsck /dev/sdX`                                           |
| 74 | What is a swap partition?                     | A space on disk used when RAM is full                     |
| 75 | How to add additional swap space?             | Create a swap file, `mkswap`, `swapon`                    |
| 76 | How to extend a mounted filesystem using LVM? | `lvextend -L +5G /dev/vg/lv` → `resize2fs /dev/vg/lv`     |
| 77 | How to view mounted filesystems?              | `mount` or `findmnt`                                      |
| 78 | Difference between RAID 0, 1, 5, and 10?      | 0: striping, 1: mirroring, 5: parity, 10: striped mirrors |
| 79 | How to check RAID status?                     | `cat /proc/mdstat`                                        |
| 80 | How to create a software RAID in Linux?       | `mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sd[b-c]` |

---

## 📑 Section 9: Networking

| #   | Question                                   | Answer                                                              |
| :-- | :----------------------------------------- | :------------------------------------------------------------------ |
| 81 | Command to display IP addresses?           | `ip addr`                                                           |
| 82 | How to configure static IP on Linux?       | Edit `/etc/sysconfig/network-scripts/ifcfg-ethX` or `/etc/netplan/` |
| 83 | How to flush the DNS cache? | `systemd-resolve --flush-caches` (systemd-based), or `sudo service nscd restart` for non-systemd |
| 84 | How to test DNS resolution?                | `dig`, `nslookup`                                                   |
| 85 | How to check open TCP/UDP ports?           | `ss -tulnp`                                                         |
| 86 | How to capture network packets?            | `tcpdump`                                                           |
| 87 | What is `/etc/hosts` file used for?        | Static hostname to IP mapping                                       |
| 88 | How to add a static route?                 | `ip route add 192.168.1.0/24 via 10.0.0.1`                          |
| 89 | How to disable/enable a network interface? | `ip link set eth0 down` / `up`                                      |
| 90 | How to test network latency?               | `ping`, `mtr`                                                       |

---

## 📑 Section 10: Performance Tuning and Monitoring

| #   | Question                                          | Answer                                             |
| :-- | :------------------------------------------------ | :------------------------------------------------- |
| 91 | How to check system load average?                 | `uptime` or `top`                                  |
| 92 | What is the significance of load average numbers? | Average runnable processes in 1, 5, 15 minutes     |
| 93 | How to check disk I/O statistics?                 | `iostat`                                           |
| 94 | How to identify a memory leak?                    | Monitor increasing memory usage with `top`, `free` |
| 95 | How to limit CPU usage for a process?             | `cpulimit` or `nice/renice`                        |
| 96 | How to check swap usage?                          | `swapon -s`, `free -m`                             |
| 97 | How to tune system limits for open files?         | Edit `/etc/security/limits.conf`                   |
| 98 | What is `vm.swappiness`?                          | Kernel parameter controlling swap tendency         |
| 99 | How to reduce swappiness?                         | `sysctl -w vm.swappiness=10`                       |
| 100 | How to monitor network bandwidth usage?           | `iftop` or `nload`                                 |

---

## 📑 Section 11: Additional Advanced Linux Operations & Commands

| #   | Question                                            | Answer                                                               |
| :-- | :-------------------------------------------------- | :------------------------------------------------------------------- |
| 101 | How to search for a string in a file?               | `grep "text" filename`                                               |
| 102 | How to search recursively inside directories?       | `grep -r "text" /path`                                               |
| 103 | Difference between `find` and `locate`?             | `find` searches real-time; `locate` uses a prebuilt index            |
| 104 | How to find files modified in last 24 hours?        | `find /path -mtime -1`                                               |
| 105 | How to find files over 500MB size?                  | `find / -type f -size +500M`                                         |
| 106 | How to compress a directory as a `.tar.gz`?         | `tar -czvf archive.tar.gz /dir`                                      |
| 107 | How to extract `.tar.gz` files?                     | `tar -xzvf archive.tar.gz`                                           |
| 108 | What is the difference between `screen` and `tmux`? | Both are terminal multiplexers; `tmux` is more modern and scriptable |
| 109 | How to kill a process by name?                      | `pkill processname`                                                  |
| 110 | How to list all environment variables?              | `printenv`                                                           |

---

## 📑 Section 12: DevOps and Cloud-Friendly Linux Operations

| #   | Question                                        | Answer                                                   |
| :-- | :---------------------------------------------- | :------------------------------------------------------- |
| 111 | How to install Docker on Ubuntu?                | `apt install docker.io`                                  |
| 112 | How to enable and start Docker service?         | `systemctl enable --now docker`                          |
| 113 | How to check running Docker containers?         | `docker ps`                                              |
| 114 | How to create a new user with sudo permissions? | `useradd -m username && usermod -aG sudo username`       |
| 115 | How to create an SSH key pair?                  | `ssh-keygen -t rsa`                                      |
| 116 | How to copy SSH public key to another server?   | `ssh-copy-id user@host`                                  |
| 117 | How to monitor real-time network connections?   | `netstat -tunap` or `ss -tunap`                          |
| 118 | How to configure Nginx reverse proxy?           | Install Nginx → Edit `nginx.conf` to set `proxy_pass`    |
| 119 | How to install Kubernetes minikube on Linux? | Download binary: `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64` → `chmod +x` → move to `/usr/local/bin/` |
| 120 | How to test TCP/UDP connectivity?               | `nc -zv host port`                                       |

---

## 📑 Section 13: Linux Security and Hardening

| #   | Question                                                 | Answer                                                                |
| :-- | :------------------------------------------------------- | :-------------------------------------------------------------------- |
| 121 | How to disable root SSH login?                           | Edit `/etc/ssh/sshd_config` → `PermitRootLogin no`                    |
| 122 | How to set a firewall rule to allow SSH only?            | `firewall-cmd --permanent --add-service=ssh` → `--reload`             |
| 123 | How to set password complexity policy?                   | Modify `/etc/login.defs` and PAM modules                              |
| 124 | How to list currently open ports?                        | `ss -tulnp`                                                           |
| 125 | How to audit file access logs?                           | `auditd` → configure rules in `/etc/audit/audit.rules`                |
| 126 | How to check failed login attempts?                      | `lastb` or check `/var/log/secure` or `/var/log/auth.log`             |
| 127 | What is AppArmor and how does it differ from SELinux?    | Mandatory Access Control tool like SELinux, but path-based            |
| 128 | How to disable USB devices on a Linux server?            | Blacklist USB kernel modules in `/etc/modprobe.d/` config             |
| 129 | How to check if your system is under brute-force attack? | Check `/var/log/secure`, use `fail2ban` logs                          |
| 130 | How to enable password history restriction?              | Configure `pam_unix.so` with `remember=N` in `/etc/pam.d/system-auth` |

#### [[⬆]](#toc) <a name='references'>Other Great References:</a>

Some questions are 'borrowed' from:

* https://github.com/chassing/linux-sysadmin-interview-questions
* https://github.com/darcyclarke/Front-end-Developer-Interview-Questions
* https://github.com/kylejohnson/linux-sysadmin-interview-questions/blob/master/test.md
* http://slideshare.net/kavyasri790693/linux-admin-interview-questions
* https://github.com/DopplerHQ/awesome-interview-questions
