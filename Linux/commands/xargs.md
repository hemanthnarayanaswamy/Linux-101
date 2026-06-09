# The `xargs` command

`xargs` is used to build and execute command lines from standard input

The `xargs` command is a versatile tool that can be used in various scenarios to handle large sets of inputs and execute commands efficiently.

In Linux, both `-exec` (used with find) and `xargs` are ways to run commands on files found by find, but they differ in execution method, performance, and safety.

### Syntax:

```
some_command | xargs [options] another_command
```
`some_command` produces output that is sent to the standard input of `xargs`. `xargs` then takes this input, splits it into words, and uses these words as arguments for another_command.

The **options** for xargs allow you to customize its behavior, such as specifying the maximum number of arguments to pass to the command at a time, or changing the delimiter used to split the input. Some common options include:

* `-0`: Input items are terminated by a null character instead of white spaces.
* `-a file`: Read items from a file instead of standard input.
* `-p`: Prompt the user about whether to run each command line.
* `-r`: If the standard input does not contain any non-blanks, do not run the command.
* `-t`: Print the command line on the standard error output before executing it.

**xargs and find: made for each other**

---
## Simple Examples of `xargs`

#### Deleting Files
```bash
find /tmp -name core -type f -print0 | xargs -0 rm -f
```
Find files named **core** in or below the directory **/tmp** and delete them. Note that this will work incorrectly if there are any filenames containing newlines or spaces.

#### Running a Command on Multiple Files
```bash
ls *.txt | xargs chmod 644
```
Let's say you want to change the permission of all the .txt files in a directory to 644. You can use ls and xargs together.

#### Handling Spaces in Input
One of the challenges when using xargs is dealing with input that contains spaces. 
- By default, `xargs` splits the input into words based on whitespace, which can cause problems if your file names or other input values contain spaces.

To handle spaces properly, you can use the `-0` option 

For example, the find command has the `-print0` option to output file names separated by null characters instead of newlines.
```bash
find . -name "*.txt" -print0 | xargs -0 rm
```
`find` searches for all .txt files in the current directory and its subdirectories, and outputs the file names separated by **null characters**. 
- The `-0` option in `xargs` tells it to use null characters as the **delimiter**, ensuring that file names with spaces are treated as a single argument.

#### Compressing Files
```bash
find . -name "*.pdf" -print0 | xargs -0 gzip
```

#### Searching for Text in Files
If you want to search for a specific text pattern in all the .txt files in a directory and its subdirectories, you can use `find` and `xargs` with `grep`:
```bash
find . -name "*.txt" -print0 | xargs -0 grep "pattern"
```

#### Control the Arguments `-n`
The `-n` option allows you to specify the maximum number of arguments to pass to the command at a time.
```bash
ls *.jpg | xargs -n 5 convert -resize 50%
```

In this example, `xargs` will pass a maximum of **5 .jpg** file names to the convert command at a time, resizing each group of 5 files to 50% of their original size.