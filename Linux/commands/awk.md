## 4. `awk`
* It a very complex command but we'll mainly use it for getting columns and using seperators. 

```bash
awk [options] 'pattern {action}' input-file`

# pattern: Specifies the condition to match
# action: Defines what to do when the pattern matches. 
# options: Includes flags like `-F` to set a custom field separator or `-f` to read an AwK program from a file
```
* AWK splits each line into fields (columns) using whitespace by default. `$1, $2 etc` represent the fields. 

- `awk '{print}' file.txt`: Just prints the content of the file 
- `awk '/<word or pattern to search>/ {print}' file.txt`: Finds all the lines that matchs the pattern. 
- `awk '{print $1, $2}' file.txt`: Only prints column 1 and 2, columns are differenciated based on the spaces. `ll | awk '{print $1, $2}'`
- `awk -F: '{print $1}' /etc/passwd`: `-F` flag is used to mention custom seperator, In this case `:` is the seperator and columns will be based on the seperator instead of space. 