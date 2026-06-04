# Command line Interface - 101

## Basic Navigation

```bash
pwd                       # Show present working directory
ls                        # List files and directories
ls -l                     # List files with detailed information
ls -a                     # List all files including hidden files
ls -lh                    # List files in human readable format
cd                        # Change directory
cd /                      # Go to root directory
cd ~                      # Go to home directory
cd ..                     # Go to parent directory
cd -                      # Go to previous directory
.                         # Represents current directory
..                        # Represents previous directory
```

## History and Shortcuts

```bash
history                   # Show command history
!!                        # Run the previous command
!5                        # Run command number 5 from history
!ls                       # Run the last ls command
clear                     # Clear terminal screen
reset                     # Reset terminal display
```

## Auto Completion

```bash
Tab                       # Auto-complete command or path
Tab Tab                   # Show all possible completions
```

## Multiple Commands & Operators

```bash
command1; command2        # Run commands sequentially
command1 && command2      # Run second command if first succeeds
command1 || command2      # Run second command if first fails
```

## Output Redirection

```bash
command > file            # Redirect output to file (overwrite)
command >> file           # Redirect output to file (append)
command < file            # Take input from file
```

## Pipes

```bash
command1 | command2       # Send output of one command to another

ls | wc -l
```

## Exit & Session Commands 

```bash
exit                      # Exit the shell session
logout                    # Logout from the shell
```