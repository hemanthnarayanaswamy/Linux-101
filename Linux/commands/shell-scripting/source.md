# `source`

`source` runs a file in the current shell process.

It is also available as the dot shorthand:

```bash
source ~/.bashrc
. ~/.bashrc
```

## Why It Matters

Running a script normally starts a child shell:

```bash
./script.sh
```

Sourcing runs it in the current shell:

```bash
source script.sh
```

That means exported variables, aliases, and functions can affect your current terminal.

Common use:

```bash
source ~/.bashrc
source ~/.profile
```

Use it after editing shell config files so changes apply immediately.
