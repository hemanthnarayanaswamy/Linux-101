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

---
Apart from these regular permissions, there are a few special file permissions. 

![special](https://linuxhandbook.com/content/images/2020/06/linux-special-permission-suid-guid-sticky-bit.png)

* use the `ll / | awk '{print $1, $9} | grep "x|s"` to see the directory and files that has this special permissions. 

For Example `-rwsr-xr-x 1 root root     64152 May 30  2024  passwd*` the **passwd** command responsible to change the password of a user, has the letter `s` on the same place we expect to see `x` or `-`, for **user permissions**. 
- It’s important to notice that this file belongs to the **root user and root group**.
- But still as a user you can run the `passwd` command without the `sudo` access and its only possible for this type and not for other commands where the `x` permission is `x or -`.
- ***With this permission, you don’t need to give `sudo` access to a specific user when you want him to run some root script.***

---
## What is SUID ?
***When the SUID bit is set on an executable file, this means that the file will be executed with the same permissions as the owner of the executable file.***

If you look at the binary executable file of the `passwd` command, it has the SUID bit set.
<code>-rwsr-xr-x 1 root root 59640 Mar 22  2019 /usr/bin/passwd</code>

**This means that any user running the passwd command will be running it with the same permission as root.**
* The `passwd` command needs to edit files like `/etc/passwd`, `/etc/shadow` to change the password. 
* These files are owned by root and can only be modified by root. But thanks to the **setuid flag (SUID bit)**, a regular user will also be able to modify these files (that are owned by root) and change his/her password.

### How the SUID works Under-the-Hood
When a program has the `setuid` bit set, the `EUID` of the process that executes the program is set to the **user ID** of the file's owner. 
* This means that the program can perform operations with the privileges of the file's owner, even if the user who runs the program has different privileges.
* For example, if a file owned by the root user has the setuid bit set, any user who runs that file will have the EUID set to root, allowing the program to perform root-level operations.

### Why can a normal user not change the password of other users?
If you check the **code** for the `passwd` command, you’ll find that it checks the `UID` of the user whose password is being modified with the `UID` of the user that ran the command. 
* **If it doesn’t match and if the command wasn’t run by root, it throws an error.**
* with `setuid` you can execute the file but the program of the file has some conditions that needs to be completed for the above operation. 

### How to Set & Remove `SUID` bit ? 
* To set the `setuid` bit on a file, you can use the `chmod` command:
* You can also use the numeric way. **You just need to add a fourth digit to the normal permissions**. The octal number used to set **SUID** is always `4`.
```bash
chmod u+s /path/to/file
chmod 4776 /path/to/file
```
* To remove the **SUID**, use the numeric way with `0` instead of `4` with the permissions you want to set:
```bash
chmod u-s /path/to/file
chmod 0776 /path/to/file
```
### Difference Between `s` and `S` as SUID bit.
The SUID allows a file to be ***executed*** with the same permissions as the owner of the file.

But what if the file doesn’t have execute bit set in the first place? 
* If you set the SUID bit, it will show a capital `S`, not small `s`: `-rwSrw-rw- 1 linuxhandbook linuxhandbook 0 Apr 12 17:52 test.txt`

*The `S` as SUID flag means there is an error that you should look into. You want the file to be executed with the same permission as the owner but there is no executable permission on the file. Which means that not even the owner is allowed to execute the file and if file cannot be executed, you won’t get the permission as the owner. This fails the entire point of setting the SUID bit.*

### How to Find all files with SUID set ?
If you want to search files with this permission, Use the [find](../commands/find.md) command with `-perm` option

```bash
# Find all SUID files
find / -perm /4000
# Find all SGID files
find / -perm /2000
# Find all Sticky Bit directories
find / -perm /1000

# / before the permission number finds files who can be executed any one
```
---
## What is SGID ?
**SGID is similar to SUID. With the SGID bit set, any user executing the file will have same permissions as the group owner of the file.**\

It’s benefit is in *handling the directory*. When SGID permission is applied to a directory, *all sub directories and files created inside this directory will get the same group ownership as main directory* (not the group ownership of the user that created the files and directories).

![sgid](https://linuxhandbook.com/content/images/2020/06/sgid-linux.png)

```bash
drwxrwsr-x 1 root staff 512 Apr 24  2018 /var/local
# This folder /var/local has the letter ‘s’ on the same place you expect to see ‘x’ or ‘-‘ for group permissions.
```

> A practical example of `SGID` is with `Samba server` for sharing files on your local network. It’s guaranteed that all new files will not lose the permissions desired, no matter who created it.

```bash
chmod g+s directory_name
# You may also use the numeric way. You just need to add a fourth digit to the normal permissions. The octal number used to SGID is always 2.
chmod 2775 folder1

chmod g-s folder2
chmod 0775 folder2

find . -perm /2000
```
---
## What is a Sticky Bit ? 
**The sticky bit works on the directory. With sticky bit set on a directory, all the files in the directory can only be deleted or renamed by the file owners only or the root.**

![sticky](https://linuxhandbook.com/content/images/2020/06/sticky-bit-linux.png)

**This is typically used in the `/tmp` directory that works as the trash can of temporary files.**
* The folder `/tmp`, has the letter `t` on the same place we expect to see `x or –` for others permissions. 

```bash
drwxrwxrwt 1 root root 512 Apr 12 13:24 /tmp
# This means that a user (except root) cannot delete the temporary files created by other users in the /tmp directory.
```

### How to set the Sticky Bit ? 
The numeric way is to add a fourth digit to the normal permissions. The octal number used for sticky bit is always `1`.
```bash
chmod +t my_dir
chmod 1775 tmp2/

chmod -t my_dir
chmod 0775 tmp2

find . -perm /1000
```

`t` or `T` in the world of permissions field indicates the sticky bit.

---
## Common Practices
1. One of the most common uses of setuid is in password management programs like `passwd`.
2. Disk quota management programs often use the `setuid` mechanism. These programs need to access and modify the quota information stored in system files, which require root privileges. 
