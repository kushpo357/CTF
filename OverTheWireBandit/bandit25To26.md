## Bandit 25 → 26

Login shell for bandit26 wasn't bash — checked `/etc/passwd` from bandit25:

```bash
cat /etc/passwd | grep bandit26
```

*bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext*

Read the script directly since it was readable from bandit25:

```bash
cat /usr/bin/showtext
```

```sh
#!/bin/sh
export TERM=linux
exec more ~/text.txt
exit 0
```

**`exec` replaces the shell process with `more` — when `more` exits (e.g. because the file fits on one screen), the whole SSH session dies with it.** `text.txt` was only 258 bytes, so it displayed and closed instantly on a normal-size terminal.

Fix: shrink the terminal so `more` can't fit the file on one screen, forcing it to pause:

```bash
stty rows 5 cols 40
ssh -i sshkey25to26.private bandit26@bandit.labs.overthewire.org -p 2220
```

Once paused at `--More--`, `more`'s direct `!/bin/sh` shell-escape was unreliable to verify with such a small terminal — it kept bouncing back to `--More--` with no clear confirmation. Used `more`'s `v` command instead, which opens `vi` on the file — much more stable:

```
v
:set shell=/bin/bash
:shell
```

This dropped into a real interactive bandit26 shell.

**Note:** tiny terminals (e.g. `rows 1`) make command output unreadable — it flashes and scrolls away before you can read it. `rows 5` was a workable middle ground.
