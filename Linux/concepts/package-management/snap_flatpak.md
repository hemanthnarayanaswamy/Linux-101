# Snap and Flatpak

Snap and Flatpak are alternative package systems for Linux apps.

They bundle more dependencies than native distro packages and can run across multiple Linux distributions.

## Snap

Snap is common on Ubuntu and uses `snapd`.

```bash
sudo snap install app
snap list
sudo snap refresh
```

Snaps often auto-update and may be sandboxed.

## Flatpak

Flatpak is popular for desktop GUI applications and often uses Flathub.

```bash
flatpak install flathub app.id
flatpak run app.id
flatpak update
```

Flatpak uses runtimes and portals for sandboxed desktop integration.

## When To Use

Good fit:

```text
desktop GUI apps
vendor-published apps
newer app versions than distro repos
```

Avoid for:

```text
kernel
drivers
system services
low-level libraries
core admin tools
strict production servers
```

For system packages, prefer `apt`, `dnf`, or the distro package manager.
