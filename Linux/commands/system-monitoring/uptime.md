# `uptime` Command in Linux

The uptime command in Linux is used for finding how long the Linux system has been up and running.

This will show you a single line of output that shows the current time, the uptime (in days and hours), the number of users currently logged on to the system, and the load average.

```bash
sample@hello:~$ uptime
 16:13:00 up 2 days,  8:18,  1 user,  load average: 1.19, 1.54, 1.51
```

- **16:13:00**: This is the current time on the system.
- **up 2 days,  8:18**: This means the Linux system has been running for the *last 2 days, 8 hours and 18 minutes*.
- **1 user**: This is the number of users currently logged into the Linux system. except **root**
- **load average**: *1.19, 1.54, 1.51*: This gives the average CPU load for the past `1, 5 and 15` minutes. 

*`1.54` means that 154% of the CPU consumption (if it's a 4-core CPU, it means 1.54 out of 4 cores were in use).*

> The `uptime` command gets the boot related information from the `/proc/uptime` files like most other commands. It uses the `/var/run/utmp` file to get the information on the logged-in users.

## Keep Record of your uptimes

The uptime command only shows how long your system has been running.

There is a handy utility called `uprecords` that provides the record of your uptimes. It shows the best (longest) uptimes of your Linux system in a tabular format with additional information about the boot time, duration and Linux kernel version.

```bash
sudo apt install uptimed
# It's basically a daemon that tracks a system's highest uptimes via boot IDs, using the system boot time to keep sessions apart from each other.

# Once uptimed is installed, you can use the uprecords command to show the uptime records

uprecords
# Keep in mind that you won't get historical uptime records straightaway. 
# It starts recording since the time uptimed daemon is installed.
```
## Alternative Ways to Check uptime

- `w` -> Displays uptime along with logged-in user details.
- `top/htop` -> Shows uptime in the header with real-time process stats.
- `cat /proc/uptime` -> Displays raw uptime in seconds and idle time. 
- `who -b` -> Show last boot time.

