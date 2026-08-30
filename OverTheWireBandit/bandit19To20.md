log into bandit 10 using the 19th password

---

Print the bandit20 file in  `/etc/bandit_pass/`
Code: ``cat /etc/bandit_pass/bandit20``
Output: *cat: /etc/bandit_pass/bandit21: Permission denied*

As you can see we need permission to run this

---

Check the contents of the file
Code: ``ls``
Output: *bandit20-do*

Check the information about the file
Code: ``file bandit20-do``
Output: *bandit20-do: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=037b97b430734c79085a8720c90070e346ca378e, for GNU/Linux 3.2.0, not stripped*

After searching about this file we come to know that this file is an **executable file** which can be used to put setuid bitin the file and run any command with elevated priviledges

execute this file as an executable

Code: ``./bandit20-do``
Output: *Run a command as another user.
  Example: ./bandit20-do id*

Now we know how to use this file

run the bandit20 file using this

Code: ``./bandit20-do cat /etc/bandit_pass/bandit20``
Output: *21st password*

Save the 21st password
