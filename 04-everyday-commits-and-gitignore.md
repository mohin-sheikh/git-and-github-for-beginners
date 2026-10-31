# Everyday Commits and .gitignore

> **Session 4 of 12** | Part 1: Git on Your Computer | [All sessions](README.md#course-outline)

## Table of Contents
- [Where You Left Off](#where-you-left-off)
- [See What You Changed with git diff](#see-what-you-changed-with-git-diff)
- [See What You Staged](#see-what-you-staged)
- [Two Changes, Two Commits](#two-changes-two-commits)
- [Writing Good Commit Messages](#writing-good-commit-messages)
- [Renaming and Deleting Files](#renaming-and-deleting-files)
- [Files Git Should Ignore](#files-git-should-ignore)
- [Writing a .gitignore File](#writing-a-gitignore-file)
- [Beginner Mistakes](#beginner-mistakes)
- [Practice Exercises](#practice-exercises)
- [Interview Questions](#interview-questions)

---

## Where You Left Off

Open `my-notes` in VS Code, and open a terminal. Session 03 made two commits

```bash
git log --oneline
```

```text
57d1341 (HEAD -> main) Add a note about git status
22a462d Add my first notes
```

---

## See What You Changed with git diff

Add a fourth line to `notes.txt` and save it

notes.txt

```txt
Things I learned today
- Git saves snapshots called commits
- git status shows what Git sees
- git diff shows what changed
```

`git status` tells you **which** files changed. `git diff` tells you **what** changed in them

```bash
git diff
```

```text
diff --git a/notes.txt b/notes.txt
index a627806..c7fd016 100644
--- a/notes.txt
+++ b/notes.txt
@@ -1,3 +1,4 @@
 Things I learned today
 - Git saves snapshots called commits
 - git status shows what Git sees
+- git diff shows what changed
```

The line that starts with `+` is new, and it is green in the terminal. Lines with no sign did not change; Git shows them so you can see where the change is.

When the output is longer than the screen, Git shows it one page at a time: press **Space** for the next page and **q** to quit.

---

## See What You Staged

Stage the change, and run `git diff` again

```bash
git add notes.txt
git diff
```

It prints nothing: there are no changes left that are not staged. To see what you staged, add `--staged`

```bash
git diff --staged
```

```text
diff --git a/notes.txt b/notes.txt
index a627806..c7fd016 100644
--- a/notes.txt
+++ b/notes.txt
@@ -1,3 +1,4 @@
 Things I learned today
 - Git saves snapshots called commits
 - git status shows what Git sees
+- git diff shows what changed
```

![git diff compares the working folder with the staging area; git diff --staged compares the staging area with the last commit](images/04-everyday-commits-and-gitignore/diff-vs-staged.gif)

| Command              | Shows                                        |
| -------------------- | -------------------------------------------- |
| `git diff`           | Changes you have not staged yet              |
| `git diff --staged`  | Changes that go into the next commit         |

Run `git diff --staged` before you commit, and you always know what you are saving

```bash
git commit -m "Add a note about git diff"
```

```text
[main ed6903f] Add a note about git diff
 1 file changed, 1 insertion(+)
```

---

## Two Changes, Two Commits

Now make two changes that have nothing to do with each other

1. In `notes.txt`, change the first line to `Things I learned this week`
2. Create a new file `todo.txt`

todo.txt

```txt
- Learn branches
- Put my notes on GitHub
```

```bash
git status
```

```text
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   notes.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	todo.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

```bash
git diff
```

```text
diff --git a/notes.txt b/notes.txt
index c7fd016..5bcc735 100644
--- a/notes.txt
+++ b/notes.txt
@@ -1,4 +1,4 @@
-Things I learned today
+Things I learned this week
 - Git saves snapshots called commits
 - git status shows what Git sees
 - git diff shows what changed
```

A changed line shows as two lines: the old one with `-`, the new one with `+`. `todo.txt` is not in the output, because `git diff` only shows files that Git already tracks.

![The parts of git diff: the header says which file changed, the @@ line says where, the red line with - is the old line, the green line with + is the new one, and lines with no sign are unchanged](images/04-everyday-commits-and-gitignore/diff-reading.gif)

Two different changes belong in two commits. Stage only `todo.txt`

```bash
git add todo.txt
git status
```

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   todo.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   notes.txt
```

`todo.txt` will be in the next commit, and `notes.txt` will not. Commit, then do the same for `notes.txt`

```bash
git commit -m "Add a todo list"
```

```text
[main 32af224] Add a todo list
 1 file changed, 2 insertions(+)
 create mode 100644 todo.txt
```

```bash
git add notes.txt
git commit -m "Change the notes title to this week"
```

```text
[main 4526919] Change the notes title to this week
 1 file changed, 1 insertion(+), 1 deletion(-)
```

`1 insertion(+), 1 deletion(-)`: a changed line counts as one line removed and one line added.

![Two changed files become two commits: git add todo.txt then a commit, git add notes.txt then a second commit](images/04-everyday-commits-and-gitignore/two-commits.gif)

When all your changes belong together, you can stage everything at once with `git add .` (the dot means this folder and everything in it). Run `git status` first, so you know what you are adding.

---

## Writing Good Commit Messages

Months from now, you will read your history to find when something changed. Messages like `update` will not help you.

![Unclear messages like update, fix and asdf next to clear ones like Add a todo list and Fix the price shown in the cart](images/04-everyday-commits-and-gitignore/commit-messages.gif)

* Start with a verb: **Add**, **Fix**, **Change**, **Remove**, **Rename**
* Say what the commit does, so the message makes sense on its own
* Keep it short: about 50 characters
* If the message needs the word "and", it may be two commits

---

## Renaming and Deleting Files

In VS Code, rename `todo.txt` to `plan.txt` (right-click the file, **Rename**)

```bash
git status
```

```text
On branch main
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    todo.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	plan.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Git sees a deleted file and a new file. Stage both, and Git understands that it is a rename

```bash
git add .
git status
```

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	renamed:    todo.txt -> plan.txt
```

```bash
git commit -m "Rename todo.txt to plan.txt"
```

```text
[main 28eebaf] Rename todo.txt to plan.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename todo.txt => plan.txt (100%)
```

Deleting works the same way: delete the file in VS Code, run `git add .`, and commit. The old commits still contain the file.

---

## Files Git Should Ignore

Some files must never go into a commit

| File or folder     | Why it must stay out                                        |
| ------------------ | ----------------------------------------------------------- |
| `.env`             | It holds passwords and secret keys                          |
| `node_modules/`    | Thousands of package files that `npm install` creates again |
| `*.log`            | Log files that change all the time                          |

In a Node.js project these appear by themselves. To practise, create them in `my-notes`: a folder `node_modules`, a file `.env` with the line `DB_PASSWORD=your-password-here`, and a file `debug.log` with any text. Then

```bash
git status
```

```text
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.env
	debug.log
	node_modules/

nothing added to commit but untracked files present (use "git add" to track)
```

Running `git add .` now would put your password in a commit.

---

## Writing a .gitignore File

Create a file called exactly `.gitignore` in `my-notes`. The name starts with a dot and has no other ending

.gitignore

```txt
node_modules/
.env
*.log
```

| Line              | Git ignores                                              |
| ----------------- | -------------------------------------------------------- |
| `node_modules/`   | The folder `node_modules` and everything inside it       |
| `.env`            | The file `.env`                                          |
| `*.log`           | Every file that ends in `.log` (`*` means "anything")    |

```bash
git status
```

```text
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

![The files .env, debug.log and node_modules match the lines of .gitignore and are crossed out; git status only shows .gitignore](images/04-everyday-commits-and-gitignore/gitignore-filter.gif)

The ignored files are still in your folder, and VS Code shows them in grey. Git simply does not see them. Commit the `.gitignore` file itself, so it works for everyone who gets the project

```bash
git add .
git commit -m "Add a .gitignore file"
```

```text
[main 66ccd5b] Add a .gitignore file
 1 file changed, 3 insertions(+)
 create mode 100644 .gitignore
```

```bash
git log --oneline
```

```text
66ccd5b (HEAD -> main) Add a .gitignore file
28eebaf Rename todo.txt to plan.txt
4526919 Change the notes title to this week
32af224 Add a todo list
ed6903f Add a note about git diff
57d1341 Add a note about git status
22a462d Add my first notes
```

GitHub keeps ready-made `.gitignore` files for many kinds of projects. The one for Node.js is [Node.gitignore](https://github.com/github/gitignore/blob/main/Node.gitignore).

---

## Beginner Mistakes

### Mistake 1: Running git add . without looking

It stages everything, including passwords, logs and `node_modules`. Keep a `.gitignore` in every project, and run `git status` before `git add .`.

---

### Mistake 2: Messages like "update" or "fix"

They tell nobody what changed. Write what the commit does: `Fix the price shown in the cart`.

---

### Mistake 3: Ignoring a file after it was committed

In another project, `.env` was committed in the first commit. Later it was added to `.gitignore` and changed

```bash
git status
```

```text
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   .env

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore

no changes added to commit (use "git add" and/or "git commit -a")
```

`.gitignore` only works for files that Git does not track yet. To stop tracking `.env` and keep the file in your folder

```bash
git rm --cached .env
```

```text
rm '.env'
```

```bash
git status
```

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	deleted:    .env

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore
```

`deleted` means deleted from the next commit, not from your folder. Commit both changes

```bash
git add .gitignore
git commit -m "Stop tracking .env"
```

```text
[main ec30857] Stop tracking .env
 2 files changed, 1 insertion(+), 1 deletion(-)
 delete mode 100644 .env
 create mode 100644 .gitignore
```

![.env was committed in the first commit; after adding it to .gitignore it still shows as modified; git rm --cached .env and a commit stop tracking it, but the old commit still has the password](images/04-everyday-commits-and-gitignore/ignore-too-late.gif)

The first commit still contains the password. If the project was ever on GitHub, change the password.

---

### Mistake 4: Naming the file gitignore.txt

The file must be called exactly `.gitignore`. Some editors, like Notepad, add `.txt` to the name. Create it in VS Code.

---

### Mistake 5: One huge commit with everything

A commit that changes ten unrelated things is hard to read and hard to undo. Stage and commit one change at a time.

---

## Practice Exercises

### Exercise 1

Change a line in `notes.txt`. Run `git diff` and find the `-` line and the `+` line. Then run `git add notes.txt` and `git diff --staged`.

### Exercise 2

Make two unrelated changes, in two different files, and commit them as two separate commits.

### Exercise 3

Rename a file in VS Code. Run `git status` before and after `git add .`. What changes?

### Exercise 4

Create a file `.env` and a file `debug.log`. Check that `git status` shows them, then write a `.gitignore` that hides both, and commit the `.gitignore`.

### Exercise 5

Rewrite these commit messages so they say what the commit does: `update`, `fix bug`, `stuff`.

---

## Interview Questions

### What does git diff show?

The changes in tracked files that are not staged yet, line by line: lines with `-` were removed, lines with `+` were added.

### What is the difference between git diff and git diff --staged?

`git diff` compares your files with the staging area: the changes you have not staged. `git diff --staged` compares the staging area with the last commit: what the next commit will contain.

### What is a .gitignore file?

A file that lists names and patterns of files Git should not track, like `.env`, `node_modules/` and `*.log`.

### Why should node_modules not be committed?

`npm install` can create it again at any time from `package.json`, and it contains thousands of files. Committing it only makes the repository huge.

### A file is already committed. Does adding it to .gitignore stop tracking it?

No. `.gitignore` only works for untracked files. Run `git rm --cached <file>` and commit. The file stays in the old commits.

---

| Previous | All sessions | Next |
| :--- | :---: | ---: |
| [03. Your First Repository](03-your-first-repository.md) | [Course outline](README.md#course-outline) | [05. Looking at History](05-looking-at-history.md) |
