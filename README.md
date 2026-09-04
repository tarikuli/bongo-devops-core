# bongo-devops-core

This repository is my hands-on Git and GitHub practice project. I am keeping the commands and commit details here so I can look back and understand what I did, not just remember that a task was completed.

## Task 01: First Impression

I started by setting the name and email that Git should use for my commits:

```bash
git config --global user.name "Tarikul Islam"
git config --global user.email "tarikuli@gmail.com"
```

Then I initialized the repository, created the first README, and made the first commit:

```bash
git init
git add README.md
git commit -m "chore: initial repository setup"
```

I created the GitHub repository and pushed the local `main` branch with:

```bash
gh auth status
gh repo create tarikuli/bongo-devops-core \
	--public \
	--source=. \
	--remote=origin \
	--push
```

To check the author, message, branch, and remote, I used:

```bash
git show -s --format='%h%n%s%n%an <%ae>' HEAD
git status --short --branch
git remote -v
```

## Task 02: Safe Space

I created a local `.env` file with a fake value. It is only for practice; real passwords and API keys should never go into Git.

```dotenv
FAKE_PASSWORD=not-a-real-password
```

I added `.env` to `.gitignore`:

```gitignore
.env
```

The rule was checked with:

```bash
git check-ignore -v .env
git status --short --ignored
```

Git reported `.env` as ignored, so it will not appear as an untracked file or get committed accidentally.

## Task 03: Parallel Universe

I created `feature/system-optimization`, added `kernel_tuning.txt`, and committed it there:

```bash
git switch -c feature/system-optimization
git add kernel_tuning.txt
git commit -m "feat: add kernel tuning notes"
git push --set-upstream origin feature/system-optimization
```

After switching back to `main`, `kernel_tuning.txt` was not present. That was the point of the exercise: files and commits belong to the branch where they were made.

## Task 04: Selective Memory

I created two configuration files and deliberately committed them separately. First I staged only the web file:

```bash
git add web_fix.conf
git diff --cached --name-only
git commit -m "fix: update web configuration"
```

Then I did the same for the database file:

```bash
git add db_fix.conf
git diff --cached --name-only
git commit -m "fix: update database configuration"
```

The commits were `b77f170` and `e421ccf`. Checking the staged file before each commit made sure the two changes did not get mixed together.

## Task 05: Cloud Connection

The repository was already connected to GitHub from Task 01. For a new local repository, the command would be:

```bash
git remote add origin https://github.com/tarikuli/bongo-devops-core.git
```

I checked the existing connection and pushed `main` with:

```bash
git remote -v
git remote get-url origin
git push origin main
git ls-remote --heads origin main
```

The `origin` remote points to [tarikuli/bongo-devops-core](https://github.com/tarikuli/bongo-devops-core).

## Task 06: History Detective

At first, the repository did not contain the broken port change described in the exercise. I fixed that by creating `port.conf` on `main`:

```text
smtp_port = 8492
```

I committed and pushed it with:

```bash
git add port.conf
git commit -m "fix: configure SMTP port"
git push origin main
```

To find who wrote the line, I used both history commands:

```bash
git log -p -1 -- port.conf
git blame port.conf
```

The result was:

- Commit: `c206f6d5d846c2a65f5e1299cf79619f3ab2166`
- Author: `Tarikul Islam <tarikuli@gmail.com>`
- Message: `fix: configure SMTP port`
- Date: `2026-09-04 01:35:21 -0400`

## Task 07: Safety Net

I started five lines of work in `feature.py` on `feature/system-optimization`, but did not commit it. An urgent fix then appeared on `main`, so I used a stash to switch context:

```bash
git stash push --include-untracked -m "WIP: feature work before production bug fix"
git switch main
```

The `--include-untracked` option mattered because `feature.py` was a new file. On `main`, I fixed `main.py`, committed it, and pushed it:

```bash
git add main.py
git commit -m "fix: correct production SMTP port"
git push origin main
```

That fix is commit `e5a8298`. Finally, I returned to the feature branch and restored the work:

```bash
git switch feature/system-optimization
git stash pop
```

`feature.py` was back, still uncommitted.

## Task 08: Clean Merge

I made three deliberately small commits on `feature/system-optimization` while editing `squash-practice.txt`:

```bash
git add squash-practice.txt
git commit -m "wip: start squash practice"
git commit -am "wip: refine squash practice wording"
git commit -am "wip: finish squash practice wording"
```

Those commits were `3283c90`, `bb7adcd`, and `0b71e00`. I combined them into one change on `main`:

```bash
git switch main
git merge --squash feature/system-optimization
git commit -m "feat: consolidate system optimization work"
git push origin main
```

The result was one clean commit, `5e15384`, instead of three small work-in-progress commits in the `main` history.

## Task 09: Conflict Resolution

I created `optimization.txt` on `main`, changed line 1 differently on another branch, and merged the branches. Git stopped with conflict markers because both branches changed the same line.

The important part of the merge was:

```bash
git merge feature/conflict-resolution-v2
```

I edited the file manually, removed `<<<<<<<`, `=======`, and `>>>>>>>`, kept the production wording, and finished the merge:

```bash
git add optimization.txt
git commit -m "merge: resolve optimization conflict"
git push origin main
```

The merge commit is `84b52e2`. I checked that no conflict markers remained with:

```bash
grep -nE '^(<<<<<<<|=======|>>>>>>>)' optimization.txt
git log --oneline --graph -6
```

## Task 10: Time Machine

I made a recovery checkpoint and then intentionally moved `main` back one commit:

```bash
git add recovery_checkpoint.txt
git commit -m "chore: create reflog recovery checkpoint"
git reset --hard HEAD~1
```

The commit seemed to disappear, but `git reflog` still knew where `HEAD` had been:

```bash
git reflog -6 --date=iso
git reset --hard 261c1e44c2f249a479eeed32a51a454374157f83
```

That restored `recovery_checkpoint.txt` and the commit. The recovery commit was `261c1e44`, authored by `Tarikul Islam <tarikuli@gmail.com>`.

Repository: [github.com/tarikuli/bongo-devops-core](https://github.com/tarikuli/bongo-devops-core)
