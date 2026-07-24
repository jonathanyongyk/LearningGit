# Git Worktree Tutorial

## What is `git worktree`?

`git worktree` lets you check out multiple branches from the same Git repository at the same time, each in its own folder (working tree), while sharing one common `.git` history and object database.

In short: one repository, multiple working directories.

## What problem does it solve?

Normally, one clone can only have one branch checked out at a time. If you need to:

- work on a feature branch,
- quickly hotfix `main`,
- and review another branch,

you would usually stash/commit/switch repeatedly or create extra clones.

`git worktree` solves this by giving you separate folders for each branch, so you can keep work isolated and switch context instantly.

## Benefits

- **Parallel workstreams**: work on multiple branches at once.
- **No repeated clone cost**: worktrees share the same Git data, saving disk and setup time.
- **Cleaner context switching**: each branch has its own directory and files.
- **Safer than constant checkout switching**: reduces accidental overwrites or mixed branch state.

## Caveats

- **One branch per worktree at a time**: the same branch cannot be checked out in two worktrees simultaneously.
- **Path management**: you need to remember where each worktree lives.
- **Cleanup required**: remove unused worktrees (`git worktree remove ...`) so stale directories do not accumulate.
- **Repository-level settings are shared**: some Git config/state is common because all worktrees belong to one repo.

## Step-by-step tutorial (starting from repo initialization)

### 1) Initialize a repository

```bash
mkdir demo-worktree
cd demo-worktree
mkdir src
cd src
git init
```

This creates a new Git repository inside the `src` directory.

### 2) Create initial content and first commit

```bash
echo "# Demo Worktree Repo" > README.md
git add README.md
git commit -m "Initial commit"
```

You need at least one commit before creating meaningful branches and worktrees.  
`git branch -M main` ensures your default branch is named `main` for the rest of this tutorial.

### 3) Create and commit on a feature branch (main working directory)

```bash
git checkout -b feature/login
echo "Login page TODOs" > feature-login.txt
git add feature-login.txt
git commit -m "feat: add initial login work"
```

Now your primary directory has a real feature commit on `feature/login`.

### 4) Go back to `main`

```bash
git checkout main
```

Return to `main` so you can create a hotfix branch in a separate worktree.

### 5) Add a new worktree for the hotfix branch

From inside your original repository directory:

```bash
git worktree add ../demo-hotfix -b hotfix/urgent
```

What this does:

- creates a new folder `../demo-hotfix`,
- creates branch `hotfix/urgent`,
- checks it out in that new folder.

### 6) Make and commit a hotfix in the hotfix worktree

```bash
cd ../demo-hotfix
echo "Critical production fix" > hotfix-note.txt
git add hotfix-note.txt
git commit -m "fix: apply urgent production hotfix"
```

Now you have:

- `feature/login` commit in the original directory.
- `hotfix/urgent` commit in the hotfix worktree.

### 7) Verify branch isolation in both directories

In original directory:

```bash
cd ../src
pwd
git branch --show-current
```

In hotfix worktree:

```bash
cd ../demo-hotfix
pwd
git branch --show-current
```

Each folder stays on its own branch with its own changes.

### 8) List active worktrees

Run from either worktree:

```bash
git worktree list
```

This shows all linked working directories and which branch each uses.

### 9) Merge the hotfix back to `main` first

```bash
cd ../src
git checkout main
git merge hotfix/urgent
```

This models a realistic urgent fix flow: patch production first.

### 10) Merge `main` into `feature/login` to avoid regression

Before merging the feature branch into `main`, pull the hotfix into the feature branch first. This ensures the feature is tested against the latest production code and cannot reintroduce a regression.

```bash
git checkout feature/login
git merge main
```

Resolve any conflicts, then verify the feature still works correctly with the hotfix applied.

### 11) Merge the updated feature branch into `main`

Now that `feature/login` contains the hotfix, it is safe to merge back:

```bash
git checkout main
git merge feature/login
```

A text editor may open for you to edit the merge commit message. Save and close it to complete the merge.

`main` now includes both the urgent fix and the fully-integrated feature work.

### 12) Finish work and remove the extra worktree

```bash
git worktree remove ../demo-hotfix
```
Although the worktree is removed, the `hotfix/urgent` branch still exists in the repository. You can delete it if you no longer need it.

Optionally delete the associated branches if merged:

```bash
git branch -d hotfix/urgent
git branch -d feature/login
```

### 13) Prune stale metadata (optional maintenance)

```bash
git worktree prune
```

Useful if directories were removed manually and Git metadata needs cleanup.

## Common command summary

```bash
# Add worktree for existing branch
git worktree add ../path branch-name

# Add worktree and create new branch
git worktree add ../path -b new-branch

# List worktrees
git worktree list

# Remove a worktree
git worktree remove ../path

# Clean stale worktree references
git worktree prune
```
