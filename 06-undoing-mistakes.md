# Undoing Mistakes

> **Session 6 of 12** | Part 1: Git on Your Computer | [All sessions](README.md#course-outline)

## Table of Contents
- [Where You Left Off](#where-you-left-off)
- [Throw Away Changes in a File](#throw-away-changes-in-a-file)
- [Get Back a Deleted File](#get-back-a-deleted-file)
- [Unstage a File](#unstage-a-file)
- [Fix the Last Commit's Message](#fix-the-last-commits-message)
- [Add a Forgotten File to the Last Commit](#add-a-forgotten-file-to-the-last-commit)
- [Undo the Last Commit, Keep the Changes](#undo-the-last-commit-keep-the-changes)
- [Undo Any Commit Safely: git revert](#undo-any-commit-safely-git-revert)
- [Which Command Do I Need?](#which-command-do-i-need)
- [Beginner Mistakes](#beginner-mistakes)
- [Practice Exercises](#practice-exercises)
- [Interview Questions](#interview-questions)

---

## Where You Left Off

Everybody makes mistakes. With Git, almost every mistake can be undone, if you know which command to use. Open `my-notes`

```bash
git log --oneline -3
```

```text
66ccd5b (HEAD -> main) Add a .gitignore file
28eebaf Rename todo.txt to plan.txt
4526919 Change the notes title to this week
```

---

## Throw Away Changes in a File

Add a line to `notes.txt` that you do not want, `- this line is a mistake`, and save the file

```bash
git status
```

```text
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   notes.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

The second hint tells you how to throw the change away

```bash
git restore notes.txt
git status
```

```text
On branch main
nothing to commit, working tree clean
```

The line is gone. `notes.txt` is exactly as it was in the last commit.

![notes.txt has an extra line you do not want; git restore notes.txt removes it, and git status says working tree clean](images/06-undoing-mistakes/restore-file.gif)

Be careful: a change that was never committed cannot be brought back. Use `git restore` only when you are sure.

---

## Get Back a Deleted File

Delete `plan.txt` in VS Code (right-click, **Delete**)

```bash
git status
```

```text
On branch main
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    plan.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

The same command brings it back

```bash
git restore plan.txt
git status
```

```text
On branch main
nothing to commit, working tree clean
```

`plan.txt` is back, as it was in the last commit. This works for every file that is in a commit.

---

## Unstage a File

Add a line to `notes.txt`, `- git restore throws away changes`. Also create a file `scratch.txt` with anything in it, for your own tests. Then stage everything

```bash
git add .
git status
```

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   notes.txt
	new file:   scratch.txt
```

`scratch.txt` should not be in the commit. Take it out of the staging area

```bash
git restore --staged scratch.txt
git status
```

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   notes.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	scratch.txt
```

![Both files are staged; git restore --staged scratch.txt moves scratch.txt back to the working folder as untracked, while notes.txt stays staged](images/06-undoing-mistakes/restore-staged.gif)

`--staged` only changes the staging area: `scratch.txt` still exists. You do not need it, so delete it in VS Code.

| Command                          | Changes the staging area | Changes your file             |
| -------------------------------- | ------------------------ | ----------------------------- |
| `git restore --staged <file>`    | Yes                      | No                            |
| `git restore <file>`             | No                       | Yes: back to the last commit  |

---

## Fix the Last Commit's Message

Commit, with a typo in the message

```bash
git commit -m "Add a note abuot git restore"
```

```text
[main fb4c496] Add a note abuot git restore
 1 file changed, 1 insertion(+)
```

Fix it with `--amend`

```bash
git commit --amend -m "Add a note about git restore"
```

```text
[main d882cf4] Add a note about git restore
 Date: Wed Nov 4 09:00:00 2026 +0530
 1 file changed, 1 insertion(+)
```

```bash
git log --oneline -3
```

```text
d882cf4 (HEAD -> main) Add a note about git restore
66ccd5b Add a .gitignore file
28eebaf Rename todo.txt to plan.txt
```

The commit with the typo is gone from the log. `--amend` does not edit the old commit: it **replaces** it with a new one, with a new ID. The `Date` line shows when the original commit was made.

![The last commit fb4c496 has a typo in its message; git commit --amend replaces it with d882cf4, which has the fixed message](images/06-undoing-mistakes/amend.gif)

---

## Add a Forgotten File to the Last Commit

Add a line to `plan.txt`, `- Learn how to undo mistakes`. It belongs in the last commit too

```bash
git add plan.txt
git commit --amend --no-edit
```

```text
[main b708cbe] Add a note about git restore
 Date: Wed Nov 4 09:00:00 2026 +0530
 2 files changed, 2 insertions(+)
```

`--no-edit` keeps the message as it is. The commit now changes two files

```bash
git log --oneline -3
```

```text
b708cbe (HEAD -> main) Add a note about git restore
66ccd5b Add a .gitignore file
28eebaf Rename todo.txt to plan.txt
```

Only amend commits that you have **not pushed to GitHub yet** (Session 09). Other people may already have the old commit, and then your histories no longer match.

---

## Undo the Last Commit, Keep the Changes

Sometimes you commit too early. Add a line to `notes.txt`, `- git commit --amend fixes the last commit`, and commit it with a message that says nothing

```bash
git add notes.txt
git commit -m "WIP"
```

```text
[main 8e7182a] WIP
 1 file changed, 1 insertion(+)
```

(`WIP` means "work in progress". It tells nobody what the commit does.) Undo the commit, and keep the change

```bash
git reset --soft HEAD~1
git status
```

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   notes.txt
```

```bash
git log --oneline -2
```

```text
b708cbe (HEAD -> main) Add a note about git restore
66ccd5b Add a .gitignore file
```

`HEAD~1` is the commit before the last one (Session 05). `--soft` moves `main` back to it and keeps the change staged. Now commit it properly

```bash
git commit -m "Add a note about git commit --amend"
```

```text
[main 200ad57] Add a note about git commit --amend
 1 file changed, 1 insertion(+)
```

![The WIP commit is removed from main by git reset --soft HEAD~1, and its change goes back to the staging area](images/06-undoing-mistakes/reset-soft.gif)

You will also see `git reset --hard` on the internet. It throws away the changes too, and they cannot be brought back. Do not use it while you learn.

---

## Undo Any Commit Safely: git revert

In Session 04 you changed the title of the notes to `this week` (commit `4526919`). Undo that change, and keep everything that came after it. Use the ID from your own `git log`

```bash
git revert --no-edit 4526919
```

```text
Auto-merging notes.txt
[main 06b37e2] Revert "Change the notes title to this week"
 Date: Wed Nov 4 09:20:00 2026 +0530
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```bash
git log --oneline -4
```

```text
06b37e2 (HEAD -> main) Revert "Change the notes title to this week"
200ad57 Add a note about git commit --amend
b708cbe Add a note about git restore
66ccd5b Add a .gitignore file
```

`git revert` makes a **new commit** that does the opposite of the old one. The old commit stays in the history. `Auto-merging notes.txt` means Git combined the undo with the later changes to the same file. Without `--no-edit`, Git opens VS Code with the message already written: save and close the tab.

```bash
git show HEAD
```

```text
commit 06b37e2230eaf99b007706d245ab951116c6c4d8 (HEAD -> main)
Author: Riya Sharma <riya@example.com>
Date:   Wed Nov 4 09:20:00 2026 +0530

    Revert "Change the notes title to this week"
    
    This reverts commit 452691903ac29091fc42362f0c75a419280a1ca3.

diff --git a/notes.txt b/notes.txt
index 4763b3c..1ce1a4c 100644
--- a/notes.txt
+++ b/notes.txt
@@ -1,4 +1,4 @@
-Things I learned this week
+Things I learned today
 - Git saves snapshots called commits
 - git status shows what Git sees
 - git diff shows what changed
```

The title is `today` again, and the lines you added later are still there.

![Commit 4526919 changed the title; git revert adds commit 06b37e2 at the end, which changes it back, and both commits stay in the history](images/06-undoing-mistakes/revert.gif)

Because `git revert` only adds a commit, it is safe even for commits that are already on GitHub.

---

## Which Command Do I Need?

![A table of seven situations and their commands; amend and reset are marked: only for commits that are not pushed yet](images/06-undoing-mistakes/which-command.gif)

| I want to...                                  | Command                                           |
| --------------------------------------------- | ------------------------------------------------- |
| Throw away the changes in a file              | `git restore <file>`                              |
| Get back a deleted file                       | `git restore <file>`                              |
| Unstage a file and keep the changes           | `git restore --staged <file>`                     |
| Fix the message of the last commit            | `git commit --amend -m "..."`                     |
| Add a forgotten file to the last commit       | `git add <file>`, then `git commit --amend --no-edit` |
| Undo the last commit and keep the changes     | `git reset --soft HEAD~1`                         |
| Undo any commit, even one on GitHub           | `git revert <id>`                                 |

---

## Beginner Mistakes

### Mistake 1: Running git restore on work you wanted

The changes are gone for good. Commit often: a commit can always be brought back.

---

### Mistake 2: Mixing up git restore and git restore --staged

`git restore --staged` only unstages. `git restore` without `--staged` throws your changes away. Check the table in [Unstage a File](#unstage-a-file).

---

### Mistake 3: Reverting with unsaved changes

```bash
git revert --no-edit HEAD~1
```

```text
error: Your local changes to the following files would be overwritten by merge:
	notes.txt
Please commit your changes or stash them before you merge.
Aborting
fatal: revert failed
```

`notes.txt` has changes that are not committed, and the revert would overwrite them, so Git stops. Commit your changes first (or throw them away with `git restore`), then revert. The message talks about "merge" and "stash" because revert works like a small merge; you do not need `stash` in this course.

---

### Mistake 4: Changing commits that are already on GitHub

`--amend` and `git reset` replace or remove commits. For commits that you already pushed, use `git revert`.

---

### Mistake 5: Using git reset --hard to clean up

It deletes every change that is not committed, with no way back. To throw away the changes in one file, use `git restore <file>`.

---

## Practice Exercises

### Exercise 1

Add a line to a file, then throw it away with `git restore`. Check with `git status`.

### Exercise 2

Delete a file in VS Code and get it back with `git restore`.

### Exercise 3

Change two files and stage both with `git add .`. Unstage one of them with `git restore --staged`.

### Exercise 4

Make a commit with a typo in the message, and fix it with `git commit --amend`. Compare the IDs before and after.

### Exercise 5

Revert one of your older commits. Look at the new commit with `git show HEAD`. Is the old commit still in `git log --oneline`?

---

## Interview Questions

### What is the difference between git restore and git restore --staged?

`git restore <file>` puts the file back to its last committed version and throws the changes away. `git restore --staged <file>` only removes the file from the staging area and keeps the changes.

### What does git commit --amend do?

It replaces the last commit with a new one, to change its message or to add forgotten changes. The new commit has a new ID.

### What is the difference between git revert and git reset?

`git revert` adds a new commit that undoes an old one, so the history stays and it is safe for pushed commits. `git reset` moves the branch back and removes commits from it, so use it only for commits nobody else has.

### What does git reset --soft HEAD~1 do?

It undoes the last commit and keeps its changes in the staging area.

### Why should you not amend a commit that is already pushed?

Amend creates a new commit with a new ID. Other people already have the old commit, so the histories no longer match.

---

| Previous | All sessions | Next |
| :--- | :---: | ---: |
| [05. Looking at History](05-looking-at-history.md) | [Course outline](README.md#course-outline) | [07. Branches](07-branches.md) |
