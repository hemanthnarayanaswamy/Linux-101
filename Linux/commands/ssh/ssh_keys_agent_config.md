# SSH Keys, Agent, and Config

> STATUS: TODO — new stub. Roadmap Phase 3 / Week 18 topics, not yet covered by [5_ssh_remote_access.md](../../Phase2-Networking/5_ssh_remote_access.md) (currently just an intro stub) or [ssh_deepdive.md](../../concepts/ssh/ssh_deepdive.md) (currently empty).

## To Cover

- [ ] Key types: `ed25519` (preferred) vs RSA — why ed25519 (Week 18 self-check).
- [ ] `ssh-keygen -t ed25519` — generate a key pair.
- [ ] `ssh-copy-id` — copy a public key to a remote host for passwordless login (Week 18 Exercise 1).
- [ ] `ssh-agent` / `ssh-add` — avoid re-entering a passphrase every connection.
- [ ] `~/.ssh/config`: `Host`, `HostName`, `User`, `Port`, `IdentityFile` — alias a host so `ssh prod-web` just works (Week 18 Exercise 2).
- [ ] `ProxyJump` in `~/.ssh/config` vs. `ssh -J jump@bastion target@internal` on the command line (Week 18 Exercise 3).
- [ ] `authorized_keys` and `known_hosts` — what each file does, host key verification/fingerprint checking.
- [ ] Port forwarding: local (`-L`), remote (`-R`), dynamic/SOCKS (`-D`) — difference between `-L` and `-R` (Week 18 self-check, Exercise 4).
- [ ] Hardening: non-standard port, restricting source IP, disabling password auth — ties into [4_Firewalls.md](../../Phase2-Networking/4_Firewalls.md).

## Quick Reference (fill in as you learn)

```bash
ssh-keygen -t ed25519 -C "you@example.com"
ssh-copy-id user@remote

# ~/.ssh/config
Host prod-web
    HostName 10.0.0.5
    User deploy
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

Host internal
    HostName 10.0.1.20
    User ubuntu
    ProxyJump jump@bastion

ssh -L 8080:localhost:80 user@remote   # local forward
ssh -R 9000:localhost:3000 user@remote # remote forward
ssh -D 1080 user@remote                # dynamic SOCKS proxy
```
