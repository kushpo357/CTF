# Bandit Level 32 → Level 33

This level introduces a new twist: instead of a normal shell, logging in drops you into a custom wrapper — **"WELCOME TO THE UPPERCASE SHELL"** — that uppercases every command before executing it.

```
WELCOME TO THE UPPERCASE SHELL
>> ls
sh: 1: LS: Permission denied
>> LS
sh: 1: LS: Permission denied
```

The first instinct was to try common commands directly — `ls`, `man sh`, `clear`, `cd`, `sh`, `chmod`, `cat`, `vi`, `nano` — but every single one failed the same way, since whatever gets typed is uppercased before the shell tries to run it, turning valid commands into nonexistent ones like `LS` or `CAT`.

**Sticking point:** the early attempts kept trying different command *names*, assuming some specific command might slip through. But since *every* letter typed gets uppercased regardless of which command it belongs to, no command name — however obscure — was ever going to survive. The fix had to avoid alphabetic characters entirely.

**The key insight is that shells have special variables and characters that don't rely on letters at all.** One of these is `$0`, a built-in variable every shell process has that holds the path/name of the currently running shell. Since `$0` is just a dollar sign and a digit, there's nothing for an uppercase filter to mangle.

```bash
$0
```

This dropped straight into a normal, unrestricted shell — the uppercase wrapper was bypassed entirely.

From there, standard recon picked up:

```bash
ls
file uppershell
```

```
uppershell: setuid ELF 32-bit LSB executable, Intel i386, version 1 (SYSV), dynamically linked, ...
```

The wrapper binary itself is **setuid**, the same privilege concept from Bandit 26→27 — meaning it runs with the privileges of its file owner rather than the invoking user, regardless of who calls it. That's what made escaping into `$0` land in a shell with the right permissions to read the next password directly:

```bash
cat /etc/bandit_pass/bandit33
```

Password: `<bandit33_password> (will differ per player)`.

**Key technique:** when a restricted shell filters or mangles your input (uppercasing, blacklisting characters, etc.), don't just cycle through command names — reach for non-alphabetic shell built-ins and special variables (like `$0`, which holds the current shell's path) that can slip past character-based filters and drop you into an unrestricted shell.
