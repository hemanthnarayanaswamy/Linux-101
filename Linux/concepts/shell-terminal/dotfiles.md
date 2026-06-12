# Dotfiles

Dotfiles are hidden configuration files in a user's home directory.

They start with a dot:

```text
~/.bashrc
~/.profile
~/.vimrc
~/.tmux.conf
~/.gitconfig
```

## Why They Matter

Dotfiles define your personal shell and tool environment:

```text
aliases
functions
PATH changes
prompt settings
editor settings
tmux settings
git identity
```

## Common Pattern

Many engineers keep dotfiles in a Git repo:

```text
~/dotfiles/
```

Then they symlink files into the home directory:

```bash
ln -s ~/dotfiles/.bashrc ~/.bashrc
ln -s ~/dotfiles/.vimrc ~/.vimrc
ln -s ~/dotfiles/.tmux.conf ~/.tmux.conf
```

## SRE/DevOps Use

Dotfiles make a new machine feel familiar quickly.

Keep them:

```text
version controlled
portable
documented
safe to re-run
```

Do not put secrets directly into public dotfiles.
