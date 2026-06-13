# [awk](https://quickref.me/awk.html)
* It a very complex command but we'll mainly use it for getting columns and using seperators. 
* Best for processing `csv` files and similar column seperated orinated data.

```bash
awk [options] 'pattern {action}' input-file`

# pattern: Specifies the condition to match
# action: Defines what to do when the pattern matches. 
# options: Includes flags like `-F` to set a custom field separator or `-f` to read an AwK program from a file
```
* AWK splits each line into fields (columns) using whitespace by default. `$1, $2 etc` represent the fields. 

### Built-in Variables

<table>
 <tr>
   <th>Variable</th>
   <th>Meaning</th>
 </tr>
 <tr>
   <td>$0</td>
   <td>Entire Line</td>
 </tr>
 <tr>
   <td>$1, $2, ..</td>
   <td>Respective Columns</td>
 </tr>
 <tr>
   <td>NF</td>
   <td>Number of Fields</td>
 </tr>
 <tr>
   <td>NR</td>
   <td>Line Number</td>
 </tr>
 <tr>
   <td>NRF</td>
   <td>Line Number per file</td>
 </tr>
 <tr>
   <td>FS</td>
   <td>Input Separator (default: space)</td>
 </tr>
 <tr>
   <td>OFS</td>
   <td>Output Separator</td>
 </tr>
</table>

---

## Printing Specific Columns

- Print the first column from a file: `awk '{ print $1 }' file.txt`
- Print the first and third columns:`awk '{ print $1, $3 }' file.txt`
- Print the last field of every line, regardless of how many fields there are: `awk '{ print $NF }' file.txt`
- Print the second-to-last field: `awk '{ print $(NF-1) }' file.txt`
- Print the total number of fields on each line: `awk '{ print NF }' file.txt`
- Print line number, first col, and number of fields `awk '{print NR,$1,NF}' sample-simple.csv`

> To sepeate the output columns by space using the seperator `,`

## Custom Field Separator

By default, awk splits on whitespace. You can change this with `-F`.
- Or you can use `sed` to change the seperator with space. 

```bash
sed 's/seperator/new_seperator/g' > new_file.txt
```

```bash
awk -F: '{ print $1, $7 }' /etc/passwd

awk -F',' '{ print $1, $3 }' data.csv
```
## AWK Expressions

<table>
<tbody>
<tr>
<td><code>$1 == "root"</code></td>
<td>First field equals root</td>
</tr>
<tr>
<td><code>{print $(NF-1)}</code></td>
<td>Second last field</td>
</tr>
<tr>
<td><code>NR!=1{print $0}</code></td>
<td>From 2th record</td>
</tr>
<tr>
<td><code>NR &gt; 3</code></td>
<td>From 4th record</td>
</tr>
<tr>
<td><code>NR == 1</code></td>
<td>First record</td>
</tr>
<tr>
<td><code>END{print NR}</code></td>
<td>Total records</td>
</tr>
<tr>
<td><code>BEGIN{print OFMT}</code></td>
<td>Output format</td>
</tr>
<tr>
<td><code>{print NR, $0}</code></td>
<td>Line number</td>
</tr>
<tr>
<td><code>{print NR "	" $0}</code></td>
<td>Line number (tab)</td>
</tr>
<tr>
<td><code>{$1 = NR; print}</code></td>
<td>Replace 1th field with line number</td>
</tr>
<tr>
<td><code>$NF &gt; 4</code></td>
<td>Last field &gt; 4</td>
</tr>
<tr>
<td><code>NR % 2 == 0</code></td>
<td>Even records</td>
</tr>
<tr>
<td><code>NR==10, NR==20</code></td>
<td>Records 10 to 20</td>
</tr>
<tr>
<td><code>BEGIN{print ARGC}</code></td>
<td>Total arguments</td>
</tr>
<tr>
<td><code>ORS=NR%5?",":"\n"</code></td>
<td>Concatenate records</td>
</tr>
</tbody>
</table>

---
## Examples

#### 1. Search Lines with a Keyword

```bash
awk '/manager/ {print}' employee.txt
```
#### 2. Print Specific Lines

`NR` is used to print specific lines (from line 3 to line 6, inclusive) of a file named employee.txt, along with their corresponding line numbers.

```bash
$ awk 'NR==3, NR==6 {print NR,$0}' employee.txt 
```
#### 3. Add Seperator to Output

```bash
$ awk '{print NR "- " $1 }' geeksforgeeks.txt
```