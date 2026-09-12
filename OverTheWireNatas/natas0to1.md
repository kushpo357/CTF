# Natas0 → Natas1

Natas0 is the entry point into the Natas wargame, and it starts about as gently as possible: the password for the next level is sitting directly in the page.

Logging in with the starting credentials (`natas0`/`natas0`) over HTTP Basic Auth loads a simple page with a short instruction that the password can be found on the page itself.

```bash
curl -u natas0:natas0 http://natas0.natas.labs.overthewire.org/
```

Viewing the page source (Ctrl+U in most browsers, or just reading the raw HTML returned by curl) reveals an HTML comment containing the password for natas1.

*View source shows something like:*
```html
<!--The password for natas1 is <natas1_password> (will differ per player) -->
```

**Key insight:** HTML comments are stripped from the rendered page but not from the source — anything left in a comment by a developer is fully visible to anyone who checks "View Source."

**Key technique:** Viewing page source to find secrets left in HTML comments.
