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
