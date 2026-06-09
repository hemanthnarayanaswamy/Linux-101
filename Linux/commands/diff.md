# The `diff/sdiff` command
This command is used to display the differences in the files by comparing the files line by line.
- The `diff` command is a powerful tool for comparing files and directories, making it essential for debugging and version control tasks.

### Syntax:
```
diff [options] File1 File2 
```

### Key Symbols in Output:
- `<`: Line from the first file.
- `>`: Line from the second file.
- `c`: Change.
- `a`: Add.
- `d`: Delete.

[detail](https://www.geeksforgeeks.org/linux-unix/diff-command-linux-examples/)

### Example
Lets say we have two files with names **a.txt and b.txt** containing 5 Indian states as follows-:
```bash
$ cat a.txt
Gujarat
Uttar Pradesh
Kolkata
Bihar
Jammu and Kashmir

$ cat b.txt
Tamil Nadu
Gujarat
Andhra Pradesh
Bihar
Uttar pradesh
```
On typing the diff command we will get below output.

```bash
$ diff a.txt b.txt
0a1
> Tamil Nadu
2,3c3
< Uttar Pradesh
< Kolkata
---
> Andhra Pradesh
5c5
< Jammu and Kashmir
---
> Uttar pradesh
```

The first line of the diff output will contain:  
* Line numbers corresponding to the first file,
* A special symbol and
* Line numbers corresponding to the second file.

Like in our case, **0a1** which means after lines 0(at the very beginning of file) you have to add Tamil Nadu to match the second file line number 1. It then tells us what those lines are in each file preceded by the symbol
* Lines preceded by a `<` are lines from the first file.
* Lines preceded by `>` are lines from the second file.
* Next line contains **2,3c3** which means from line 2 to line 3 in the first file needs to be changed to match line number 3 in the second file. It then tells us those lines with the above symbols.
* The three dashes **(---)** merely separate the lines of file 1 and file 

*As a summary to make both the files identical, first add Tamil Nadu in the first file at very beginning to match line 1 of second file after that change line 2 and 3 of first file i.e. Uttar Pradesh and Kolkata with line 3 of second file i.e. Andhra Pradesh. After that change line 5 of first file i.e. Jammu and Kashmir with line 5 of second file i.e. Uttar Pradesh.*