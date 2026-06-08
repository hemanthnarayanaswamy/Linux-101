# [`grep`](https://linuxize.com/post/how-to-use-grep-command-to-search-files-in-linux/)
*  It is used to search for specific words, phrases, or patterns inside text files, and shows the matching lines on your screen.

```ini
grep [options] pattern [files]

* [options] These are command-line flags that modify the behavior of grep.
* [pattern] This is the regular expression you want to search for.
* [file] This is the name of file you want to search within. You can specify multiple files for simultaneous searching. 
```

<table><thead><tr><th>Option</th><th>Description</th></tr></thead><tbody><tr><td><code>-i</code></td><td>Ignore case</td></tr><tr><td><code>-n</code></td><td>Show line numbers</td></tr><tr><td><code>-v</code></td><td>Invert match - negation</td></tr><tr><td><code>-w</code></td><td>Match whole words</td></tr><tr><td><code>-r</code></td><td>Recursive search</td></tr><tr><td><code>-E</code></td><td>Extended regex</td></tr><tr><td><code>-F</code></td><td>Fixed strings (no regex)</td></tr><tr><td><code>-H</code></td><td>Always show filename</td></tr><tr><td><code>-q</code></td><td>Quiet mode (exit status only) - Used in Bash Scripts</td></tr><tr><td><code>-o</code></td><td>Prints only the matched portion of each line, not the entire line.</td></tr></tbody></table>

#### Basic Search 
* Find matching lines in a file

<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>grep "pattern" file.txt</code></a></td><td>Search a single file</td></tr><tr><td><code>grep -i "pattern" file.txt</code></td><td>Case-insensitive search</td></tr><tr><td><code>grep -n "pattern" file.txt</code></td><td>Show line numbers</td></tr><tr><td><code>grep -v "pattern" file.txt</code></td><td>Return Lines where pattern is not a match - Invert Match</td></tr><tr><td><code>grep -w "word" file.txt</code></td><td>Match whole words - By default, GREP matches the patter inside word</td></tr></tbody></table>

#### Recursive Search
* To recursively search for a pattern use `-r`, It searches through all files in the specified directory and its subdirectories, skipping symlinks that are encountered recursively. 

<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>grep -r "pattern" dir/</code></td><td>Recursive search</td></tr><tr><td><code>grep -r --include="*.conf" "pattern" dir/</code></td><td>Include file types</td></tr><tr><td><a href="/post/grep-exclude/"><code>grep -r --exclude="*.log" "pattern" dir/</code></a></td><td>Exclude file types</td></tr><tr><td><code>grep -r --exclude="*.log" --exclude="*.tmp" "pattern" /path/to/directory</code></td><td>To exlude multiple file types</td></tr><tr><td><code>grep -r --exclude-dir="node_modules" "pattern" /path/to/directory</code></td><td>To exlude a directory from the search</td></tr></tbody></table>

#### Multiple Files
* Search across multiple files, In the *current working directory by default* or provided file path is explicity defined. 

<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>grep "pattern" file1 file2</code></td><td>Search specific files</td></tr><tr><td><code>grep "pattern" *.log</code></td><td>Search by glob</td></tr><tr><td><code>grep -l "pattern" *.log</code></td><td>Show matching filenames</td></tr><tr><td><code>grep -L "pattern" *.log</code></td><td>Show non-matching filenames</td></tr></tbody></table> 

#### To get Aggreated Insights
<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>grep -c "pattern" file.txt</code></td><td>Count matching lines</td></tr><tr><td><code>grep -o "pattern" file.txt</code></td><td>print only the matched portion of each line, rather than the entire line</td></tr><tr><td><code>grep -m 1 "pattern" file.txt</code></td><td>Stop after 1 match</td></tr></tbody></table>

#### Working inside Bash Scripts
* The `-q` option (or `--quiet`) tells grep to run silently without displaying anything on standard output. If a match is found, the command exits with status `0`. *This is useful in shell scripts where you want to check whether a file contains a string and perform an action based on the result.*
* When using grep in shell scripts, you can pass shell variables as the search pattern which supports variable expansion. Enclose the variable in double quotes to prevent word splitting and globbing.

```bash
pattern="search term"
filename="hello.txt"

if grep -q "$pattern" "$filename"
then
    echo pattern found
else
    echo pattern not found
fi
```

