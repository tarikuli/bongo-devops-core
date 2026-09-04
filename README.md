# bongo-devops-core

## Task 01: First Impression

### Q: What was the goal of this task?

Create a new Git repository, configure the Git author identity, add an initial `README.md`, commit the setup, and publish the repository to GitHub.

### Q: How was the Git identity configured?

```bash
git config --global user.name "Tarikul Islam"
git config --global user.email "tarikuli@gmail.com"
```

These settings identify the author of future commits on this computer.

### Q: How was the repository initialized?

```bash
git init
```

This creates a local Git repository in the project directory.

### Q: How was the initial file staged and committed?

```bash
git add README.md
git commit -m "chore: initial repository setup"
```

`git add` stages the README, and `git commit` records the staged changes with a descriptive message.

### Q: How was GitHub authentication checked?

```bash
gh auth status
```

This confirms that GitHub CLI is authenticated and shows the active GitHub account.

### Q: How was the GitHub repository created and pushed?

```bash
gh repo create tarikuli/bongo-devops-core \
	--public \
	--source=. \
	--remote=origin \
	--push
```

This creates the public GitHub repository, adds it as the `origin` remote, and pushes the local branch.

### Q: How was the result verified?

```bash
git show -s --format='%h%n%s%n%an <%ae>' HEAD
git status --short --branch
git remote -v
```

These commands verify the latest commit, author, clean working tree, branch tracking, and remote URL.

### Result

