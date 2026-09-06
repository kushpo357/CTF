## Bandit 28 → 29

Goal page pointed to a git repo to clone **from the local machine**:

```
ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
```

Password for `bandit28-git` = same as `bandit28`'s password.

```bash
mktemp -d
cd <generated-tmp-dir>
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
cd repo
cat README.md
```

README.md showed the password field **redacted/censored** rather than missing entirely — a strong hint that it existed in plaintext at some earlier commit before being scrubbed.

Checked commit history to find when the redaction happened:

```bash
git log -p
```

**Note:** reading the diff of the commit that redacted the password (going from plaintext -> `xxxxxxxxxx` or similar) revealed the original plaintext value in the `-` (removed) line of the diff.

*The password to the next level is: `<bandit29_password>` (will differ per player)*

**Key technique:** git history retains everything, even data later "fixed" or removed in a subsequent commit — always check `git log -p` (or `git log --all -p` if branches are involved) when a repo's current state looks intentionally scrubbed.
