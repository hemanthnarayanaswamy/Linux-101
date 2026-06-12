# Linux Configuration Files

Linux configuration is mostly file-based. Services, shells, package managers, users, and system behavior are controlled by readable text files.

## Common Locations

| Location | Purpose |
|:---------|:--------|
| `/etc/` | System-wide configuration |
| `~/.config/` | User-specific application configuration |
| `~/.bashrc` | Interactive non-login Bash config |
| `~/.profile` | Login shell/environment config |
| `/etc/apt/` | APT package manager configuration |
| `/etc/systemd/` | systemd unit and manager configuration |

## System vs User Configuration

System config affects the whole machine:

```text
/etc/ssh/sshd_config
/etc/sudoers
/etc/apt/sources.list
```

User config affects one account:

```text
~/.bashrc
~/.vimrc
~/.tmux.conf
```

## Safe Editing Habits

Before changing important config:

```bash
sudo cp file file.bak
```

Then edit with the correct tool:

```bash
sudo visudo
sudoedit /etc/hosts
nano ~/.bashrc
vim ~/.vimrc
```

After editing, verify syntax or behavior before logging out.

Examples:

```bash
sudo nginx -t
sudo sshd -t
source ~/.bashrc
```

## SRE/DevOps Rule

Configuration changes should be:

```text
documented
reviewable
reversible
tested
```

That is why teams later move from manual edits to dotfiles, Ansible, Terraform, CI/CD, and GitOps.
