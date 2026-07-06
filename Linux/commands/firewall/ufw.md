# `ufw` Essential Commands

UFW `(uncomplicated firewall)` is a command-line tool designed to simplify firewall management on Linux systems, particularly those based on Ubuntu. Built on top of `iptables`, it provides a user-friendly way to define rules for controlling network traffic, such as allowing or blocking specific ports, IP addresses, or services.

> Refer for the document for detail overview. [UFW Reference Guide](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)

### Key Takeaways

- **UFW Simplifies Firewall Management**: UFW is a user-friendly interface for managing iptables, designed to simplify firewall configuration on Ubuntu-based systems.

- **Default Policies Are Secure by Design**: By default, UFW denies all incoming connections and allows all outgoing connections, creating a secure baseline for most servers.

- **Always Allow SSH Before Enabling UFW**: If you’re connected via SSH, enable SSH access with `sudo ufw allow OpenSSH` before activating UFW to avoid losing remote access.

- **Use Application Profiles When Available**: UFW integrates with application profiles (e.g., Nginx Full, OpenSSH), allowing easier rule creation without specifying port numbers manually.

- **Support for IP-Based Rules**: You can allow or block traffic from specific IP addresses or subnets using simple commands like `ufw allow from IP` or `ufw deny from subnet`.

- **Interface-Specific Rules Offer Granular Control**: UFW allows rule targeting per network interface, which is useful for multi-interface systems and virtualized environments.

- **UFW Integrates with Both IPv4 and IPv6**: Rules apply to both IP versions unless explicitly disabled. You’ll see (v6) entries in the status output for IPv6 rules.

- **Docker Can Conflict with UFW**: Docker modifies `iptables` directly, potentially bypassing UFW rules unless additional configuration is applied.

- **UFW Rules Can Be Reset or Deleted Easily**: Use ufw reset to wipe all rules or ufw delete to remove specific ones, including by rule number for precision.

## Usage `ufw`

1. Check the status of firewall `ufw`, The output will indicate if your firewall is active or not.

```bash
sudo ufw status
# Status: inactive
```

> Note: By default, when enabled UFW will block external access to all ports on a server. In practice, that means if you are connected to a server via SSH and enable UFW before allowing access via the SSH port, you’ll be disconnected. Make sure you follow the section in this guide on how to enable SSH access before enabling the firewall if that’s your case.

2. Enable/Disable the firewall

```bash
sudo ufw enable
# Firewall is active and enabled on system startup

sudo ufw disable
# command will fully disable the firewall service on your system.
```

3. `ufw` default policies
   When you enable UFW for the first time, it applies a set of default policies that define how the firewall handles incoming and outgoing connections. By default, UFW is configured to:

- Deny all incoming connections
- Allow all outgoing connections

```bash
sudo ufw status verbose

# Status: active
# Logging: on (low)
# Default: deny (incoming), allow (outgoing), deny (routed)
# New profiles: skip
```

If you want to change the default behavior, you can update the default policies using the following commands:

```bash
# To deny all incoming connections (recommended for most servers):
sudo ufw default deny incoming

# To allow all outgoing connections (the default):
sudo ufw default allow outgoing

# To deny all forwarded traffic (relevant for outers and gateways)
sudo ufw default deny routed
```

### How UFW Integrates with `iptables`

UFW is a user-friendly front-end for managing iptables, the underlying packet filtering framework used by most Linux systems. When you configure firewall rules using UFW, it translates those rules into iptables syntax behind the scenes.

_UFW_ stores its persistent rule definitions in configuration files, under: `/etc/ufw/`

### How to Block/Allow an IP Address

To block all network connections that originate from a specific IP address, run the following command, replacing the highlighted IP address with the IP address that you want to block:

```bash
sudo ufw deny from 203.0.113.100

sudo ufw deny from 203.0.113.0/24

sudo ufw allow from 203.0.113.101

sudo ufw allow from 203.0.113.0/24.
```

### How to Block/Allow Connections to a Network Interface

```bash
sudo ufw deny in on eth0 from 203.0.113.100

sudo ufw allow in on eth0 from 203.0.113.102
```

The `in` parameter tells ufw to apply the rule only for incoming connections, and the `on eth0` parameter specifies that the rule applies only for the eth0 interface.

### How to Delete UFW Rule

To delete a rule that you previously set up within UFW, use `ufw delete` followed by the rule (allow or deny) and the target specification.

```bash
sudo ufw delete allow from 203.0.113.101
```

Another way to specify which rule you want to delete is by providing the `rule ID`.

```bash
sudo ufw status numbered

# Status: active

#      To                         Action      From
#      --                         ------      ----
# [ 1] Anywhere                   DENY IN     203.0.113.100
# [ 2] Anywhere on eth0           ALLOW IN    203.0.113.102

sudo ufw delete 1

# Deleting:
#  deny from 203.0.113.100
# Proceed with operation (y|n)? y
# Rule deleted
```
