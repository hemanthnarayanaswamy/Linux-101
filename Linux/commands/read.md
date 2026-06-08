# `read` 
* The read command in Linux is a built-in command used to read a line of input from the standard input (stdin) or a file descriptor and store it in a variable. 
* This command is commonly used in shell scripts to capture user input or handle file operations.

```ini
read [options] variable_name
# When you use read without any options, it waits for the user to input a line of text and stores it in the specified variable. For example:
```

- `-p`: Displays a prompt before reading input.
- `-s`: Disables echoing the input (useful for password entry).
- `-n`: Limits the number of characters to read.
- `-t`: Sets a timeout to wait for input. 
- `-a`: **Reads the input into an array**.

```ini
# using the -p option, you can display a prompt message before reading the input
read -p "What is your desired username? " username
echo "Your username is $username"

# The -s options is useful for reading a sensitive information liek passwords without displaying the input on screen.
read -s -p "Enter your password: " password
echo "Password entered"

# The -n option allows you to limit the number of characters the user can input:
read -n 5 -p "Enter a 5-character code: " code
echo "Code entered: $code"

# The -t option sets a timeout for the input
# This script waits for 10 seconds for the user to input their name before timing out.
read -t 10 -p "Enter your name within 10 seconds: " name
echo "Hello, $name"

read -a senArr <<< "Hello my name is naruto"
echo "${senArr[@]}" # ("Hello" "my" "name" "is" "naruto")
# This script reads space-separated input into an array and then displays the elements.
```