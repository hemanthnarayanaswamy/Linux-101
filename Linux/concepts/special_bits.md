# Special Permissino Bits

Special permission bits are **additional settings** that can be applied to files and directories in Unix-like operating systems.
<p>These settings enhance the basic file permissions system, allowing for more granular control over how files and directories are accessed and executed.</p>

The three primary types of special permissions are:
- **SUID** (Set User ID)
- **SGID** (Set Group ID)
- **Sticky Bit**

---
## User IDs in Linux

In Linux, each process has three types of **user IDs** associated with it:

- **Real User ID (RUID)**: The user ID of the user who started the process.
- **Effective User ID (EUID)**: The user ID that the process uses to determine its access rights.
- **Saved Set-User ID (SUID)**: A copy of the EUID that can be used to restore the EUID later.
- 
https://linuxhandbook.com/suid-sgid-sticky-bit/