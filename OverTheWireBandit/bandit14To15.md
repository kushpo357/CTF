log into bandit 14 using 15th password

---

In this level we have to submit the 15th password on port 30000 on localhost

let us check whether that port is available on localhost or not.
Code: ``nmap localhost``
Output:
*nmap localhost
Starting Nmap 7.80 ( https://nmap.org ) at 2023-12-27 12:28 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00015s latency).
Not shown: 994 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
1111/tcp  open  lmsocialserver
1840/tcp  open  netopia-vo2
4321/tcp  open  rwhois
8000/tcp  open  http-alt
30000/tcp open  ndmps

Nmap done: 1 IP address (1 host up) scanned in 0.10 seconds*

---

The port 30000 is open on localhost with tcp protocol

use telnet or nc to connect to localhost
Code: ``telnet localhost 30000``
Output:
*telnet localhost 30000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
15th password
Correct!
16th password

Connection closed by foreign host.*

**OR**

Code: ``nc localhost 30000``
Output:
*15th password
Correct!
16th password*

save the 15th password
