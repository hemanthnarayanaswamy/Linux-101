# <center>Hard Links vs Soft Links</center>

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
# <center>Absolute Path vs Relative Path</center>

A path is how you refer to files and directories. It gives the location of a file or directory in the Linux directory structure. It is composed of a name and slash syntax.

```
/home/hemanthn/scripts
```
-  The **Absolute path** always starts from the root directory `(/)`. For example, `/home/hemanthn/scripts/my_scripts.sh`
-  A **relative path** starts from the current directory. For example, if you are in the `/home` directory and you want to access the `my_scripts.sh` file, you can use `hemanthn/scripts/my_scripts.sh`.

![path1](https://linuxhandbook.com/content/images/2021/04/absolute-vs-relative-path-linux.png)
---

# <center>Anatomy of the Prompt in Linux</center>

When you open a terminal in Linux, the line that awaits your command input is known as the shell prompt. This prompt can provide essential information about the current user, machine, directory, and other contextual data. However, the prompt isn't just a static piece of text—it's highly customizable and can be configured to provide different kinds of data according to user preferences. 

```ini
username@hostname:~$

username: is the name of the current user
hostname: is the name of the machine
~ represents the home directory of the user
$ indicates that you're a regular user 
# if your see # that means you're the root user

Example:
alex@ubuntumachine:/home/alex/projects$    --> Normal User $

root@ubuntumachine:/home/alex/projects#    --> Root User #
```
The appearance and content of the prompt are determined by the value of a shell environment variable called `PS1`

```bash
echo $PS1
\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\u@\h:\w\$
```

<table><thead><tr><th>Segment</th><th>Description</th></tr></thead><tbody><tr><td><code>\[\e]0;</code></td><td>Begins a sequence to set the title of the terminal window.</td></tr><tr><td><code>\u</code></td><td>The username of the current user.</td></tr><tr><td><code>@\h</code></td><td>The "@" symbol followed by the hostname of the machine (up to the first <code>.</code>).</td></tr><tr><td><code>: \w</code></td><td>A colon followed by the current working directory. <code>\w</code> provides the full path.</td></tr><tr><td><code>\a\]</code></td><td>Ends the sequence for setting the terminal title.</td></tr><tr><td><code>${debian_chroot:+($debian_chroot)}</code></td><td>Conditional inclusion. If <code>debian_chroot</code> is set, it will insert its value.</td></tr><tr><td><code>\u@\h:\w\$</code></td><td>Displays the username, the hostname, the current directory, and <code>$</code> or <code>#</code> depending on the user.</td></tr></tbody></table>

## [Customizing the Linux Prompt](https://cloudaffle.com/series/customizing-the-prompt/changing-information/)

The default prompt may not provide the exact information or appearance that you prefer. Customizing the prompt is a straightforward task.

<table><thead><tr><th>Escape Code</th><th>Description</th></tr></thead><tbody><tr><td><code>\a</code></td><td>An ASCII bell character (07)</td></tr><tr><td><code>\d</code></td><td>The date, in "Weekday Month Date" format</td></tr><tr><td><code>\D{format}</code></td><td>The format is passed to strftime(3) and the result is inserted into the prompt string; an empty format results in a locale-specific time representation</td></tr><tr><td><code>\e</code></td><td>An ASCII escape character (033)</td></tr><tr><td><code>\h</code></td><td>The hostname, up to the first <code>.</code></td></tr><tr><td><code>\H</code></td><td>The full hostname</td></tr><tr><td><code>\j</code></td><td>The number of jobs currently run in the background</td></tr><tr><td><code>\l</code></td><td>The basename of the shell’s terminal device name</td></tr><tr><td><code>\n</code></td><td>Newline</td></tr><tr><td><code>\r</code></td><td>Carriage return</td></tr><tr><td><code>\s</code></td><td>The name of the shell (i.e., "bash")</td></tr><tr><td><code>\t</code></td><td>The current time, in 24-hour HH:MM:SS format</td></tr><tr><td><code>\T</code></td><td>The current time, in 12-hour HH:MM:SS format</td></tr><tr><td><code>\@</code></td><td>The current time, in 12-hour am/pm format</td></tr><tr><td><code>\A</code></td><td>The current time, in 24-hour HH:MM format</td></tr><tr><td><code>\u</code></td><td>The username of the current user</td></tr><tr><td><code>\v</code></td><td>The version of Bash</td></tr><tr><td><code>\V</code></td><td>The release of Bash, version + patch level</td></tr><tr><td><code>\w</code></td><td>The current working directory</td></tr><tr><td><code>\W</code></td><td>The basename of the current working directory</td></tr><tr><td><code>\\</code></td><td>A literal backslash</td></tr><tr><td><code>\!</code></td><td>The history number of this command</td></tr><tr><td><code>\#</code></td><td>The command number of this command</td></tr><tr><td><code>\$</code></td><td>If the effective UID is 0, <code>#</code>, otherwise <code>$</code></td></tr></tbody></table>

