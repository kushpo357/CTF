## Bandit 27 → 28

Goal page pointed to a git repo to clone **from the local machine** (not the OverTheWire box):

```
ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo
```

Password for `bandit27-git` = same as `bandit27`'s password.

**Prerequisite:** `git` must be installed on your local machine (not the OverTheWire box) — check with `git --version` before starting.

First attempt failed — `git clone` defaulted to port 22 instead of the wargame's port 2220:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo
```

*Permission denied (publickey) — server explicitly warns it's not listening for game logins on port 22.*

**Fix: embed the custom port directly in the `ssh://` URL, right after the hostname (`host:port`), same pattern as any other URL with a non-default port:**

```bash
mktemp -d
cd <generated-tmp-dir>
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
cd repo
cat README
```

*The password to the next level is: `<bandit28_password>` (will differ per player)*

**Note:** worked from a `mktemp -d` temp directory rather than the home directory, per the wargame's write-access restrictions on home dirs.
