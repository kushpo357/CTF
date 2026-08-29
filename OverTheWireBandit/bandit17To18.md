log into bandit 17 using the ssh key
Code: ``ssh bandit17@bandit.labs.overthewire.org -p 2220 -i bandit17sshkey``

**Note: if you are facing problem in logging in check the last part of bandit 16 tutorial**

---

Check the contents
Code: ``ls``
Output:
*passwords.new  passwords.old*

Find the Difference between the two file using `diff` command
Code: ``diff passwords.new passwords.old``
Output:
*42c42
< password 18
\---
\> Random text in bandit18*

Save the 18th password
