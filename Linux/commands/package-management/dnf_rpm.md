# `dnf` and `rpm`

These are common on RHEL-family systems such as Fedora, Rocky Linux, AlmaLinux, and RHEL.

## `dnf`

`dnf` is the high-level package manager.

```bash
sudo dnf search nginx
sudo dnf install nginx
sudo dnf remove nginx
sudo dnf update
dnf info nginx
```

## `rpm`

`rpm` is the lower-level package tool.

```bash
rpm -qa
rpm -ql nginx
rpm -qf /usr/bin/curl
sudo rpm -i package.rpm
sudo rpm -e package-name
```

| Command | Meaning |
|:--------|:--------|
| `rpm -qa` | List installed RPM packages |
| `rpm -ql package` | List files from package |
| `rpm -qf path` | Find package owning a file |

On RHEL-family systems, prefer `dnf` for normal package management.
