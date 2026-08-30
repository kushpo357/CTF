log into bandit 20 using the 21st password
---

In this level we have a setuid binary `suconnect` in the home directory.

check what kind of file it is
Code: ``file suconnect``
Output:
*suconnect: setuid ELF 32-bit LSB executable, Intel i386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=..., for GNU/Linux 3.2.0, not stripped*

---

According to the level description, `suconnect` connects out to `localhost` on a port you give it as an argument, reads a line of text from that connection, and compares it to bandit20's password. If it matches, it sends back bandit21's password over that same connection.

Since `suconnect` is setuid, it runs with bandit21's privileges even though we're logged in as bandit20 — that's how it's able to read and hand over bandit21's password.

Important: `suconnect` is a **client**, not a server. It connects out — it doesn't listen. So we need to be the one listening.

---

check which ports are already in use so we don't collide with anything
Code: ``nmap localhost``
Output:
*PORT      STATE SERVICE
22/tcp    open  ssh
1111/tcp  open  lmsocialserver
1840/tcp  open  netopia-vo2
4321/tcp  open  rwhois
8000/tcp  open  http-alt
30000/tcp open  ndmps
50001/tcp open  unknown*

**Note:** these service names (lmsocialserver, netopia-vo2, rwhois, ndmps, etc.) are just nmap's default guesses based on well-known port number assignments — not actual verified services. Most of these are leftover listeners from earlier Bandit levels (e.g. 30000 is the old level14→15 password checker). None of them are related to this challenge, and none of them will validate bandit20's password — that logic only exists inside `suconnect` itself.

Pick any unused port outside this list, e.g. **12345**.

---

first attempt (wrong): tried pointing suconnect at an existing service
Code: ``./suconnect 22``
Output:
*Read: SSH-2.0-OpenSSH_10.2p1
ERROR: This doesn't match the current password!*

This confirms port 22 is just SSH's banner — not something built for this challenge. We need our own listener.

---

set up a listener on the chosen free port and feed it bandit20's password automatically, in the background, then run suconnect against the same port in the same terminal session:

Code: ``echo "bandit20_password_here" | nc -l 12345 &``
Code: ``./suconnect 12345``

Output:
*[1] <pid>
Read: bandit20_password_here
Correct!
<21st password>*

The `&` backgrounds the listener so the same terminal is free to immediately run `suconnect`. `suconnect` connects to our listener, reads the password we echoed, validates it, and (since it's setuid) hands back bandit21's password.

---

confirm both processes finished cleanly
Code: ``jobs``
Output: *[1]+ Done   echo "bandit20_password_here" | nc -l 12345*

Code: ``nmap -p 12345 localhost``
Output: *12345/tcp closed*

Port shows closed since `nc` (without `-k`) exits after serving one connection — confirming the listener did its job and shut down.

save the 21st password