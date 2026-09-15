# Natas3 → Natas4

Natas3's page gave no visible hints in its HTML or source — no comments, no suspicious file paths. The next lead came from a standard web-recon habit: checking `robots.txt`.

```bash
curl http://natas3.natas.labs.overthewire.org/robots.txt
```

The file contained a `Disallow` entry:

```
User-agent: *
Disallow: /s3cr3t
```

`robots.txt` exists to tell search engine crawlers which paths *not* to index — but the file itself is world-readable, so a disallowed path is effectively an announcement of where something is being hidden, not a real access restriction.

Browsing directly to the disallowed path:

```
http://natas3.natas.labs.overthewire.org/s3cr3t/
```

led to a directory listing, and inside it was a file containing the password for natas4.

**Key insight:** `robots.txt` restricts crawler behavior, not human (or attacker) access. A `Disallow` entry is often a map straight to the content someone wanted to keep out of search results — and, unintentionally, out of casual browsing, but not out of direct requests.

**Key technique:** Checking `robots.txt` for disallowed paths and browsing to them directly to find hidden content.
