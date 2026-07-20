# SELinux Commands

SELinux is RHEL/Fedora's **MAC** system: access is controlled by **contexts (labels)** on every file and process, not file paths. See [DAC vs MAC](../../concepts/security/dac_vs_mac.md).

## Mode

```bash
getenforce                   # Enforcing | Permissive | Disabled
sudo setenforce 0            # → Permissive (temporary, until reboot)
sudo setenforce 1            # → Enforcing
sestatus                     # detailed status
```

Persistent mode: `/etc/selinux/config` (`SELINUX=enforcing`).

| Mode | Meaning |
|:-----|:--------|
| **Enforcing** | Policy applied; violations **blocked** and logged |
| **Permissive** | Violations **logged but allowed** — for debugging/policy authoring |
| **Disabled** | SELinux off entirely |

## Contexts (Labels)

Every file/process carries `user:role:type:level`. The **type** is what most policy keys on.

```bash
ls -Z /var/www/html          # file contexts (e.g. httpd_sys_content_t)
ps -Z                        # process contexts
id -Z                        # your context
```

> This is why `chmod 777` may not fix an SELinux denial: if a file has the wrong **type** for the process, the mode bits are irrelevant — the policy still blocks it. Fix the label, not the mode:

```bash
sudo restorecon -Rv /var/www/html        # reset to the default policy context
sudo chcon -t httpd_sys_content_t file    # set a type (temporary)
sudo semanage fcontext -a -t httpd_sys_content_t "/srv/web(/.*)?"  # permanent rule
```

## Diagnosing Denials

```bash
sudo ausearch -m avc -ts recent           # recent access-vector-cache denials
sudo audit2allow -a                        # summarize denials as a candidate policy
sudo audit2allow -a -M mypol && semodule -i mypol.pp   # build & load a module
```

Workflow: set Permissive → run the service → collect denials → `audit2allow` → apply → back to Enforcing.

## Booleans

Toggle common policy switches without writing rules:

```bash
getsebool -a | grep httpd
sudo setsebool -P httpd_can_network_connect on
```

## Related

- Concept: [DAC vs MAC](../../concepts/security/dac_vs_mac.md)
- [AppArmor](./apparmor.md) (the Ubuntu equivalent), [`auditd`](./auditd.md)
