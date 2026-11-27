# Git Learning Guide

This guide teaches Git step by step, starting with `git init`. Follow the sections in order and try the commands in a new, empty practice folder on your computer.

Commands are entered in a terminal opened inside your practice folder. When a command uses an example file or branch name, you can use that name or replace it with your own.

## Git and GitHub

**Git** keeps a history of changes to your files. It lets you save checkpoints, compare versions, and work on changes in separate branches.

**GitHub** is a website that hosts Git repositories and provides collaboration features. A pull request is a GitHub feature; Git itself is the tool that tracks and shares the code. Other hosting sites, such as GitLab and Bitbucket, offer similar features.

## 1. Create a Git repository

Open a terminal in the empty folder you want Git to track, then run:

```bash
git init
```

This creates a hidden `.git` folder that stores the project's history and settings. You usually run `git init` once when starting a project; do not run it inside a project that is already a Git repository.

## 2. Set your commit name and email

Before making your first commit, tell Git the name and email to record as the author:

```bash
git config --global user.name "Your Name"
```

This sets the name Git records on your commits for your user account.

```bash
git config --global user.email "you@example.com"
```

This sets the email Git records on your commits. Use an email you recognize; if you use a hosting site, you may want to use an email connected to that account.

```bash
git config --list
```

This displays your Git settings. The `--global` option applies a setting to all your repositories; without it, a setting applies only to the current repository.

## 3. Check the repository's state

Create a file such as `notes.txt` in your practice folder using a text editor. Then run:

```bash
git status
```

This shows your current branch, which files are new or changed, and whether changes are staged or still uncommitted. Git calls a file it has not started tracking an **untracked file**.

## 4. Stage changes for a commit

Git lets you choose which changes go into each commit. The selected changes are called **staged** changes.

```bash
git add notes.txt
```

This stages `notes.txt` for the next commit. Replace the filename with the file you changed.

```bash
git add .
```

This stages all changes under the current folder. Run `git status` first so you can see what will be included.

```bash
git diff
```

This shows changes you have made but have not staged yet.

```bash
git diff --staged
```

This shows the changes you have staged, so you can review what the next commit will contain.

## 5. Save a commit

```bash
git commit -m "Add practice notes"
```

This saves the staged changes as a **commit**, a named checkpoint in the repository's history. The text after `-m` is the commit message. Use a short message that describes the change.

```bash
git status
```

Run this again after a commit. If you have no remaining changes, Git reports that the working tree is clean.

### The three places Git tracks

```text
Working directory  ->  Staging area  ->  Repository history
edit files             choose changes    save a commit
```

- **Working directory:** the files you see and edit.
- **Staging area:** the changes selected for the next commit.
- **Repository history:** the commits Git has saved.

## 6. Read the commit history

```bash
git log
```

This lists commits, newest first, with details such as the author, date, and message. Press `q` to leave the history view.

```bash
git log --oneline
```

This shows one short line per commit, including a shortened commit ID and its message.

```bash
git show
```

This shows the latest commit and the changes it saved.

`HEAD` is Git's name for the commit currently checked out. A commit ID identifies a particular commit and appears in the output of `git log`.

## 7. Use branches for separate work

A **branch** is a separate line of work. You can make changes on a feature branch while keeping the `main` branch stable.

```bash
git branch
```

This lists your local branches. The `*` marks the branch you are currently using.

```bash
git switch -c add-greeting
```

This creates a branch named `add-greeting` and switches to it. Create a branch before starting a new feature or fix.

Make a change, then stage and commit it as shown earlier. The commit will be added to the branch you are on.

```bash
git switch main
```

This switches to the existing `main` branch. Check `git status` first and save work you want to keep; Git may prevent a switch if it could overwrite your changes.

```bash
git merge add-greeting
```

Run this while on `main` to bring the commits from `add-greeting` into `main`. A merge joins the work from two branches.

```bash
git branch -d add-greeting
```

This deletes the local `add-greeting` branch after its work has been merged. It deletes the branch name, not the commits already merged into `main`.

## 8. Resolve a merge conflict

A **merge conflict** happens when Git cannot automatically combine two edits to the same part of a file. Git marks the conflicting section in the file, for example:

```text
<<<<<<< HEAD
the version on your current branch
=======
the version being merged in
>>>>>>> add-greeting
```

Open the file, choose or combine the content you want, and remove all the marker lines. Then run:

