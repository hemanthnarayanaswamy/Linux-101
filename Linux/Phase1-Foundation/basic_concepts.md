# Hard Links vs Soft Links

A Linux file is really made of two parts:
1. **File name**: what you see, like notes.txt
2. **Inode**: the actual filesystem record that points to the file’s data.

A directory is basically a list like this: **Links are different ways of pointing to files.**

```ini
notes.txt --> inode 12345
```
---
## Hard Links
A hard link is another filename pointing to the same inode.

```ini
original.txt  -> inode 12345
hardlink.txt  -> inode 12345
```
- If you edit one: You will see the update, because both names refer to the same file.
- If you delete `original.txt`, `hardlink.txt` still works. The data is deleted only when the last hard link is removed. 

```bash
ls -li

12345 -rw-r--r-- 2 user user 12 original.txt
12345 -rw-r--r-- 2 user user 12 hardlink.txt

Same inode number: 12345
Link count: 2
```
---
## Soft Links 

A soft link, also called a symbolic link or symlink, is more like a shortcut. 
- It is a special type of file that simply points to another file or directory just like *shortcuts in Windows*. 
- Creating symbolic link is like creating alias to an actual file.

```bash
echo "hello" > original.txt
ln -s original.txt softlink.txt

original.txt  -> inode 12345
softlink.txt  -> points to path "original.txt"
```
- The symlink has its own inode. It does not point directly to the file data. It points to the file path.
- If you read the symlink, Linux follows the link and reads `original.txt`.
- But if you delete the original file: The symlink **breaks**, because it still points to a path that no longer exists. This is called a `broken symlink`.

![soft-hard](https://i.pinimg.com/736x/14/7d/0f/147d0f0707c1b407c914ac4036d25d25.jpg)

---
# Absolute Path vs Relative Path

A path is how you refer to files and directories. It gives the location of a file or directory in the Linux directory structure. It is composed of a name and slash syntax.

```
/home/hemanthn/scripts
```
-  The **Absolute path** always starts from the root directory `(/)`. For example, `/home/hemanthn/scripts/my_scripts.sh`
-  A **relative path** starts from the current directory. For example, if you are in the `/home` directory and you want to access the `my_scripts.sh` file, you can use `hemanthn/scripts/my_scripts.sh`.

![path1](https://linuxhandbook.com/content/images/2021/04/absolute-vs-relative-path-linux.png)
---