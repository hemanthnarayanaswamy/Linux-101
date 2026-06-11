# Package Management 

Package managers and systemctl are essential tools in Linux used to manage software and control system services. Together, they help users install applications, handle dependencies, and start, stop, or monitor services efficiently.

## Fundamental Concepts 
A package manager interacts with the user system, repositories, packages, metadata, and dependencies to install and manage software.

![arch](https://media.geeksforgeeks.org/wp-content/uploads/20251111114619233280/1.webp)

#### Package / Software Package
A package is a collection of files and metadata that represents a software application or a system component. or Pre-compiled applications bundled with metadata, including version infromation, dependencies, installation scripts and configuration files. 
* Packages are usually distributed in a compressed format, such as `.deb` for Debian-based systems or `.rpm` for Red Hat-based systems.

Each package contains:
* **Binary Files**: The actual compiled executable programs. 
* **Configuration Files**: Dafault settings and system integration files. 
* **Metadata**: Package information, dependencies, and version details. 
* **INstallation Scripts**: Pre and post installation commands. 
* **Documentation**: Man pages, README files, and usage guides. 

#### Repository
A repository is a collection of packages stored on a remote server. 
- Repositories are maintained by Linux distributions, software vendors, or third-party organizations. 
- They provide a centralized location for users to download and install packages. 
- *Package managers are configured to access these repositories and retrieve the necessary packages.*

#### Dependency
A dependency is a relationship between two or more packages. 
- A package may depend on other packages to function correctly. 
- For example, a graphical application may depend on a specific version of a graphics library. 
- Package managers are responsible for resolving these dependencies and ensuring that all required packages are installed before installing the target package.

Package managers automatically:
- Identify required dependencies
- Check if dependencies are already installed
- Download and install missing dependencies
- Resolve version conflicts
- Maintain dependency trees for safe removal

#### Package Manager
A package manager is a software tool that automates the process of managing packages on a Linux system. 
- It interacts with repositories, resolves dependencies, and performs tasks such as installing, upgrading, and removing packages. 
- Different Linux distributions use different package managers, but they all share the same basic functionality. `apt, yum, dnf, pkg etc..`

![package manager](https://codelucky.com/wp-content/uploads/2025/08/linux-package-management-mermaid-1.svg)

---
## Popular Package Manager

### 1. APT (Advanced Package Tool) - Debian/Ubuntu Systems
APT is the default package manager for Debian-based distributions, including Ubuntu, Linux Mint, and elementary OS.
- It uses `.deb` packages.
- Additional functionality of `apt` apart from the regular function is *APT can lock packages to specific versions, ensuring system stability.*
- APT maintains a **local cache** of package information. This cache helps in faster package searches and updates. Users can update this cache using the `apt update` command.

> `apt` command is used for interacting with `dpkg`(packaging system used by debian). There is already the `dpkg` command to manage `.deb` packages. But `apt` is a more user-friendly and efficient way.

```bash
sudo apt update              # Fetches the latest package information from repositories.
                             # Does not install or upgrade packages -- Just updates the local package index. 

sudo apt upgrade             # Installs the newest versions of all installed packages without removing anything.

sudo apt full-upgrade        # Upgrades packages and removes obsolete/conflicting packages if necessary.
                             # Useful after major OS upgrades.

sudo apt install <package name> # Installs a package and its dependencies

sudo apt install pkg1 pkg2 pkg3 # Install multiple packages at once

sudo apt install package=version # Install specific version of a package

sudo apt remove <package_name>   # Removes the package but keeps configuration files

sudo apt purge <package_name>    # Removes the package and its configuration files.

sudo apt autoremove           # Removes packages that were installed as dependencies but are no longer needed. 

sudo apt clean                # Deletes all cached .deb files in /var/cache/apt/archives

sudo apt autoclean            # Delectes only obsolete cached .deb files

sudo apt search <keyword>     # Search for packages matching the keyword

sudo apt list --installed     # Lists all installed packages 

sudo apt list --upgradable    # Lists packages that have available updates 

apt show <package_name>       # Displays detailed information about a package (description, dependencies, version, etc.).

sudo apt edit-sources         # Opens /etc/apt/sources.list in the default editor for editing repository sources.

sudo apt --fix-broken install  # Repairs broken dependencies.

apt download <package_name>    # Downloads .deb file without installing it

apt install --simulate <package_name> # Shows what would happen without actually installing.

sudo apt update && sudo apt upgrade -y    # -y auto-confirms prompts

sudo apt-mark hold nginx                # Hold a package at current version

sudo apt-mark unhold nginx              # Unhold a package

apt-mark showhold                       # List held packages
```

Refer to [APT Detailed Guide](../commands/apt.md) for advance usage. 

### 2. YUM (Yellowdog Updater Modified) – RHEL/CentOS Systems
YUM is the traditional package manager for Red Hat Enterprise Linux (RHEL), CentOS, and Fedora (older versions). It’s built on top of RPM (Red Hat Package Manager) and provides automatic dependency resolution.

![yum architecture](https://codelucky.com/wp-content/uploads/2025/08/linux-package-management-mermaid-3.svg)

### 3. DPKG (Debian Package Manager):
**DPKG** (Debian Package Manager) is a fundamental package management tool for Debian-based Linux distributions, including Debian itself, Ubuntu, and their derivatives.
- `apt` is more user friendly and efficient compared to `dpkg` 

```bash
sudo dpkg -i package.deb      # Installing a packag

sudo dpkg -r package-name     # Removing a package:

dpkg -l | grep package-name   # Querying package information:
```
---
# <center>Understand repos, sources, and PPAs</center>

APT does not magically know where software is. It reads **repos** definitions. there is not a single repository that contains all the packages. to have packages in different repositories.

```ini
repository -> source entry -> apt update -> local package index -> apt install
```

## Ubuntu's Default Repositories

Each Ubuntu version has its own official set of four repositories:
1. **Main** – Canonical-supported free and open-source software.
2. **Universe** – Community-maintained free and open-source software.
3. **Restricted** – Proprietary drivers for devices.
4. **Multiverse** – Software restricted by copyright or legal issues.

It reads repo definitions from this files: Then it downloads package lists from those repositories.
```bash
/etc/apt/sources.list
/etc/apt/sources.list.d/
```
> In APT language, a `source` is an entry that tells APT where to look for packages. a “source” is not the software itself. It is the address and category information for a repository.

1. `/etc/apt/sources.list` This is the old main file for APT sources.
```bash
cat sources.list

deb http://archive.ubuntu.com/ubuntu noble main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu noble-updates main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu noble-security main restricted universe multiverse
```
* Each line tells APT about one repo area.
* You usually keep official Ubuntu/Debian repos here, although newer systems may use `.sources` files instead.

2. `/etc/apt/sources.list.d/` This directory holds extra repo files.
```bash
/etc/apt/sources.list.d/docker.list
/etc/apt/sources.list.d/google-chrome.list
/etc/apt/sources.list.d/graphics-drivers-ubuntu-ppa-noble.list
/etc/apt/sources.list.d/ubuntu.sources
```
* The reason this directory exists is cleanliness. Instead of dumping every third-party repo into one big file, each repo gets its own file.

APT reads both:
```bash
official OS repos        -> sources.list or ubuntu.sources
third-party/vendor repos -> sources.list.d/
PPAs                     -> sources.list.d/

official OS repos        -> sources.list or ubuntu.sources
third-party/vendor repos -> sources.list.d/
PPAs                     -> sources.list.d/
```
### `.list` format breakdown

```bash
cat /etc/apt/sources.list.d/docker.list

deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu focal stable

# deb --> Binary packages. This is what you usually install
# signed-by=/etc/apt/keyrings/docker.gpg Use this specific GPG key to verify this reposity
# https://download.docker.com/linux/ubuntu Repository URL
# focal/noble/bookworm/jammy   Release codename.
# main/stable component/section inside the repo
```
### `.source` format breakdown

```bash
cat /etc/apt/sources.list.d/azure-cli.sources

Types: deb           # package types
URIs: https://packages.microsoft.com/repos/azure-cli/ # repo url
Suites: focal                                # release name
Components: main                          # component section inside repo
Architectures: amd64                     # system architecture
Signed-by: /etc/apt/keyrings/microsoft.gpg # gpg key of verification
```
### Exact work flow

```ini
sudo apt update
    |
    v
Read /etc/apt/sources.list
    |
    v
Read /etc/apt/sources.list.d/*.list
Read /etc/apt/sources.list.d/*.sources
    |
    v
Ignore comments, disabled entries, invalid files
    |
    v
Parse each active source:
    type, URL, suite, components, options
    |
    v
Build remote paths:
    dists/<suite>/InRelease
    dists/<suite>/Release
    dists/<suite>/<component>/binary-<arch>/Packages.xz
    |
    v
Download metadata
    |
    v
Verify signatures and checksums
    |
    v
Store indexes in /var/lib/apt/lists/
    |
    v
APT now knows available package versions

Example: deb http://archive.ubuntu.com/ubuntu noble main universe

1. Check:
   http://archive.ubuntu.com/ubuntu/dists/noble/InRelease

2. Read metadata for:
   noble

3. Download package lists for:
   main
   universe

4. For your CPU architecture, maybe:
   binary-amd64

5. Store results locally:
   /var/lib/apt/lists/
```
---
## Third-Party Repositories and PPAs

Official repositories don’t include every piece of software. To install tools like `Docker`, `VS Code`, or `Google Chrome`, you’ll need to add third-party repositories:
> Ubuntu PPAs `(Personal Package Archives)`: Community-maintained repos hosted on Launchpad. Add a PPA with:

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update

sudo add-apt-repository ppa:user/ppa-name  
sudo apt update  

# Remove PPA
sudo add-apt-repository --remove ppa:user/ppa-name
sudo apt update
```
* This usually creates a file inside `/etc/apt/sources.list.d/`
> A PPA is just another APT repository, but maintained by a person, team, or project instead of Ubuntu’s official archive.

### Compiling From Sources

Sometimes, software isn’t available as a package (e.g., bleeding-edge tools). In these cases, compile from source using the classic `configure-make-make install` workflow:

1. Download the source code (usually a `.tar.gz` file).
2. Extract it: `tar -xf source-code.tar.gz && cd source-code-directory`
3. Configure the build: `./configure` (add flags like `--prefix=/usr/local` to set install path).
4. Compile the code: `make`
5. Install: `sudo make install`

### Package Verification and Security
APT verifies repositories using signing keys.
To ensure packages haven’t been tampered with, verify their integrity using GPG signatures:

To add a third-party APT repo, you need two things:

1. A GPG public key
2. A source entry 

* The source entry tells APT where the repo is.
* The GPG key tells APT how to verify that the repo metadata really came from that repo. `/etc/apt/keyring/***.gpg`
* APT does not blindly trust package servers. A repository signs its metadata using its private key. Your machine stores the matching public key.
*  Put repo key in `/etc/apt/keyrings/`. Reference that exact key using `signed-by=`. Add repo to source file `/etc/apt/soruces.list.d/****.source`.

```bash
cat /etc/apt/sources.list.d/azure-cli.sources

Types: deb           
URIs: https://packages.microsoft.com/repos/azure-cli/ 
Suites: focal                                
Components: main                          
Architectures: amd64                     
Signed-by: /etc/apt/keyrings/microsoft.gpg # gpg key of verification placed in the /etc/keyrings
```

To add the 3rd party repo:
```bash
# download and store the repo key
curl -fsSL https://packages.example.com/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/examplesoft.gpg

# place the file in key-ring dir where apt can read it 
sudo chmod a+r /etc/apt/keyrings/examplesoft.gpg

# Put repo key in /etc/apt/keyrings/
# Reference that exact key using signed-by=
# Add repo to source file
echo "deb [signed-by=/etc/apt/keyrings/examplesoft.gpg] https://packages.example.com/ubuntu noble stable" | sudo tee /etc/apt/sources.list.d/examplesoft.list

# update the repo list and install
sudo apt update
sudo apt install examplesoft

# Removing A third party repo
sudo rm /etc/apt/sources.list.d/examplesoft.list
sudo rm /etc/apt/sources.list.d/examplesoft.sources

sudo apt update
```
---
## Advanced Package Management Techniques

### Creating Custom Packages
For organizations, creating custom packages ensures consistent deployments:

```bash
# Debian package creation structure
mkdir -p mypackage/DEBIAN
mkdir -p mypackage/usr/local/bin

# Create control file
cat > mypackage/DEBIAN/control << EOF
Package: myapp
Version: 1.0
Section: base
Priority: optional
Architecture: amd64
Maintainer: Your Name 
Description: Custom application package
EOF

# Build the package
dpkg-deb --build mypackage
```
---
## Troubleshooting Common Issues

#### Dependency Conflicts (“Dependency Hell”)
A package can’t install because dependencies are missing, outdated, or conflicting.

```bash
apt --fix-broken install 
# (Debian/Ubuntu) to repair broken dependencies.
```

#### Broken Packages 
A package fails to install/remove, leaving the system in an inconsistent state.

- Reconfigure the package: `sudo dpkg --configure -a`
- Force-remove a broken package: `sudo dpkg --remove --force-remove-reinstreq <package-name>`

#### Repository Errors
*“Repository not found”* or “404 error” when updating.

* Check for typos in repository URLs (Debian: `/etc/apt/sources.list`)
* Disable outdated repos: `sudo add-apt-repository --remove ppa:user/ppa-name` (Ubuntu) 

#### GPG Key Errors
“GPG signature verification failed” when installing packages.

* Import the missing key `.gpg` 
* delete the old key in the `/etc/apt/keyring/old.gpg` and replace it with new one `/etc/apt/keyring/new.gpg`
* And update the `signed-by:` field in the `.sourc/.list` of the reposity source with the new path fo the `.gpg` file path. 

---
## Universal Package Managers

Traditional Linux package management relied on distribution-specific formats—`DEBs` for Debian/Ubuntu, `RPMs` for Fedora/RHEL, and various others.
```bash
Ubuntu uses .deb with apt
Fedora uses .rpm with dnf
Arch uses pacman packages
```
**This Creates problems for app developers**
> One app may need different packages for Ubuntu, Debian, Fedora, Arch, etc.
> `Snap` and `Flatpak` try to solve this by giving apps a more universal packaging system. They bundle more dependencies and run apps that work across distributions.

Some examples 
```ini
VLC
Firefox
LibreOffice
OBS Studio
GIMP
Spotify
Discord
```
#### SNAP
A snap contains the app plus many of the libraries it needs. Instead of fully depending on your system’s installed libraries, the app carries a lot with it.

```bash
sudo apt install snapd

sudo ln -s /var/lib/snapd/snap /snap

snap # snap package is called 

snapd # snap uses a background service

snap list
sudo snap install firefox
sudo snap remove firefox
snap info firefox
sudo snap refresh

/snap/appname/

which firefox
/usr/bin/firefox   # APT app
/snap/bin/firefox  # Snap app
```

Avoid them for core system tools. Prefer `apt`, `dnf`, Because these need tight integration with the system. 
```ini
kernel
drivers
systemd
shells
compilers
Python system packages
network tools
Docker engine
desktop environment components
low-level libraries
```

1. Is it a core system package?   --->  Use `apt/dnf/pacman`.
2. Are you on Ubuntu and the vendor officially supports Snap> ---> `snap` may be fine.
3. Is it a CLI/dev/admin tool?   ---> Prefer native package manager, official vendor repo, or language-specific toolchain.
4. Do you need strict version control? ---> Avoid auto-updating Snap unless you configure refresh behavior.

---
### Reference
* https://www.thelinuxvault.net/linux-package-management/a-comprehensive-guide-to-linux-package-management/#4-advanced-package-management