```bash
git status
```

This shows which files still need conflict resolution.

```bash
git add notes.txt
```

After editing the conflicted file, this stages the resolved version. Use the actual filename shown by `git status`.

```bash
git commit
```

This completes a merge after you have staged the resolved files. Follow any instructions Git prints in the terminal.

## 9. Connect to an online repository

You can keep using Git locally. To share your work, first create an empty repository on GitHub, GitLab, or another hosting site. Copy its repository URL, then connect your local repository to it:

```bash
git remote add origin https://github.com/your-name/git-practice.git
```

This saves the online repository's address under the nickname `origin`. The URL above is an example; replace it with the URL for your own empty online repository.

```bash
git remote -v
```

This displays the remote names and addresses connected to your local repository.

```bash
git branch -m main
```

If your current branch has a different name, this renames it to `main`, a common default branch name. Skip this command if your current branch is already named `main`.

```bash
git push -u origin main
```

This uploads the `main` branch and its commits to `origin`. The `-u` option links your local branch to the online branch, so later `git push` and `git pull` can usually work without extra names.

## 10. Share and receive changes

```bash
git push
```

This uploads your new commits on the current branch to its connected online branch.

```bash
git fetch origin
```

This downloads new commits and branch information from `origin`. It does not merge those changes into your current branch or change your files.

```bash
git pull
```

This downloads changes from the connected online branch and integrates them into your current branch. Check `git status` first and commit or save your own work before pulling.

## 11. Open a pull request

A **pull request (PR)** asks other people to review your branch and merge it into another branch. You open and review a pull request on a hosting website; there is no `git pull-request` command.

A typical workflow looks like this:

```bash
git switch main
```

Switch to the main branch before starting a new piece of work.

```bash
git pull
```

Update your local `main` with the latest changes from the online repository.

```bash
git switch -c fix-typo
```

Create and switch to a branch for your change. Give the branch a short, descriptive name.

Edit a file, then stage and commit your change:

```bash
git add notes.txt
```

Stage the file you changed.

```bash
git commit -m "Fix spelling in notes"
```

Save the staged change in a commit.

```bash
git push -u origin fix-typo
```

Publish your branch online. Then open the hosting website and create a pull request from `fix-typo` into `main`. Teammates can review it, request changes, and approve it. Push any follow-up commits to the same branch; they appear in the pull request. After it is merged, switch to `main` and run `git pull` to get the merged changes.

If you cannot push to the original project, you may need to create a **fork**—your own online copy—and open the pull request from your fork.

## 12. Undo or correct common changes

Check `git status` before undoing work. These commands have different effects:

```bash
git restore notes.txt
```

This discards unstaged edits to `notes.txt` and restores it to the staged version. Use it only if you are sure you do not need those edits.

```bash
git restore --staged notes.txt
```

This unstages `notes.txt` but keeps your edits in the working directory.

```bash
git commit --amend -m "Updated commit message"
```

This replaces your latest commit and its message. Any staged changes are included in the replacement commit. Use it for a commit you have not shared; changing a commit that others already have can cause confusion.

```bash
git revert abc1234
```

This creates a new commit that reverses the changes from the commit with ID `abc1234`. Replace `abc1234` with the real commit ID. Reverting is generally the safer way to undo a commit that has already been shared.

## 13. Ignore files you do not want to commit

Create a file named `.gitignore` in the repository folder. Add file names or patterns that Git should ignore, one per line. For example:

```gitignore
node_modules/
.env
*.log
```

This prevents matching untracked files from being added to future commits. A file already tracked by Git remains tracked even after you add it to `.gitignore`.

```bash
git rm --cached .env
```

This removes `.env` from Git's tracking while keeping your local copy. Use the actual filename you want Git to stop tracking, then commit the change. Do not commit passwords or secret keys.

## 14. Save work temporarily with stash

```bash
git stash push -m "work in progress"
```

This temporarily shelves tracked, uncommitted changes so you can switch tasks. The message helps you remember what the saved work is. New, untracked files are left out by default; to include them, use `git stash push -u -m "work in progress"` (`-u` means include untracked files).

```bash
git stash list
```

This lists the changes you have stashed.

```bash
git stash pop
```

This reapplies the newest stash and removes it from the stash list. Check `git status` afterward; Git may report a conflict if the files changed in the meantime.
