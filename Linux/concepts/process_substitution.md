# Process Substitution 
Process substitution is an advanced feature in Bash that allows you to use the output of a command as if it were a file. 

This can be particularly useful when you need to compare the output of two commands or redirect the output of one command to another command.

**Whenever you think you need a temporary file to do something, consider if process substitution can be used.**

## Syntax and Usage

There are two main forms of process substitution:

1. **Reading from a command**: `<(command)`
2. **Writing to a command**: `>(command)`

## Reading from a Command
The `<(command)` syntax creates a named pipe (FIFO) and connects the output of the command inside the brackets to it. This allows you to use the output as input for another command.

```bash
# Compare the output of ls in two directories
diff <(ls dir1) <(ls dir2)
```

## Writing to a Command
The `>(command) `syntax also creates a named pipe but connects it to the input of the command inside the brackets. This allows you to redirect the output of one command to the input of another command. 

> To be honest, `>(...)` operator is **less common**. You'll find that `<(...)`can be used more often.