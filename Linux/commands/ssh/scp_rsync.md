# `scp` and `rsync`

> STATUS: TODO — new stub. Roadmap Phase 3 / Week 18 topic: "`scp`, `rsync -av --progress` (and why rsync is usually better)."

## To Cover

- [ ] `scp` basic syntax: `scp file.txt user@host:/path/`, and recursive with `-r`.
- [ ] `rsync -av --progress` basic syntax and why it's generally preferred over `scp` (delta transfer, resumability, `--delete`).
- [ ] `rsync --delete` and `--dry-run` — preview a sync before doing it for real (Week 18 Exercise 5).
- [ ] Using `rsync` over SSH (`-e ssh` or implicit) vs. `rsync` daemon mode.
- [ ] When `scp` is still the simpler/right choice (one-off single-file copy).

## Quick Reference (fill in as you learn)

```bash
scp file.txt user@remote:/home/user/
scp -r ./dir user@remote:/home/user/dir

rsync -av --progress ./localdir/ user@remote:/remote/dir/
rsync -av --delete --dry-run ./localdir/ user@remote:/remote/dir/
rsync -av --delete ./localdir/ user@remote:/remote/dir/
```