#### Using `grep` to Filter Command Output
* A command’s output can be filtered with `grep` through piping `|`, and only lines matching a given pattern will be printed.
* For Instance, To find which processes are running as user **root**, you can pipe the output of the `ps -ef` to command `grep`
```bash
ps -ef | grep "root"
# We can use the suitables options or glob patters as disscussed above in this usecase as well
```
> Note that `grep` may also match its own process in the output. A common trick to avoid this is to use a character class in the pattern. 
`ps -ef | grep "[r]oot"` By wrapping the first character in brackets `[]`, the `grep` command itself no longer matches the pattern.

#### grep Anchors
* **Anchors** in `grep` are special characters used in regular expressions to match patterns at specific positions within a line. The two primary anchors are: `^ $`
* By default, grep interprets the pattern as a [basic regular expression (BRE)](https://linuxize.com/post/regular-expressions-in-grep/), where all characters except the meta-characters match themselves.

1. `^` - Matches the start of a line. `grep "^kangaroo" file.txt` Matches only if it occurs at the beginning of a line.
2. `$` - Matches the end of line. `grep "kangaroo$" file.txt` *kangaroo* matches only if it occurs at the end of a line.
3. `.` - Matches any single character. `grep "kan..roo" file.txt`. To match anything that begins with *kan*, then has **two characters**, and ends with *roo*:
4. `[]` - Matches any single character enclosed in the brackets. `grep "acce[np]t" file.txt` For instance, to find lines that contain *accept* and *accent*.
5. `[^ ]` - Matches any single character not enclosed in the brackets. `grep "co[^l]a" file.txt`.  The following pattern matches strings like *coca or coma, but not cola*.
6. Combining Anchors - You can combine `^ $` to match entire lines. `grep "^exact_line$" file.txt`. This Matches lines that contain the exact patter.
7. **FILTER COMMENTS** `-v ^#` - Exclude lines starting with `#` in configuration file. `grep -v "^#" .env`

### Search for Multiple Strings (Patterns) - [Regular Expressions](https://linuxize.com/post/regular-expressions-in-grep/)
* Two or more search patterns can be joined using the OR operator `|`, With the `-E (--extended-regexp)` option, grep uses [extended regular expressions](https://linuxize.com/post/regular-expressions-in-grep/).
* `egrep` is alias for `grep -E`
```bash
grep -E 'pattern1|pattern2' file
grep -E 'regular expression patter' file

grep -E 'fatal|error|critical' /var/log/nginx/error.log
egrep 'fatal|error|critical' /var/log/nginx/error.log

grep -E -o "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" file.txt
# Extracts all email Addresses form a file.

grep -E -o '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' file.txt
# Extracts all valid IPV4 addresses from a file
```
### Combining `grep` with `find`
* When you need more control over which files are searched, combine `grep` with the `find` command using `-exec`
* This is helpful when you want to filter by file attributes such as *modification time, size, or permissions* before searching their contents.

```bash
# To search for "TODO" in all .py files modified in the last seven days.

find . -type f -name "*.py" -mtime -7 -exec grep -l "TODO" {} +
# The find command locates the matching files, and grep -l prints the names of files that contain the pattern. Using + instead of \; passes files to grep in batches, which is much faster than invoking grep once for each file.
```
### [Grep Character Class](https://www.tecmint.com/linux-grep-commands-character-classes-bracket-expressions/)
* **character classes** (or character sets) are used to match any one character from a specified set of characters. They are defined by enclosing the characters in square brackets `[]`.

```ini
[abc] matches any one character that is either 'a', 'b', or 'c'.

[a-z] matches any one lowercase letter from 'a' to 'z'.

[A-Z] matches any one uppercase letter from 'A' to 'Z'.

[0-9] matches any one digit from '0' to '9'.
```

1. `grep "^[[:alnum:]]" file.txt`: To find lines starting with alphanumeric characters *(A-Z, a-z, 0-9)*.
2. `grep "[[:alpha:]]" file.txt`: Matches alphabetic characters *(A-Z, a-z)*.
3. `grep "[[:digit:]]" file.txt`: Matches digits *(0-9)*.
4. `grep "[[:lower:]]" file.txt`: Matches lowercase letters *a-z*
5. `grep "[[:upper:]]" file.txt`: Matches uppercase letters *A-Z*
6. `grep "[[:space:]]" file.txt`: Matches whitespace characters *space, tabs*
7. `grep "[[:punct:]]" file.txt`: Matches punctuation characters *(!, @, #, etc.)*
---