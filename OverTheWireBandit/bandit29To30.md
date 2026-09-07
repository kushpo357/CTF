# Bandit Level 29 → Level 30

This level drops you into a directory containing a git repository. Instead of finding the password sitting in a plaintext file, the goal here is to dig through the repo's version history — the password is hidden somewhere in git's past, not its present.

Start by exploring the repo the same way you'd explore any unfamiliar directory:

```bash
ls -la
```

Inside, there's a `.git` folder, confirming this is a git repository. **A git repo isn't just the current state of files — it's the entire history of changes**, and that history can be inspected even for commits that aren't currently checked out.

The first move was `git log` to see the commit history, followed by checking out the initial commit to compare against the current state:

```bash
git log
git checkout <initial-commit-hash>
```

**Note/Sticking point:** eyeballing the checked-out files for differences didn't reveal anything obviously useful. The right approach wasn't to check out commits one at a time and compare by hand — it was to directly diff between two commits:

```bash
git diff <commit1> <commit2>
```

Before diffing further, it's worth returning to the tip of the branch rather than staying in a detached HEAD state from a checked-out commit:

```bash
git branch -a
git checkout master
```

By default, `git log` only follows the current branch. To see the full picture across every branch in the repo, the `--all` flag is needed:

```bash
git log --all --oneline --graph
```

This revealed several branches beyond `master`: `dev` and `sploits-dev`. One commit on `master` stood out immediately — its message was *"fix username"*. A commit message like that is a strong hint that something related to credentials changed.

Diffing that commit against its parent showed:

```
-- username: bandit30
++ username: bandit29
```

**Sticking point:** this diff was tempting to read as "found the password," but it only changed the *username* field — the password line was still a placeholder (`<no passwords in production!>`), not a real value. It's important to read a diff carefully and confirm what actually changed before assuming you've found the answer.

The real content was sitting on the `dev` branch instead. Rather than checking out the branch and trusting appearances, the safer technique is to view a specific file's content directly from a branch without switching to it:

```bash
git show dev:README.md
```

This displayed the real credentials for `bandit30`, including the password: `<bandit30_password> (will differ per player)`.

**Key technique:** when a git repo hides secrets in its history, don't just diff commits on your current branch — always check `git log --all` to see every branch, and use `git show <branch>:<file>` to inspect file contents on other branches without needing to check them out.
