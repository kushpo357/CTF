# Natas5 → Natas6

Natas5 moves from headers to cookies. Loading the page with valid natas5 credentials showed a message indicating access was denied because you're not "logged in" — even though you'd just authenticated with HTTP Basic Auth to reach the page at all.

Checking the cookies for the site (via browser DevTools → Application/Storage → Cookies) showed a cookie set by the server:

```
Name: loggedin
Value: 0
```

Since cookies are just client-side data attached to each request, and nothing here indicated the value was signed or otherwise validated server-side, editing it directly was worth trying.

**Resolution:** In DevTools, the `loggedin` cookie value was changed from `0` to `1`, then the page was refreshed.

```
Access granted. The password for natas6 is <natas6_password> (will differ per player)
```

**Key insight:** Cookies are entirely client-controlled. If a server uses a cookie value to represent authentication or authorization state without verifying it against real server-side session data (or signing/encrypting the value), the client can simply set whatever value it wants and the server will trust it.

**Key technique:** Editing a client-side cookie value directly in browser DevTools to bypass a login-state check.
