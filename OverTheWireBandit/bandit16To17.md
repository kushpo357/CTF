log into bandit 16 using the 17th password

---

check open ports between 31000 and 32000 on localhost
Code: `nmap -p 31000-32000 localhost`
output->
*Starting Nmap 7.80 ( https://nmap.org ) at 2023-12-31 05:43 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00012s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.06 seconds*

**Note: This can be different for you**

---

check open ports that accept ssl connection
Code: ``nmap --script ssl-enum-ciphers -p 31000-32000 localhost``
ports with ssl-enum-ciphers written below them have speak ssl
example:
*PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
| ssl-enum-ciphers:
|   TLSv1.2:
|     ciphers:
|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (secp256r1) - A
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
|       TLS_RSA_WITH_AES_128_CBC_SHA256 (rsa 2048) - A
|       TLS_RSA_WITH_AES_128_CCM (rsa 2048) - A
|       TLS_RSA_WITH_AES_128_CCM_8 (rsa 2048) - A
|       TLS_RSA_WITH_AES_128_GCM_SHA256 (rsa 2048) - A
|     compressors:
|       NULL
|     cipher preference: client
|     warnings:
|       Weak certificate signature: SHA1
|_  least strength: A*

---

there will be some ports which will speak ssl try submitting password on both of them.

To submit the password do this
Code: ``openssl  s_client -connect localhost:31518``

paste your password after
*read R Block*

**Note: If it is not the correct port then it will repeat your given password**

you will get a RSA key copy this key and paste it in a file in your computer after logging out

---

check the permission of the file
Code: ``ls -l file``
Output:
*-rw-r--r-- 1 kushpo kushpo 1675 Dec 29 16:03 file*

change the permissions of the file to ***-r--------*** if it is different
Code: ``chmod 400 file``

**Note: this step is necessary otherwise the ssh will not take this key as the key will be too open**

---


