# File Operations 

## **Create Files**

```bash
touch file1.txt           # Create an empty file
echo "hello" > file2.txt  # Create file with content
echo "world" >> file2.txt # Append content to file
cat > file3.txt           # Create file and add content using stdin
```

## **View File Content**

```bash
cat file1.txt             # Display file content
less file1.txt            # View file content page by page
more file1.txt            # View file content page by page
head file1.txt            # Show first 10 lines of file
tail file1.txt            # Show last 10 lines of file
tail -f file1.txt         # Monitor file in real time
```

## **Copy Files & Directories**

```bash
cp file1.txt file2.txt    # Copy file
cp -r dir1 dir2           # Copy directory recursively
cp -v file1.txt /tmp      # Copy file with verbose output
cp -p file1.txt /backup   # Preserve file attributes
```

## **Move & Rename**

```bash
mv file1.txt file2.txt    # Rename file
mv file1.txt /tmp         # Move file to another directory
mv dir1 /home/user        # Move directory
```

## **Delete Files & Directories**

```bash
rm file1.txt              # Delete file
rm -i file1.txt           # Delete file with confirmation
rm -f file1.txt           # Force delete file
rm -r dir1                # Delete directory recursively
rm -rf dir1               # Force delete directory
rmdir dir1                # Delete empty directory
```


## File Type Identification 

```bash
file filename             # Show file type
stat filename             # Show detailed file information
```

## File Information 

```bash
stat filename              # Display file size, permissions, inode, and timestamps
du -sh filename            # Show file size
du -sh directory           # show directory size
```

## iNode Information

```bash
ls -i                     # List the files and show the inode numbers of all files 
df -i                     # Show indoe usage
```

## View Permissions

![linux permissions](https://th.bing.com/th/id/R.bb23339e56913d1df461711064a93fb3?rik=CiHaX3ISv4K0KQ&riu=http%3a%2f%2fstudy-notes.org%2fimages%2finfographics%2flinuxfilepermissions.png&ehk=W8UCHb6im1ELnoj84HlGD%2bsX3y1ElTU8ByXX072GX58%3d&risl=&pid=ImgRaw&r=0)

[Access Control Lists](https://www.howtogeek.com/how-to-use-filesystem-acl-on-linux/)
ACLs provide a more flexible permission mechanism than the traditional file mode bits, allowing permissions to be set for individual users and groups.

```bash
ls -l                     # Show file permissions and ownership
stat file1.txt            # Show detailed permission information
getfacl file1.txt         # Show ACL permissions (if enabled)
```
![permissions](https://assets.bytebytego.com/diagrams/0259-linux-permissions-copy.png)


Every file or directory is assigned 3 types of owner:
1. **Owner**: the owner is the user who created the file or directory.
2. **Group**: a group can have multiple users. All users in the group have the same permissions to access the file or directory.
3. **Other**: other means those users who are not owners or members of the group.

* If you are the owner, only the owner’s permissions apply.
* If not the owner but in the group, only group permissions apply.
* If neither, others permissions apply.

## Change Permissions

```bash
chmod 777 file1.txt       # Give full permissions to all
chmod 755 file1.txt       # rwx for owner, rx for group and others
chmod 644 file1.txt       # rw for owner, r for group and others
chmod u+rwx file1.txt     # Add rwx permission to user
chmod g+rw file1.txt      # Add read and write to group
chmod o-rwx file1.txt     # Remove all permissions from others
chmod u=rwx,g=rx,o=r file1.txt   # Set exact permissions
```
## Change Ownership

```bash
chown user file1.txt              # Change owner
chown user:group file1.txt        # Change owner and group
chown -R user:group dir1          # Change ownership recursively
```

## [Access Control List](https://linuxvox.com/blog/linux-acl-list/)

ACLs, on the other hand, allow for more granular control. They can assign permissions to specific users or groups in addition to the standard owner, group, and others.

An ACL consists of multiple entries. Each entry has the following components:

- **Type**: Can be user, group, mask, other, etc.
- **user**: Defines permissions for a specific user.
- **group**: Defines permissions for a specific group.
- **mask**: Limits the effective permissions of all non-other entries.
- **other**: Defines permissions for users who do not match any other entry.
- **Who**: Specifies the user or group name (for user and group types).
- **Permissions**: A combination of read (r), write (w), and execute (x).

To check if a file or directory has an ACL, you can use the `ls -l command`. 
*If there is a `+` sign at the end of the permission string*, it means the file has an **ACL**. 

```bash
ls -l testfile
-rw-r--r--+ 1 user group 0 Jan  1 00:00 testfile
```
* To view the detailed ACL of a file, use the `getfacl` command:

```bash
getfacl testfile
# file: testfile
# owner: user
# group: group
user::rw-
group::r--
other::r--
```
##### The `setfacl` command is used to set ACLs.

- Use the `-m` option to grant permissions to a specific user or group:

```bash
setfacl -m u:john:rw file.txt
setfacl -m g:developers:r file.txt
```
*  **Remove Specific AC**L Entries Use the `-x` option to remove a user or group from the ACL:

```bash
setfacl -x u:john file.txt  
setfacl -b file.txt           # To remove all extended ACL entries but keep base permissions.
```