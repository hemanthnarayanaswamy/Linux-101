## Visualizing File System

The `tree` command in Bash is a powerful utility for visualizing the directory structure of a path or the entire file system in a tree-like format. 
- It lists directories, subdirectories, and files in a hierarchical manner, making it easier to understand the organization of files and directories within a given path.

```bash
sudo apt-get install tree

tree                 # This will output the directory structure starting from the current directory.
tree -d target_directory # List only directories:
 tree -a target_directory # Include hidden files
  tree -L 2 # display to a certian depth
   tree -p target_directory # List files with permissions
```
* use `man` pages to know more about the ability of `tree` command.

```bash
root@ubuntu-host / ✖ tree -d -L 1
.
├── bin -> usr/bin
├── bin.usr-is-merged
├── boot
├── dev
├── etc
├── home
├── lib -> usr/lib
├── lib.usr-is-merged
├── lib64 -> usr/lib64
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin -> usr/sbin
├── sbin.usr-is-merged
├── srv
├── sys
├── tmp
├── usr
└── var
```