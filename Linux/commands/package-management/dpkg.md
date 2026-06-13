# `dpkg`

`dpkg` is the low-level Debian package tool. `apt` uses it underneath.

Use `apt` for normal installs because it handles repositories and dependencies better.

## Common Commands

```bash
dpkg -l
dpkg -l | grep nginx
dpkg -L nginx
dpkg -S /usr/bin/curl
sudo dpkg -i package.deb
sudo dpkg -r package-name
sudo dpkg --configure -a
```

| Command | Meaning |
|:--------|:--------|
| `dpkg -l` | List installed packages |
| `dpkg -L package` | List files installed by package |
| `dpkg -S path` | Find package owning a file |
| `dpkg -i file.deb` | Install local `.deb` |
| `dpkg -r package` | Remove package |
| `dpkg --configure -a` | Finish interrupted package configuration |
