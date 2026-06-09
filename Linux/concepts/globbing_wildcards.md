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