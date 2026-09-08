# Bandit Level 30 → Level 31

Like the previous level, this one hands you a git repository and expects you to go digging through it for the password. This time, though, the history itself is much thinner — which is the whole trick.

Starting with the usual recon:

```bash
git log
```

This showed only a single commit — *"initial commit of README.md"* — with no earlier history to dig through. A quick check of branches confirmed there was nothing else to explore there either:

```bash
git branch -a
```

Output showed only `master` and its remote tracking branches, no additional local or remote branches. Running `git log --all` also returned just the one commit, confirming this wasn't a case of a branch hiding data (unlike 29→30).

**Sticking point:** at this point it looked like a dead end — one commit, no other branches, nothing obviously hidden. The next instinct was to check for *uncommitted* changes sitting in the working directory:

```bash
git status
```

The output showed the branch was up to date with `origin/master` and the working tree was clean — no staged or unstaged changes either. So the password wasn't in an uncommitted edit.

The piece that had been overlooked was **git tags**. A tag is a named pointer to a specific commit — often used to mark release versions — and unlike branches, tags don't move forward as new commits happen. Critically, tags don't show up in `git branch` or plain `git log` output; they have to be listed explicitly:

```bash
git tag
```

This returned a tag named `secret` — a strong signal on its own. Inspecting it:

```bash
git show secret
```

revealed the password: `<bandit31_password> (will differ per player)`.

**Key technique:** git stores references in more places than just branches and commit history — always check `git tag` when a repo seems to have no more secrets to give up, since tags are easy to forget and won't appear in standard log or branch listings.
