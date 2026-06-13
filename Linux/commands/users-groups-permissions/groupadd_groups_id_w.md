# `groupadd`, `groups`, `id`, and `w`

These commands inspect users, groups, and logged-in sessions.

## `groupadd`

Creates a group.

```bash
sudo groupadd developers
```

## `groups`

Shows group membership.

```bash
groups
groups alice
```

## `id`

Shows UID, GID, and group IDs.

```bash
id
id alice
```

Typical output:

```text
uid=1000(alice) gid=1000(alice) groups=1000(alice),27(sudo)
```

## `w`

Shows who is logged in and what they are doing.

```bash
w
```

Use `who` for a simple login list and `w` for a more detailed activity view.
