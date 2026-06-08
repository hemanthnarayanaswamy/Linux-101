# HERE STRING `<<<`

* A *here string* in Linux is a concise way to pass a string directly as standard input to a command, using the `<<<` operator.

```bash
command <<< "string"
command <<< $VAR
# This sends "string/var" to the command’s stdin.
```
1. **Passing a String to a Command**
```bash
wc -w <<< "Welcome to Linux Naruto"
# This counts the words in the string and output 3. 
```
2. **Using Variables**
```bash
msg="Hello $user"
cat <<< "$msg"
# Here the variable is expanded and passed to cat, which prints it
```
3. **Pattern Matching and Parameters**
```bash
description="my name is naruto and I am a anime character from leaf village and play the role as nanja"

if grep -q "ninja" <<< "$description"; then
        echo "Description contains 'ninja'"
fi
# This check if the variable contains a substring
```
4. **Multi Line Input**
```bash
cat <<< "Line 1
Line 2
Line 3"
# Each line is passed as part of the same here string
```
---
# HERE DOCUMENT `<<EOF`

- A **Here Document (Heredoc)** in Bash is a type of redirection that allows you to pass multiple lines of input to a command. 
- It is particularly useful when you need to pass a multiline block of text or code to an interactive command, such as `cat, tee, or sftp., ssh`

```ini
[COMMAND] <<[-] 'DELIMITER'
 Line 1
 data
 Line 2
DELIMITER

# COMMAND: This optional and can be any command that accepts redirection
# <<: The redirection operator
# DEMILITER: A string tha tmarks the end of the heredoc. Common delimiter are EOF, END
# -: Adding a dash suppresses leading tab characters, allowing for indentation
```

1. **Sample Example**
```bash
ssh -T baeldung@example.com "touch log1.txt" # Instead of this 
ssh -T baeldung@example.com "touch log2.txt"

ssh -T baeldung@host.com << EOF
touch log1.txt
touch log2.txt
EOF
```
2. **TAB Supression**: We add indentations to our heredoc with tabs so that it is much easier to read. 
- But rarely do we want the **tabs** to be part of the output. To remove the tabed indentations in the output. we need to use the `-` symbol.
```bash
cat <<-EOF
    This message is indented
        This message is double indented
EOF

# ouput: NO TABS SPACE
#This message is indented
#This message is double indented
```
*white spaces will not be suppressed even with the dash prefix.*

3. **Parameter Substitution & Command Substitution**
```bash
cat <<EOF
Hello ${USER}
EOF

cat <<EOF
Hello! It is currently: $(date)
EOF
```
4. **Passing Arguments to Functions**

```ini
./script.sh <<EOF
> arg1
> arg2
> arg3
> EOF
```
5. **Using the Calculator**
```bash
bc <<EOF
>10 + 20
>5 * 3
>EOF

# 30
# 15
```
6. **Directly Writing SQL Query**
```bash
mysql -u user -p db <<EOF
SELECT * FROM users;
EOF
```
7. **Piping Output**: You can pipe heredoc output into another command. 
```bash
cat <<EOF | grep hello
hello world
bye world
EOF

# heredoc → cat → grep
# we can use multiple pipes as well
```
8. **Redirecting Output**
* Write/append to a file directly or redirect some output to a file. 
```bash
cat <<EOF >> .env
DB_HOST=localhost
PORT=8080
DEBUG=true
EOF
```
---