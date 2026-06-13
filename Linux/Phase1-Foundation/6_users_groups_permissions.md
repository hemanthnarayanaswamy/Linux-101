# User & Group Management & Permissions

## Users

In Linux, a user is an **individual or a process** that can access the system. Each user has a unique user ID `(UID)` and usually a **username**.
* The UID is a numerical value that the system uses to identify the user internally, while the username is a human-readable string.

Linux systems typically support up to **1,000+** users, making them suitable for large-scale use.

![users](https://media.geeksforgeeks.org/wp-content/uploads/20250730100822742759/linux_user_ids.webp)

### Types of User
Each user type serves a specific purpose and has different levels of access and control.

<table><thead><tr><th style="text-align: center;"><span>User Type</span></th><th style="text-align: center;"><span>Description</span></th></tr></thead><tbody><tr><th style="text-align: center;"><span>Root (Superuser)</span></th><td style="text-align: center;"><span>Full system control. Can install software, change config files, and delete anything. Powerful but risky.</span></td></tr><tr><th style="text-align: center;"><span>Regular User</span></th><td style="text-align: center;"><span>Limited access. Can create files, run applications, but not modify system-level settings.</span></td></tr><tr><th style="text-align: center;"><span>Sudo User</span></th><td style="text-align: center;"><span>Regular user with temporary admin rights via the sudo command. Common in modern systems.</span></td></tr><tr><th style="text-align: center;"><span>System/Service Account</span></th><td style="text-align: center;"><span>Non-human accounts used by services (e.g., mysql, nginx). Limited privileges.</span></td></tr><tr><th style="text-align: center;"><span>Guest User</span></th><td style="text-align: center;"><span>Temporary users with minimal privileges. Changes are not saved after logout. (desktop environment specific)</span></td></tr></tbody></table>

---
## Groups/User Groups
A user group is a collection of users. If you give permission to a group, all users in that group get the same access. This makes it easier to manage file and system permissions for many users at once. Each group has a **unique group ID** (`GID`) and a **group name**.

### Different Groups

##### 1. Primary Group (default for files)
* Every Linux user is assigned one primary group.
* When a user creates a file, the group ownership of that file is automatically set to their primary group.
* By default, this group usually has the same name as the user.
* It helps manage file ownership cleanly without much extra configuration.

##### 2. Secondary Groups
A secondary group is any additional group a user is added to, allowing them to share files and resources with other users in that group. Unlike the primary group, belonging to secondary groups is optional, and a user can belong to multiple secondary groups (up to 15 by default).
- They are commonly used for team-based access or system-level permissions (e.g., accessing Docker, video devices, or running sudo).

---
## User Management Files 

These files are essential for managing users, groups, and permissions on a Linux system, and they play a key role in ensuring security and efficient system administration.

![files](https://media.geeksforgeeks.org/wp-content/uploads/20250722180805763872/user_management.webp)

#### 1. `/etc/passwd`
File stores user account information. You can view its contents using the `cat` command.

Each line in the `/etc/passwd` file represents a user account and is divided into seven fields separated by colons (`:`)

#### 2. `/etc/shadow`
Files Stores encrypted user passwords and password-related settings.

Contains information like encrypted password, last pasword change date, password expiration and account enpiration.

#### 3. `/etc/group`
The `/etc/group` file stores group information. Defines all groups in the system and user memberships.

Contains information about all the users part of that group. 

#### 4. `/etc/gshadow`
Secure counterpart to `/etc/group`, storing: 
* group password
* group administrators
* group members

#### 5. `/etc/sudoers`
Manages sudo access for users and groups:
* who can use the sudo command.
* What commands they can run.

#### 6. `/etc/skel`
Directory containing default configuration files copied to a new user’s home directory

* Typically includes `.bashrc`, `.profile`, etc.
* Used to provide default shell settings and environment.

#### 7. `/var/log/auth.log`
Records authentication-related events:
* all login attempts
* usage of sudo command
* Account lock and unlock events
* other security-related activities. 

---
## User Identification Commands

<table>
<thead>
<tr>
<th>Command</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>who</td>
<td>Lists all logged-in users</td>
<td><a href="../commands/users-groups-permissions/who.md">who command</a></td>
</tr>
<tr>
<td>id</td>
<td>Get user and group ID</td>
<td><a href="https://linuxhandbook.com/id-command/">id command</a></td>
</tr>
<tr>
<td>finger</td>
<td>Logged in user information</td>
<td><a href="https://linuxhandbook.com/finger-command/">finger command</a></td>
</tr>
</tbody>
</table>

* The  `id` command provides the user with user ID (UID) and group ID (GID) of the username by default.

```bash
id username
```
```bash
cat /etc/passwd           # View user account database
cat /etc/shadow           # View password database
cat /etc/group            # View group database
cat /etc/gshadow          # View secure group information
```
---
## User account Management Commands

<table>
<thead>
<tr>
<th>Command</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>useradd</td>
<td>Create a new user account</td>
<td><a href="../commands/users-groups-permissions/useradd.md">useradd command</a></td>
</tr>
<tr>
<td>userdel</td>
<td>Delete an existing user</td>
<td><a href="../commands/users-groups-permissions/userdel.md">userdel command</a></td>
</tr>
<tr>
<td>usermod</td>
<td>Modify user account</td>
<td><a href="../commands/users-groups-permissions/usermod.md">usermod command</a></td>
</tr>
<tr>
<td>passwd</td>
<td>Manage use passwords</td>
<td><a href="../commands/users-groups-permissions/passwd.md">passwd command</a></td>
</tr>
</tbody>
</table>

---
## Group Management Commands

<table>
<thead>
<tr>
<th>Command</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>groupadd</td>
<td>Creates new groups</td>
<td><a href="https://linuxhandbook.com/groupadd-command/">groupadd command</a></td>
</tr>
<tr>
<td>groupdel</td>
<td>Delete a group</td>
<td><a href="https://linuxhandbook.com/groupdel-command/">groupdel command</a></td>
</tr>
<tr>
<td>groupmod</td>
<td>Modify group properties</td>
<td><a href="https://linuxhandbook.com/groupmod-command/">groupmod command</a></td>
</tr>
</tbody>
</table>

---
## Common Issues in User Management in Linux

### Privilege Escalation Risks
Improper configurations can allow unauthorized privilege escalation.

Solution: Review and edit the `/etc/sudoers` file carefully, preferably using `visudo` to prevent syntax errors.

```bash
sudo visudo
```

###  Misconfigured User Management Files
Errors in critical files like `/etc/passwd` and `/etc/shadow` can disrupt user management.

**Use commands like `vipw` and `vigr` to safely edit these files:**

```bash
sudo vipw
sudo vigr
```
These commands lock the files during editing, preventing concurrent modifications.

---
## Permissions

use the commands `chmod`, `chown`, `chgrp` to change the permissions on files and directories. 

[umask](https://www.geeksforgeeks.org/linux-unix/umask-command-in-linux-with-examples/) umask (user file-creation mode mask) defines the default permission bits that are removed from newly created files and directories. It acts as a filter, ensuring certain permissions are not granted by default.

By default:
1. **Files** start with permissions 666 (read & write for all).
2. **Directories** start with permissions 777 (read, write & execute for all).
The **umask** value is subtracted from these defaults to determine the final permissions.
* `0002` The first digit (often 0) is the special mode (sticky/setuid/setgid), and the last three digits represent owner, group, and others restrictions.

```bash
# To view the umask value 
umask

# To set a temp umask value
umask 0644

# To set umask permanently edit files 
vi /etc/profile   # -> umask 0270
vi /ect/bashrc    # -> umask u=rwx,g=rx,o=
```

> `umask` only **removes** permissions, never adds them.

---
## Special Permissions

* Apart from permissions we have special File Permissions. `SUID` `GUID` and `Sticky Bit`. called ***SPECIAL BITS***.

[Special Bits](../concepts/filesystem-permissions/special_bits.md)

