# Bandit Level 31 → Level 32

This level flips the previous git challenges on their head. Instead of digging through history for a hidden password, the task is to *push* a specific file to the remote repository — and the repo itself has a rule in place to reject or accept your push based on what you send.

Starting with recon as usual:

```bash
ls
cat README.md
```

The README laid out exact requirements: create a file named `key.txt`, containing the exact content `'May I come in?'`, and push it to the `master` branch. **Precision matters here** — filename, content, and branch all had to match exactly what was specified.

The file was created with `vi key.txt`, then the usual add/commit/push flow was attempted:

```bash
git add .
git commit -m "May I come in?"
git status
```

**Sticking point:** `git status` kept reporting *"nothing to commit, working tree clean"* even after creating `key.txt` and running `git add .`. This was confusing at first — the file existed on disk (`ls` showed it) but git wasn't picking it up at all.

Trying to add the file explicitly revealed the real issue:

```bash
git add key.txt
```

```
The following paths are ignored by one of your .gitignore files:
key.txt
Use -f if you really want to add them.
```

**A `.gitignore` file was silently excluding `key.txt` from being tracked.** This is a common real-world git pattern — repos often ignore certain filenames or patterns (build artifacts, secrets, temp files) so they never accidentally get committed. Here, it was clearly set up on purpose to block the exact filename the challenge asked for, forcing engagement with the force-add mechanism rather than ordinary `git add`.

The first instinct was to edit `.gitignore` itself to remove the rule blocking `key.txt`, but that's unnecessary and adds risk of committing unwanted changes to a tracked config file. The cleaner fix is git's built-in override:

```bash
git restore .gitignore
git add -f key.txt
```

`add -f` (force add) tells git to track the file *despite* the ignore rule, without needing to touch `.gitignore` at all. **Key insight: when `git add` silently does nothing and a file won't show up in `git status`, check whether `.gitignore` is excluding it before assuming something else is wrong.**

With `key.txt` properly staged, the commit and push went through:

```bash
git commit -m "May I come in?" && git push origin master
```

The remote's pre-receive hook validated the pushed file and printed the password for bandit32 directly in the push output — *even though the push was ultimately rejected* (`! [remote rejected] master -> master (pre-receive hook declined)`). This is a notable quirk: **the server-side validation script ran and revealed the password before making its final accept/reject decision**, so the rejection didn't matter for completing the level. Password: `<bandit32_password> (will differ per player)`.

**Key technique:** when `git add` doesn't behave as expected and a file refuses to show up as staged, suspect `.gitignore` before anything else — and remember `git add -f` overrides an ignore rule without needing to edit the ignore file itself. Also worth remembering: some server-side git hooks (like this one) print output *before* deciding whether to accept a push, so a "rejected" push doesn't always mean you got nothing out of it — read the full output, not just the final status line.
