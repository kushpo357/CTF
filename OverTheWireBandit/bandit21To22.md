log into bandit 21 using the 21st password

---

This level is about a cron job that runs as bandit22 and leaks its password into a predictable file.

check bandit21's own crontab first
Code: ``crontab -l``
Output:
*crontabs/bandit21/: fopen: Permission denied*

No access to bandit21's personal crontab — but system-wide cron jobs are configured elsewhere.

---

check system-wide cron jobs
Code: ``cd /etc/cron.d && ls``
Output:
*behemoth4_cleanup  cronjob_bandit22  cronjob_bandit24  leviathan5_cleanup    otw-tmp-dir
clean_tmp          cronjob_bandit23  e2scrub_all       manpage3_resetpw_job*

`cronjob_bandit22` stands out as directly relevant.

Code: ``cat cronjob_bandit22``
Output:
*@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null*

This job runs `/usr/bin/cronjob_bandit22.sh` as **bandit22**, every minute (and once at reboot). Since it runs as bandit22, whatever it does happens with bandit22's privileges — that's our way in.

---

check what the script actually does
Code: ``cat /usr/bin/cronjob_bandit22.sh``
Output:
*#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv*

The script:
1. Makes a specific file in `/tmp` world-readable (`644`)
2. Dumps bandit22's password into that same file

The filename is hardcoded and predictable — no need to brute force or guess it.

---

try to list /tmp directly
Code: ``ls /tmp``
Output:
*ls: cannot open directory '.': Permission denied*

Directory listing is blocked, but that doesn't matter — we already know the exact filename from the script, and the file itself is world-readable.

---

read the leaked password file directly
Code: ``cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv``
Output:
*<22nd password>*

Since the cron job runs as bandit22 and writes its own password to a world-readable file every minute, any user who can read the cron config (and thus the script path) can recover the password without needing execute access to anything privileged.

save the 22nd password
