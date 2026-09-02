log into bandit 24 using the 24th password

---

check the level goal
Source: https://overthewire.org/wargames/bandit/bandit25.html

*A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing. You do not need to create new connections each time.*

Key detail: **no new connection needed per attempt** — this rules out tools like Hydra (which open a fresh connection per guess for known protocols like SSH/HTTP/FTP). This daemon is a custom raw-socket protocol, so we need to build the brute-force ourselves and send everything over a single persistent connection.

---

confirm the port and protocol manually
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

(30002 doesn't show because nmap's default scan range/timing didn't catch it here — but the level goal already told us the port.)

Code: ``nc localhost 30002``
Output:
*I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.*

So the protocol is: one line of plaintext, `<bandit24_password> <pincode>`, sent over the connection.

---

**Building the brute-force pipeline, piece by piece**

**1. Generate all 10000 possible pincodes (0000–9999), zero-padded**

Code: ``seq -w 0 9999``

`seq` prints a sequence of numbers. By default `seq 0 9999` prints `0, 1, 2 ... 9999` — no padding, so `1` instead of `0001`. The `-w` flag ("width") tells `seq` to pad every number to the same width as the largest number in the range, so we get `0000, 0001, 0002 ... 9999`. This matters because the pincode is a 4-digit code — the daemon expects it in that exact format.

---

**2. Prefix every line with bandit24's password**

Code: ``seq -w 0 9999 | sed 's/^/PASSWORD /'``

`sed` (stream editor) rewrites each line of input according to a pattern. The pattern `s/^/PASSWORD /` means "substitute (`s`) the start-of-line marker (`^`) with `PASSWORD `" — since `^` matches an empty position right before every line's content, this effectively **prepends** `PASSWORD ` (with the actual password swapped in) to every line, without touching the number itself.

Result: 10000 lines, each looking like `bandit24_password 0000`, `bandit24_password 0001`, etc. — exactly the format the daemon expects, one guess per line.

---

**3. Send the entire list over a single netcat connection and save the output**

Code: ``seq -w 0 9999 | sed 's/^/bandit24_password /' | nc -w 5 localhost 30002 > /tmp/results24``

Since `nc` reads all piped input and sends it over the same open connection, this satisfies the "no new connections" requirement — all 10000 guesses go out over one TCP session. The daemon responds to each one in turn (mostly with a "Wrong!" style message), and all of that response text gets written to `/tmp/results24`.

`-w 5` sets a timeout: `nc` will close the connection if 5 seconds pass with no more activity, which prevents it from hanging indefinitely after the last response comes back.

This step takes a little while since it's 10000 round trips over one connection — let it finish.

---

**4. Filter the results to find the one line that isn't a failure**

Code: ``grep -v "Wrong" /tmp/results24``

`grep -v` inverts the match — instead of showing lines *containing* "Wrong", it shows every line that does **not** contain it. Since 9999 of the 10000 responses are rejections, inverting the filter leaves just the handful of lines around the one successful attempt, including the response containing bandit25's actual password.

Output:
*<line containing the 25th password>*

save the 25th password

---

**Why this approach instead of Hydra:** Hydra excels at brute-forcing *known* protocols where each attempt is a fresh connection with a request/response Hydra already understands (SSH login, HTTP form POST, etc.). This challenge is a bespoke raw TCP protocol, and it explicitly rewards *not* reconnecting — reusing one connection is far faster than Hydra's per-attempt connection overhead here. Building the guesses with `seq`/`sed` and firing them all down one `nc` session is a lightweight, purpose-built alternative to reaching for a heavier tool that doesn't actually fit the constraint.