---
# <center>Globbing - Wildcards</center>

Wildcards in Linux are special characters that help match file and directory names based on patterns rather than exact names.

- Globbing is filename pattern expansion done by the shell (e.g., bash, zsh) before a command runs.
- It matches directory entries, not file contents.
- If a pattern matches multiple files, the shell expands it into a list of paths.

### Asterisk `*`
* Matches any string, including empty, within a single path segment.
* Does not cross `/` boundaries

```bash
ls *.log        # all files ending with .log
rm img*         # img, img1, img_2025, etc.
echo *          # expands to all entries in current dir
```
### Question Mark `?`
* Matches exaclty one character in a name. 

```bash
ls file?.txt    # file1.txt, fileA.txt (but not file10.txt)
ls ?.md         # a.md, z.md (one character only)
```
### Square Brackets `[]`
* It is used to specify a range or a set of characters. 
* You can use ranges like `0-9`, `a-z`. 
* Example:  if you want to list all files starting with a letter from a to c, you can use the following command: `ls [a-c]*`
* Using `^` or `!` instead of match you can do negate. like `[!abc], [^abc]`.

```bash
ls file[123].txt    # file1.txt, file2.txt, file3.txt
ls report[0-9].pdf  # report1.pdf ... report9.pdf
ls img[^0-9].png    # imgA.png, img_.png, etc.
```
### Curly Brackets `{}`
* Not a glob; it is brace expansion done by the shell. 
* Expands not multiple words; each word can then be globbed. 
* For example, if you want to copy files named file1.txt and file2.txt to a directory named backup, you can use the following command: `cp file{1,2}.txt backup/`

```bash
echo {a,b,c}          # a b c
ls file{1,2,3}.txt    # file1.txt file2.txt file3.txt
cp {src,backup}/app.conf /tmp/
```
### Combine with Globs
```bash
ls *.{jpg,png,gif}    # expands to *.jpg *.png *.gif, then globs

ls [a-c]*.txt

mkdir {src,tests,docs}
```
---
# <center>Redirection and Pipes</center>

Every process starts with 3 standard streams:
- `0` = stdin (input)-
- `1` = stdout (normal output)
- `2` = stderr (errors)
Redirection changes where these streams read from or write to.

![redir](https://linuxhandbook.com/content/images/2020/06/Linux-redirection-normal-flow.png)

### Pipe `|`
* Connects **stdout of left command to stdin** of right command.
* Only `stdout` is piped by default, stderr stays on the terminal unless you redirect it.
* `|` does not pipe stderr unless you redirect it.

![pipe](https://linuxhandbook.com/content/images/2020/06/pipe-redirection.png)

```bash
command_1 | command_2 | command_3 | command_4

ls | wc -l
# Sort and remove duplicates
sort record.txt | uniq
```
### Redirect stdout: `>` and `>>`
* `>` sends stdout to a file, overwriting it.
* `>>` sends stdout to a file, appending.

```bash
ls > files.txt       # overwrite
ls >> files.txt      # append
```
**If the file does not exist, it is created.**

### Redirect stderr: `2>`
* `2>` redirects only stderr.
* Useful for separating erros from normal output. 

```bash
find /root 2> errors.txt
```
### Redirect Both stdout and stderr

##### **Redirect stdout and stderr to Differnt File**

* `command > out.log 2> err.log` or `command >> out.log 2>> err.log`

##### **Redirect Both stdout and stderr to Same File**
`command >> output.log 2>&1`

`2>&1`: *send stderr 2 to wherever stdout 1 is currently going*

```bash
ls -l new.txt fff 2>> combined.txt >> combined.txt  

# Above can written as below
ls -l new.txt fff > output.txt 2>&1
```
> Keep in mind that you cannot use 2>>&1 thinking of using it in append mode. 2>&1 goes in append mode already.

*  `2>` first and then use `1>&2` to redirect stdout to same file as stderr. Basically, it is `“>&”` which redirects one out data stream to another.

```bash
ls -l fff 2>> test.txt 1>&2
```

##### Shorthand `&>`
* `&>` redirects both stdout and stderr to the same place.
* Equivalent to `command > file 2>&1`

```bash
command &> all.txt
command &>> all.txt   # append both

command > /dev/null 2>&1
# or
command &> /dev/null
```