log into bandit 13 using 14th password

---

We have a private key as a text file using which we have to access bandit14
Use this ssh key to log in to bandit14.
Code: ``ssh bandit14@bandit.labs.overthewire.org -p 2220 -i sshkey.private``
Now you will be in bandit 14

---

go to **/etc/bandit_pass/**
Code: ``cd ../../etc/bandit_pass``

Check the contents
Code: ``ls``
Output:
*bandit0   bandit12  bandit16  bandit2   bandit23  bandit27  bandit30  bandit4  bandit8
bandit1   bandit13  bandit17  bandit20  bandit24  bandit28  bandit31  bandit5  bandit9
bandit10  bandit14  bandit18  bandit21  bandit25  bandit29  bandit32  bandit6
bandit11  bandit15  bandit19  bandit22  bandit26  bandit3   bandit33  bandit7*

Open bandit14
Code: ``cat bandit14``
Output: *15th Password*

save the 15th password
