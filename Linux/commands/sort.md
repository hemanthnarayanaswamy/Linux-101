# The `sort` command

the `sort` command is used to sort a file, arranging the records in a particular order. By default, the sort command sorts a file assuming the contents are ASCII. Using options in the sort command can also be used to sort numerically. 

## Common Options for sort:
* **Alphabetical Sorting:** Run `sort filename.txt` to sort lines alphabetically.
* **Numerical Sorting:** Use `sort -n filename.txt` to sort lines numerically.
* **Reverse Order:** Use `sort -r filename.txt` to sort lines in descending order.
* ***Unique Lines:*** Use `sort -u filename.txt` to remove duplicate lines.
* ***Ignore Case:*** Use `sort -f filename.txt` to ignore case while sorting.
* ***Sort by Month:*** Use `sort -M filename.txt` to sort lines by month names.