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

## Summary 

Ouput redirection in Bash allows you to control where the output of a command is sent, which is essential for managing data flow in your scripts. 
* `>`: Redirects ouput to a file, overwriting it
* `>>`: Appends ouput to a file without overwriting. 

* `2>`: Redirects standard error to a file.
* `&>`: Redirects both standard output and error to a file.

```bash
echo "Hello, World!" > output.txt # output to file
ls non_existent_file 2> error.log # output errors to file

ls >> output.txt # append output to file
```
**If you want to suppress output, you can redirect it to `/dev/null`, effectively discarding it:

`/dev/null` is a special virtual file often referred to as the **"black hole"** Any data written to it is discarded, and reading from it provides no output. It is widely used for managing command outputs and errors efficiently.

```bash
command > /dev/null 2>&1 
# Here both the std ouput and error are redirected to /dev/null
```
* To combine standard output and error streams into the same file, you can use `2>&1`:
---