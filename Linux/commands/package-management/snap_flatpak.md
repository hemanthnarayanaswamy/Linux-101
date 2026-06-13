# `snap` and `flatpak`

Snap and Flatpak are universal app packaging systems.

## Snap

```bash
snap list
snap info firefox
sudo snap install firefox
sudo snap remove firefox
sudo snap refresh
```

Snap is common on Ubuntu and uses `snapd`.

## Flatpak

```bash
flatpak list
flatpak install flathub org.videolan.VLC
flatpak run org.videolan.VLC
flatpak uninstall org.videolan.VLC
flatpak update
```

Flatpak is common for desktop GUI apps and often uses Flathub.

## When To Avoid

Prefer native packages for:

```text
kernel
drivers
system services
core CLI tools
compilers
low-level libraries
production server daemons
```

Use Snap/Flatpak mainly for desktop apps or vendor-supported applications where the tradeoff is acceptable.
