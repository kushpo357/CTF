## Bandit 26 → 27

From the bandit26 shell, checked home directory:

```bash
ls -la
```

Found a setuid binary:

```bash
ls -l bandit27-do
```

*-rwsr-x--- 1 bandit27 bandit26 14880 Jun 24 14:59 bandit27-do*

**Setuid bit (`s`) means the binary runs with the *owner's* privileges (bandit27), not the caller's — regardless of who executes it.** Since bandit26 is in the owning group, it can be run directly:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

This executed `cat` as bandit27, printing the next password.

**Note:** general recon command for finding setuid binaries on a fresh box: `find / -perm -4000 2>/dev/null`.
