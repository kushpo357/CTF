log into bandit 18 using 18th password
but you get logged out automatically

---

Here the .bashrc is configured to log you out as soon as you log in

For this we can either run the command as soon as we log in
**Or**
We can log in through other methods

---

###Directly executing the code

We know that the password is saved in readme file so we can use `cat` command to show the code

Code:``ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme``
*Output: 
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:*
Enter the 18th password
Output:*19th password*

this happens because the `cat readme` command runs as soon as the login happens

Save the 19th password

---

###Logging in through other methods

1. **Using** ``bash --noprofile``

Code: `` ssh bandit18@bandit.labs.overthewire.org -p 2220 "bash --noprofile"``
*Output:                 _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:*
Enter the 18th password
There will be no output
Code: `ls`
Output: *readme*
Code: ``cat readme``
Output: *19th password*
Code: ``exit``

The ``"bash --noprofile"`` code open ssh in a non-interactive, non-login shell
which ignores the .bashrc and .bash\_profile files
2. **Using a temp shell**

Code: ``ssh bandit18@bandit.labs.overthewire.org -p 2220 /bin/sh``
*Output:                 _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:*
Enter the 18th password
There will be no output
Code: ``ls``
Output: *readme*
Code: ``cat readme``
Output: *19th password*

The additional ``/bin/sh`` Code opens a temp sh terminal through which you can interact with the ssh
You can also use ``/dash`` or ``/bash`` instead of ``/sh``


