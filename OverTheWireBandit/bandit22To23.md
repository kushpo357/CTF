log into bandit 22 using the 22nd password

---

Another cron-based level, but this one adds a twist: the script computes its target filename dynamically based on who runs it.

check the cron job
Code: ``cat /etc/cron.d/cronjob_bandit23``
Output:
*@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null*

Runs as **bandit23**, every minute.

---

check what the script does
Code: ``cat /usr/bin/cronjob_bandit23.sh``
Output:
*#!/bin/bash
myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
cat /etc/bandit_pass/$myname > /tmp/$mytarget*

Unlike bandit22's cron job, this one doesn't use a hardcoded filename. It builds the target filename by:
1. Getting the current user (`whoami`)
2. MD5-hashing the string `"I am user <username>"`
3. Writing that user's password to `/tmp/<hash>`

---

first attempt (wrong): ran the script directly as bandit22
Code: ``sh /usr/bin/cronjob_bandit23.sh``
Output:
*Copying passwordfile /etc/bandit_pass/bandit22 to /tmp/8169b67bd894ddbb4412f91573b38db3*

This only leaks **bandit22's own password** back, since running the script manually executes it as whoever ran it (bandit22), not as bandit23. `whoami` inside the script resolves to the actual calling user — the cron config only controls what user *cron itself* runs it as when *cron* triggers it.

---

correct approach: don't execute the script — just replicate its logic locally, substituting bandit23 as the target username

Code: ``myname=bandit23 && mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1) && echo $mytarget``
Output:
*8ca319486bfbbc3663ea0fbe81326349*

**Note:** initial attempt used `myname=$(bandit23)` — the `$()` syntax tells bash to execute `bandit23` as a command and capture its output, not assign the literal string. Correct form is a plain assignment: `myname=bandit23`.

This reproduces exactly what the filename *would* be when cron runs the script as bandit23 — without needing to actually become bandit23 or execute anything as them.

---

wait ~60 seconds for the cron job to fire as bandit23, then read the resulting file

Code: ``cat /tmp/8ca319486bfbbc3663ea0fbe81326349``
Output:
*<23rd password>*

save the 23rd password
