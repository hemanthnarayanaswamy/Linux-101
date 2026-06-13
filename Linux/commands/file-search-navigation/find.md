# The `find` Command

The `find` command is one of the most powerful Linux utilities that lets you search for files and directories based on various conditions like name, size, modification time, permissions, and more.

## Basic Syntax

```
find [path] [options] [expression]
```

**In simple words:**  
> “Search starting from `[path]`, apply `[options or filters]`, and then perform an `[action]` on the result.”

Example:

```
find /home/user -name "*.log"
# This searches for all `.log` files under `/home/user`.
```
### Common Options 

<table><thead><tr><th>Option</th><th>Description</th></tr></thead><tbody><tr><td><code>-name</code></td><td>Match filename (case-sensitive)</td></tr><tr><td><code>-iname</code></td><td>Match filename (case-insensitive)</td></tr><tr><td><code>-type</code></td><td>Filter by file type</td></tr><tr><td><code>-size</code></td><td>Filter by size</td></tr><tr><td><code>-mtime</code></td><td>Filter by modification time (days)</td></tr><tr><td><code>-maxdepth</code></td><td>Limit recursion depth</td></tr><tr><td><code>-mindepth</code></td><td>Skip top levels</td></tr><tr><td><code>-prune</code></td><td>Exclude directories</td></tr><tr><td><code>-exec</code></td><td>Execute command on matches</td></tr><tr><td><code>-empty</code></td><td>Find empty files/directories</td></tr><tr><td><code>-mmin</code></td><td>Filter by modification time (minutes)</td></tr><tr><td><code>-delete</code></td><td>Delete matched files</td></tr></tbody></table>

## Common Use Cases

### 1. Search a File by Exact Name

```bash
find ./directory1 -name sample.txt
```

Finds `sample.txt` inside `directory1` and all its subdirectories.

Case-insensitive search:

```bash
find ./directory1 -iname "sample.txt"
```

### 2. Search Files by Pattern (Wildcard)

Find all `.txt` files under `directory1`.

```
find ./directory1 -name "*.txt"
```

More examples:
```
find /var/log -name "*.log"
find /etc -name "conf*"
```

### 3. Find Directories by Name

```
find / -type d -name test
```
Lists all directories named `test` from the root `/`.

### 4. Find Empty Files and Directories

```
find . -empty
```
Finds both empty files and directories in the current folder.

Only empty files:
```
find . -type f -empty
```

### 5. Find Files by Type
Sometimes you might need to search for specific file types such as regular files, directories, or symlinks. In Linux, everything is a file.

use the `-type` option and one of the following descriptors to specify the file type:

* `f`: a regular file
* `d`: directory
* `l`: symbolic link
* `c`: character devices
* `b`: block devices
* `p`: named pipe (FIFO)
* `s`: socket

A common example is to recursively change the website file permissions to `644` and directory permissions to `755` using the `chmod`, we can achive this by using `exec` or `xargs` command.

> The `exec` command in Linux is a shell built-in used to replace the current shell with another command. Unlike normal commands that start a new process, exec does not create a new process. Instead, it runs the given command in place of the current shell.
> `exec` is used to run other command after `find` finds the intended files.
> use `xargs` instead


```bash
find /var/www/my_website -type d -exec chmod 0755 {} \;
find /var/www/my_website -type f -exec chmod 0644 {} \;
```
### 5. Find Files by Modification or Access Time

| Option | Description |
|--------|--------------|
| `-mtime n` | Modified *n* days ago |
| `-atime n` | Accessed *n* days ago |
| `-ctime n` | Changed *n* days ago |
| `-mmin n`  | Modified *n* minutes ago |

Examples:

Find files modified within the last 2 days.
```
find /var/log -mtime -2
```

Find files modified within the last hour.
```
find . -mmin -60
```

### 6. Find Files by Size

| Expression | Meaning |
|-------------|----------|
| `+n` | Greater than n |
| `-n` | Less than n |
| `n` | Exactly n |

* `b`: 512-byte blocks (default)
* `c`: bytes
* `k`: Kilobytes
* `M`: Megabytes
* `G`: Gigabytes

Examples:

Find files larger than 100 MB.
```
find / -size +100M
```

Find files smaller than 10 KB.
```
find . -size -10k
```

### 7. Find Files Modified More Recently than Another File

```
find . -newer reference.txt
```
Lists files modified after `reference.txt`.

### 8. Find Files by Type

| Type | Description |
|------|--------------|
| `f` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `b` | Block device |
| `c` | Character device |

Example:

Find all block devices.
```
find /dev -type b
```

### 9. Find Files by Permission
Find files with exactly 644 permissions.
```
find /var/www -type f -perm 644
```

**Find files executable by anyone.**
```
find /usr -type f -perm /111
```

### 10. Execute Commands on Found Files

You can use the `-exec` option to run a command on each file found:

```
find . -type f -name "*.log" -exec rm -f {} \;
```
Deletes all `.log` files under the current directory.

**Alternative (faster):**

```
find . -type f -name "*.log" | xargs rm -f
```

### 11. Combine Multiple Conditions

You can combine filters together:

```
find /var/log -name "*.log" -size +50M -mtime -2
```
Finds `.log` files larger than 50 MB and modified within the last 2 days.

### 12. Print Only File Names (Quiet Output)
```
find /etc -type f -name "*.conf" -print
```
The `-print` flag ensures output is displayed (it’s the default in most systems).

## Quick Reference Table

| Task | Command |
|------|----------|
| Find files with specific name | `find . -name filename.txt` |
| Case-insensitive search | `find . -iname filename.txt` |
| Find directories only | `find . -type d` |
| Find empty files | `find . -type f -empty` |
| Find files > 1 GB | `find . -size +1G` |
| Find modified in last day | `find . -mtime -1` |
| Delete `.tmp` files | `find . -name "*.tmp" -delete` |
| Search and execute | `find . -name "*.log" -exec gzip {} \;` |

## Getting Help

To view the complete guide for the `find` command, run:

```
man find
```
