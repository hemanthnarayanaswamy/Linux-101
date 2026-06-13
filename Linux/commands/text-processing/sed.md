# `sed` - Stream Editor

* With `sed`, It processes input line by line, allowing you to perform *search, find and replace, insert, and delete words and lines*. 
* It supports basic and extended regular expressions that allow you to match complex patterns.

### Text Substitution
<table><thead><tr><th>Task</th><th>Option/Command</th><th>What It Does</th></tr></thead><tbody><tr><td>Replace first match per line</td><td><code>s/pattern/replacement/</code></td><td>Substitutes the first occurrence of “pattern” with “replacement” on each line</td></tr><tr><td>Replace all matches on each line</td><td><code>s/pattern/replacement/g</code></td><td>The <code>g</code> flag makes substitution global, replacing every occurrence</td></tr><tr><td>Editing File Directly (in-place)</td><td><code>sed -i 's/pattern/replacement/g'</code></td><td>The <code>i</code> flag modify the original file instead of just outputting the result.</td></tr><tr><td>Replace all matches on each line for multiple files</td><td><code>s/pattern/replacement/g file1.txt file2.txt</code></td><td>You can add all the list of files or use glob patterns.</td></tr></tbody></table>

`s` stands for substitution & `g` stands for global.

### Deletion Operations
<table><thead><tr><th>Task</th><th>Option/Command</th><th>What It Does</th></tr></thead><tbody><tr><td>Delete matching lines</td><td><code>/pattern/d</code></td><td>Removes entire lines that contain “pattern”</td></tr><tr><td>Delete blank lines</td><td><code>'/^$/d'</code></td><td>Removes all empty lines from output</td></tr><tr><td>Apply to pattern range</td><td><code>'/start/,/end/d'</code></td><td>Deletes lines from first “start” match to first “end” match</td></tr><tr><td>Negate address</td><td><code>'/pattern/!d'</code></td><td>Deletes all lines not matching “pattern”</td></tr></tbody></table>

### Useful Operations
1. **Print Matching Lines**: Print lines containing the word "linux".
```bash
sed -n '/linux/p' filename.txt\
```
2. **Stream Input with Pipes**: When processing data from other commands or streams, use pipes to avoid intermediate file creation and reduce disk I/O.
```bash
cat largefile.txt | sed 's/foo/bar/' > output.txt
```
3. **Simiplify Scipts**: Minimize the number of operations in a single command. combine multiple scripts into a single script to reduce file reads.
```bash
sed -e 's/foo/bar/' -e '/pattern/d' largefile.txt
```
4. **Avoid In-Place Editing on Large Files**: Instead of directly modifying large files, write output to a new file and replace the original after verifying correctness.
```bash
sed 's/old/new/' largefile.txt > temp.txt && mv temp.txt largefile.txt
```
5. **Replace TABS with SPACES**: `sed 's/\t/    /g' file1.txt`
6. **Insert Text Before a Line**:  Inserts “This is inserted text.” before the second line in file1.txt.
```bash
sed -i '2i\This is inserted text.' file1.txt
```
7. **Print Line Numbers**: `sed '=' file.txt`

---

<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>sed 's/old/new/' file</code></td><td>Replace first match on each line</td></tr><tr><td><code>sed 's/old/new/g' file</code></td><td>Replace all matches</td></tr><tr><td><code>sed 's/old/new/2' file</code></td><td>Replace second match</td></tr><tr><td><code>sed 's/old/new/Ig' file</code></td><td>Case-insensitive replace (GNU sed)</td></tr><tr><td><code>sed -n 's/old/new/p' file</code></td><td>Print only lines with replacements</td></tr><tr><td><code>sed 's/old/new/w out.txt' file</code></td><td>Write changed lines to file</td></tr></tbody></table>

<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>sed -n 'p' file</code></td><td>Print all lines (same as <code>cat</code>)</td></tr><tr><td><code>sed -n '3p' file</code></td><td>Print line 3 only</td></tr><tr><td><code>sed -n '/pattern/p' file</code></td><td>Print matching lines</td></tr><tr><td><code>sed -n '1,5p' file</code></td><td>Print range of lines</td></tr><tr><td><code>sed 'd' file</code></td><td>Delete all lines (prints nothing)</td></tr><tr><td><code>sed '3d' file</code></td><td>Delete line 3</td></tr><tr><td><code>sed '/pattern/d' file</code></td><td>Delete matching lines</td></tr></tbody></table>

<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>sed '2i\\new line' file</code></td><td>Insert before line 2</td></tr><tr><td><code>sed '2a\\new line' file</code></td><td>Append after line 2</td></tr><tr><td><code>sed '2c\\new line' file</code></td><td>Replace line 2</td></tr><tr><td><code>sed '/pattern/i\\new line' file</code></td><td>Insert before matches</td></tr><tr><td><code>sed '/pattern/a\\new line' file</code></td><td>Append after matches</td></tr></tbody></table>

<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>sed -n '1p' file</code></td><td>First line</td></tr><tr><td><code>sed -n '$p' file</code></td><td>Last line</td></tr><tr><td><code>sed -n '1~2p' file</code></td><td>Every 2nd line (GNU sed)</td></tr><tr><td><code>sed -n '/error/,+2p' file</code></td><td>Match plus next 2 lines</td></tr><tr><td><code>sed -n '/start/,/end/p' file</code></td><td>Print between patterns</td></tr></tbody></table>

<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>sed 's/[[:space:]]\+$//' file</code></td><td>Trim trailing whitespace</td></tr><tr><td><code>sed 's/^[[:space:]]\+//' file</code></td><td>Trim leading whitespace</td></tr><tr><td><code>sed 's/[[:space:]]\+/ /g' file</code></td><td>Collapse whitespace</td></tr><tr><td><code>sed '/^#/d;/^$/d' file</code></td><td>Remove comments and blank lines</td></tr><tr><td><code>sed -n 'n;p' file</code></td><td>Print even lines</td></tr></tbody></table>

---
### Handling Special Characters in Patterns 

**While handling special characters in the search patterns, use `\` escape character, But for characters like `'`, use double quotes `""` on the outside.** 

- Always use double quotes for your sed command, and escape only the backslash needed by sed.
- 
```bash
sed "/your pattern/i\\your text" file

sed "/Clay's Quilt/i\\Hello World" book.txt
```
- Double quotes `"..."` → safely allow `'` (no need to escape)
- sed needs `\` for commands like `i\`
- Inside `"..."`, you write `\\` so that sed finally sees `\`

- `i\` is enough inside single quotes
- `i\\` needed inside double quotes (because \ gets consumed by shell)

---
### Capture Groups

Sed capture groups allow you to grab part of a match and reuse it later in the editing command.

- Easily swap, reorder, or reverse text
- Extract specific data like log file timestamps
- Reformat text by rearranging the position of tags

```bash
sed -E 's/(pattern1) (pattern2)/\2 \1/'

# (pattern1) -> group 1
# (pattern2) -> group 2
```
* `-E` enables extended regex (no need to escape parentheses).

Sed capture groups are defined using parentheses – `()`. Any text matched inside parentheses is captured.

```bash
sed ‘s/\(Linux\) \(world\)/\2 \1/‘
```
This matches and captures "Linux" into group 1. The `\1` in the replacement text refers to the first capture group, inserting "hello" again.
* We can use Up to `9` capture groups are supported in standard sed.

```ini
\1, \2, \3 ../9
```