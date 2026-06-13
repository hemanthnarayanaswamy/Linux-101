# APT Repositories, Sources, and PPAs

APT installs software from repositories.

```text
repository -> source entry -> apt update -> local package index -> apt install
```

## Repository

A repository is a server that stores packages and metadata.

Example:

```text
http://archive.ubuntu.com/ubuntu
```

## Source Entry

A source entry tells APT where to find packages.

Traditional `.list` example:

```text
deb http://archive.ubuntu.com/ubuntu noble main universe
```

Deb822 `.sources` example:

```text
Types: deb
URIs: http://archive.ubuntu.com/ubuntu
Suites: noble noble-updates
Components: main universe
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

APT reads:

```text
/etc/apt/sources.list
/etc/apt/sources.list.d/*.list
/etc/apt/sources.list.d/*.sources
```

## PPA

A PPA is a Personal Package Archive hosted on Launchpad.

```bash
sudo add-apt-repository ppa:user/ppa-name
sudo apt update
```

PPAs are third-party repositories. Use them only when you trust the maintainer.

## Modern Third-Party Repo Pattern

Use a dedicated keyring and `signed-by`:

```bash
sudo install -d -m 0755 /etc/apt/keyrings
curl -fsSL https://repo.example.com/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/example.gpg
echo "deb [signed-by=/etc/apt/keyrings/example.gpg] https://repo.example.com/ubuntu noble stable" | sudo tee /etc/apt/sources.list.d/example.list
sudo apt update
```

Avoid old `apt-key add` instructions.
