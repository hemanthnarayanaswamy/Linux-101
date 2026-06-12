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

### Syntax:

```
tree  [-acdfghilnpqrstuvxACDFQNSUX]  [-L  level [-R]] [-H baseHREF] [-T title]
      [-o filename] [--nolinks] [-P pattern] [-I  pattern]  [--inodes]
      [--device] [--noreport] [--dirsfirst] [--version] [--help] [--filelimit #]
      [--si] [--prune] [--du] [--timefmt  format]  [--matchdirs]  [--from-file]
      [--] [directory ...]
```

### Additional Flags and their Functionalities:

|**Flag**   |**Description**   |
|:---|:---|
|`-a`|Print all files, including hidden ones.|
|`-d`|Only list directories.|
|`-l`|Follow symbolic links into directories.|
|`-f`|Print the full path to each listing, not just its basename.|
|`-x`|Do not move across file-systems.|
|`-L #`|Limit recursion depth to #.|
|`-P REGEX`|Recurse, but only list files that match the REGEX.|
|`-I REGEX`|Recurse, but do not list files that match the REGEX.|
|`--ignore-case`|Ignore case while pattern-matching.|
|`--prune`|Prune empty directories from output.|
|`--filelimit #`|Omit directories that contain more than # files.|
|`-o FILE`|Redirect STDOUT output to FILE.|
|`-i`|Do not output indentation.|