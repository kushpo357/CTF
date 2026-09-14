# Natas2 → Natas3

Natas2's page doesn't have an obvious comment leaking the password. Instead, the lead came from something more subtle: an image referenced in the page source.

Checking the page source showed an `<img>` tag pointing to a path like `files/pixel.png`, rather than being inlined or served from a generic asset path. That's a signal worth following — a `files/` directory implies there may be more in that directory than just the one image being referenced.

Browsing directly to the directory (rather than the specific image file):

```
http://natas2.natas.labs.overthewire.org/files/
```

With directory listing enabled (or by guessing common filenames if it wasn't), a `users.txt` file was reachable in that same folder. Opening it directly revealed a list of users and their passwords for the wargame, including the natas3 password.

```
http://natas2.natas.labs.overthewire.org/files/users.txt
```

*users.txt contents included something like:*
```
natas3:<natas3_password> (will differ per player)
```

**Key insight:** Any file path referenced in a page's source (images, scripts, stylesheets) reveals part of the server's directory structure. That structure is worth exploring beyond just the one file that was linked — directories often contain more than what's directly referenced.

**Key technique:** Following an image path in page source to discover a hidden directory, then reading an exposed `users.txt` file within it.