Repository: [tarikuli/bongo-devops-core](https://github.com/tarikuli/bongo-devops-core)

Initial commit: `chore: initial repository setup`

Branch: `main` tracking `origin/main`

## Task 02: Safe Space

### Q: What was the goal of this task?

Create a local `.env` file with a fake password, configure Git to ignore it, and verify that the file is not included in Git changes.

### Q: What was added to `.env`?

```dotenv
FAKE_PASSWORD=not-a-real-password
```

This value is intentionally fake. Real passwords, API keys, and database credentials must never be committed to Git.

### Q: How was `.env` ignored?

The following rule was added to `.gitignore`:

```gitignore
.env
```

This tells Git to ignore the `.env` file in the repository root.

### Q: How was the ignore rule verified?

```bash
git check-ignore -v .env
git status --short --ignored
```

`git check-ignore -v .env` identifies the exact ignore rule, while `git status --short --ignored` shows `.env` as ignored instead of untracked.

### Result

The fake `.env` file exists locally, `.gitignore` protects it from accidental commits, and only `.gitignore` is staged for tracking.

## Task 03: Parallel Universe

### Q: What was the goal of this task?

Create a feature branch, add `kernel_tuning.txt` inside that branch, commit and publish the change, then switch back to `main` and observe that the file is not present there.

### Q: How was the feature branch created?

```bash
git switch -c feature/system-optimization
```

This creates the `feature/system-optimization` branch from the current `main` commit and switches to it.

### Q: How was `kernel_tuning.txt` committed?

```bash
git add kernel_tuning.txt
git commit -m "feat: add kernel tuning notes"
```

The file was staged and recorded in commit `7f4f2cb` on the feature branch.

### Q: How was the feature branch pushed to GitHub?

```bash
git push --set-upstream origin feature/system-optimization
```

This publishes the branch and sets `origin/feature/system-optimization` as its upstream branch.

### Q: How was the file's branch-specific behavior verified?

```bash
git switch main
test ! -e kernel_tuning.txt
git status --short --branch --ignored
```

After switching to `main`, the test succeeds because `kernel_tuning.txt` exists only in the feature branch. The status check confirms that `main` remains clean apart from the intentionally ignored local `.env`.

### Result

The feature branch is available on GitHub at [`feature/system-optimization`](https://github.com/tarikuli/bongo-devops-core/tree/feature/system-optimization), while `main` does not contain `kernel_tuning.txt`.

## Task 04: Selective Memory

### Q: What was the goal of this task?

Create two configuration files, stage only one file at a time, and record each change in its own commit for a clear project history.

### Q: Which files were created?

```text
web_fix.conf
db_fix.conf
```

Both files contain practice configuration settings for separate services.

### Q: How was only `web_fix.conf` committed first?

```bash
git add web_fix.conf
git diff --cached --name-only
git commit -m "fix: update web configuration"
```

The staged-file check confirmed that only `web_fix.conf` was in the index. The change was recorded as commit `b77f170`.

### Q: How was `db_fix.conf` committed separately?

```bash
git add db_fix.conf
git diff --cached --name-only
git commit -m "fix: update database configuration"
```

The second staged-file check confirmed that only `db_fix.conf` was in the index. The change was recorded as commit `e421ccf`.

### Q: Why use separate commits?

Separate commits make each change easier to review, revert, and understand. Selective staging prevents unrelated files from being bundled into the same commit.

### Result

The web and database configuration updates are committed separately on `main` and ready to be pushed to GitHub.

## Task 05: Cloud Connection

### Q: What was the goal of this task?

Connect the local repository to GitHub with an `origin` remote, push the `main` branch, and confirm that the project history is visible online.

### Q: How is a local repository linked to GitHub?

For a new local repository with no remote, use:

```bash
git remote add origin https://github.com/tarikuli/bongo-devops-core.git
```

`origin` is the conventional short name for the primary remote repository.

### Q: How was this repository's existing connection verified?

This repository was already linked to GitHub from Task 01, so the existing remote was reused:

```bash
git remote -v
git remote get-url origin
```

Both checks confirmed that `origin` points to `tarikuli/bongo-devops-core`.

### Q: How was `main` pushed?

```bash
git push origin main
```

This sends the local `main` branch to the `main` branch on GitHub.

### Q: How was the published branch verified?

```bash
git status --short --branch
git ls-remote --heads origin main
```

The status output confirms that local `main` tracks `origin/main`, and `git ls-remote` confirms that the remote `main` branch is available on GitHub.

### Result

The local repository is connected to [tarikuli/bongo-devops-core](https://github.com/tarikuli/bongo-devops-core), and the `main` branch is published and visible online.

## Task 06: History Detective

### Q: What was the goal of this task?

Investigate the repository history to find the commit and author responsible for changing a port number in a shared configuration file.

### Q: How was the port configuration created and pushed?

```bash
printf 'smtp_port = 8492\n' > port.conf
git add port.conf
git commit -m "fix: configure SMTP port"
git push origin main
```

The file was committed directly on `main` and pushed to GitHub.

### Q: Which commands identified the author and commit?

```bash
git log -p -1 -- port.conf
git blame port.conf
```

`git log -p` shows the commit patch, while `git blame` attributes the current line to the commit that introduced it.

### Q: What did the investigation find?

The line `smtp_port = 8492` was added by:


### Result

`port.conf` is published on `main`, and Git history confirms that Tarikul Islam wrote the `smtp_port = 8492` line.

## Task 07: Safety Net

### Q: What was the goal of this task?

Practice switching context between feature work and an urgent production bug fix without losing uncommitted changes.

### Q: How was the feature work started?

```bash
git switch feature/system-optimization
```

On the feature branch, `feature.py` was created with five lines of code and intentionally left uncommitted.

### Q: How was the feature work hidden safely?

```bash
git stash push --include-untracked -m "WIP: feature work before production bug fix"
git switch main
```

`--include-untracked` is important because `feature.py` was a new, untracked file. The stash temporarily removed it from the working tree so the urgent fix could be handled on `main`.

### Q: How was the production bug fixed?

```bash
git add main.py
git commit -m "fix: correct production SMTP port"
git push origin main
```

The bug fix was committed and pushed as `e5a8298`. The corrected `main.py` returns SMTP port `8492`.

### Q: How was the feature work restored?

```bash
git switch feature/system-optimization
git stash pop
```

`git stash pop` restored `feature.py` to the feature branch without creating a commit.

### Result

The production bug fix is published on `main`, while the five-line `feature.py` work is restored and remains uncommitted on `feature/system-optimization`.


## Task 08: Clean Merge

### Q: What was the goal of this task?

Create three small, messy commits on `feature/system-optimization`, then combine them into one clean commit when merging into `main`.

### Q: How were the three practice commits created?

```bash
git switch feature/system-optimization
git add squash-practice.txt
git commit -m "wip: start squash practice"
git commit -am "wip: refine squash practice wording"
git commit -am "wip: finish squash practice wording"
```

The three feature-branch commits were `3283c90`, `bb7adcd`, and `0b71e00`. Each made a small change to `squash-practice.txt`.

### Q: How was the feature branch squashed into `main`?

```bash
git switch main
git merge --squash feature/system-optimization
git commit -m "feat: consolidate system optimization work"
git push origin main
```

`git merge --squash` staged the combined result without creating a merge commit automatically. The changes were then recorded as one clean commit: `5e15384`.

### Q: How was the result verified?

```bash
git log --oneline main -4
git show --stat --oneline main
git status --short --branch
```

The `main` history shows one consolidated commit for the feature work, and the status confirms that `main` is synchronized with GitHub.

### Result

The three messy feature commits were consolidated into the single clean commit `5e15384` on `main` and pushed to GitHub.

## Task 09: Conflict Resolution

### Q: What was the goal of this task?

Create two different edits to line 1 of `optimization.txt`, merge the branches, resolve the conflict manually, and complete the merge.

### Q: How was the main version created?

```bash
git switch main
printf 'Optimization baseline: stable configuration.\n' > optimization.txt
git add optimization.txt
git commit -m "docs: add optimization baseline"
git push origin main
```

The baseline was published on `main` as commit `a8a7c6a`.

### Q: How was the conflicting branch version created?

```bash
git switch -c feature/conflict-resolution-v2 a8a7c6a
printf 'Optimization branch: experimental configuration.\n' > optimization.txt
git add optimization.txt
git commit -m "feat: add alternate optimization"
git push --set-upstream origin feature/conflict-resolution-v2
```

The feature branch changed line 1 to `Optimization branch: experimental configuration.` in commit `f0506aa`.

### Q: How was the conflict created and resolved?

```bash
git switch main
git merge feature/conflict-resolution-v2
```

Git inserted `<<<<<<<`, `=======`, and `>>>>>>>` markers because both branches changed the same line. The file was edited manually to keep `Optimization main: production configuration.`, then the merge was completed:

```bash
git add optimization.txt
git commit -m "merge: resolve optimization conflict"
git push origin main
```

The resolved merge commit is `84b52e2`.

### Q: How was the merge verified?

```bash
grep -nE '^(<<<<<<<|=======|>>>>>>>)' optimization.txt
git status --short --branch
git log --oneline --graph -6
```

An empty marker search confirms that no conflict markers remain, and the graph shows the resolved merge on `main`.

### Result

The conflict was resolved manually, the merge was completed as `84b52e2`, and the resolved `main` branch was pushed to GitHub.
