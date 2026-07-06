# SSH Deep Dive

> STATUS: TODO — stub only. This is the concept-level companion to [ssh_keys_agent_config.md](../../commands/ssh/ssh_keys_agent_config.md) and [scp_rsync.md](../../commands/ssh/scp_rsync.md). Fill in as you work through Week 18 (Roadmap Phase 3). [5_ssh_remote_access.md](../../Phase2-Networking/5_ssh_remote_access.md) currently only covers the basic client/server model — the material below is what's still missing.

## To Cover

- [ ] Why SSH exists: encrypted remote shell over an untrusted network, replacing telnet/rsh.
- [ ] Asymmetric key exchange at a conceptual level: public/private key pair, why the private key never leaves your machine.
- [ ] Why `ed25519` is preferred over RSA (smaller keys, faster, modern curve — no known practical break).
- [ ] Host key verification: what `known_hosts` protects against (MITM on first connect), and what "the authenticity of host X can't be established" actually means.
- [ ] `authorized_keys` — how the server decides which public keys may log in as which user.
- [ ] `ssh-agent` — what problem it solves (unlock a key once per session instead of per connection) and how forwarding (`-A`) extends that to a jump chain.
- [ ] Port forwarding conceptually:
  - Local (`-L`): expose a *remote* service on your *local* port.
  - Remote (`-R`): expose a *local* service on the *remote* machine's port.
  - Dynamic (`-D`): turn SSH into a SOCKS proxy for arbitrary outbound traffic.
- [ ] `ProxyJump` / bastion architecture: why you'd never expose `internal` directly, and how one hop (`bastion`) becomes the single point of SSH entry.
- [ ] Hardening checklist: non-default port, key-only auth (`PasswordAuthentication no`), `AllowUsers`/source-IP restriction, fail2ban or rate limiting — ties into [4_Firewalls.md](../../Phase2-Networking/4_Firewalls.md).

## Self-Check (from the roadmap)

- Why prefer ed25519 over RSA?
- What's the difference between `-L` and `-R` port forwarding?
