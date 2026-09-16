# Natas4 → Natas5

Natas4 introduces HTTP header manipulation. The page's content stated:

```
Access disallowed. You are visiting from "http://natas5.natas.labs.overthewire.org/index.php" while authorized
users should come only from "http://natas5.natas.labs.overthewire.org/"
```

This told me the server was checking the `Referer` header — the header a browser normally sets to indicate which page a request was linked from — and rejecting requests unless it matched an expected value.

**Setup:** Burp Suite (running on the Windows side of the WSL2 setup, since it needs a GUI) was set up as an intercepting proxy on `127.0.0.1:8080`, with FoxyProxy in the browser pointed at that listener. The request was sent to Burp's Repeater tab for editing and replaying without needing to re-intercept every time.

**Sticking point:** An early lead — a `wechallinfo` JavaScript variable visible in the page response containing a `"level"` and `"pass"` field — looked like it might be leaking the next level's password directly. It turned out this variable is unrelated to the Referer challenge: it's a hook used by the WeChall browser extension (an external progress-tracking tool for wargame sites like OverTheWire) to auto-register level completion, and it always echoes back the *current* level's own credentials, not the next level's. It was a dead end for this purpose, but useful to know it's a leak present on every Natas page.

**Resolution:** In Repeater, the `Referer` header was edited to the exact value the page required:

```
Referer: http://natas5.natas.labs.overthewire.org/
```

Sending the request returned:

```
Access granted. The password for natas5 is <natas5_password> (will differ per player)
```

**Key insight:** The `Referer` header is entirely client-controlled — nothing stops a client from setting it to whatever value it wants. Using it as an access-control mechanism is a logic flaw: it can distinguish accidental navigation from a legitimate link, but it cannot verify origin against an actual adversary.

**Key technique:** Intercepting and editing the `Referer` header in Burp Repeater to bypass a header-based access check.
