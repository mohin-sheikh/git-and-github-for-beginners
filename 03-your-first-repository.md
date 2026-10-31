# Your First Repository

> **Session 3 of 12** | Part 1: Git on Your Computer | [All sessions](README.md#course-outline)

## Table of Contents
- [Your Practice Folder](#your-practice-folder)
- [Turn the Folder into a Repository](#turn-the-folder-into-a-repository)
- [Ask Git What It Sees](#ask-git-what-it-sees)
- [Add Your First File](#add-your-first-file)
- [The Three Places Your Files Live](#the-three-places-your-files-live)
- [Stage the File with git add](#stage-the-file-with-git-add)
- [Make Your First Commit](#make-your-first-commit)
- [Change the File and Commit Again](#change-the-file-and-commit-again)
- [The Everyday Cycle](#the-everyday-cycle)
- [Beginner Mistakes](#beginner-mistakes)
- [Practice Exercises](#practice-exercises)
- [Interview Questions](#interview-questions)

---

## Your Practice Folder

1. Make a folder called `git-practice` for this course, for example in your **Documents** folder
2. Inside it, make a folder called `my-notes`
3. Open `my-notes` in VS Code (**File > Open Folder**)
4. Open a terminal (**Terminal > New Terminal**). It starts inside `my-notes`

In this course the practice folder is `D:\git-practice`. Your path will be different, and that is fine.

---

## Turn the Folder into a Repository

```bash
git init
```

```text
Initialized empty Git repository in D:/git-course-tools/sandbox/03/git-practice/my-notes/.git/
```

`git init` creates a hidden folder called `.git` inside `my-notes`. That is where Git keeps the whole history of the project. `my-notes` is now a **repository**.

![git init adds a hidden .git folder inside my-notes, and my-notes becomes a repository](images/03-your-first-repository/init-folder.gif)

VS Code hides the `.git` folder, and you never need to open it. Never edit or delete it: deleting `.git` deletes every commit.

Run `git init` once for every project, inside the project's folder.

---

## Ask Git What It Sees

```bash
git status
```

```text
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

| Line                 | Meaning                                         |
| -------------------- | ----------------------------------------------- |
| `On branch main`     | You are on the branch `main` (Session 02)       |
| `No commits yet`     | The repository is new                           |
| `nothing to commit`  | There are no files yet                          |

`git status` is the command you will use most. It never changes anything, so run it whenever you are not sure what is going on.

---

## Add Your First File

In VS Code, create a file called `notes.txt` in `my-notes` with these two lines, and save it (**Ctrl+S**)

notes.txt

```txt
Things I learned today
- Git saves snapshots called commits
```

Ask Git again

```bash
git status
```

```text
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	notes.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Git sees the new file, but it is **untracked**: Git has never saved it. In the terminal, `notes.txt` is red.

---

## The Three Places Your Files Live

Before the first commit, you need to know where a file can be

![notes.txt starts in the working folder as untracked, git add puts a copy in the staging area, and git commit saves it as commit 22a462d in the repository](images/03-your-first-repository/three-areas.gif)

| Place              | What is in it                                   | How things get there     |
| ------------------ | ----------------------------------------------- | ------------------------ |
| Working folder     | Your files, as you see them in VS Code          | You edit and save        |
| Staging area       | The changes that go into the next commit        | `git add`                |
| Repository         | All your commits, inside `.git`                 | `git commit`             |

Why is there a staging area? So you choose exactly what goes into each commit. If you changed three files but only two belong together, you stage only those two (Session 04).

---

## Stage the File with git add

```bash
git add notes.txt
```

It prints nothing. Check with `git status`

```bash
git status
```

```text
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   notes.txt
```

`notes.txt` is now under **Changes to be committed**, in green: it will be in the next commit. The line in brackets is a hint for undoing this; you do not need it now (Session 06).

![git status at three moments: the untracked file in red, the staged file in green, and working tree clean after the commit](images/03-your-first-repository/status-colors.gif)

---

## Make Your First Commit

```bash
git commit -m "Add my first notes"
```

```text
[main (root-commit) 22a462d] Add my first notes
 1 file changed, 2 insertions(+)
 create mode 100644 notes.txt
```

`-m` stands for **message**. The output tells you what Git saved

![The first line of the commit output with its parts highlighted: the branch main, root-commit, the commit ID 22a462d and the message](images/03-your-first-repository/commit-output.gif)

| Part                                 | Meaning                                              |
| ------------------------------------ | ---------------------------------------------------- |
| `main`                               | The branch you are on                                |
| `root-commit`                        | The very first commit of this repository             |
| `22a462d`                            | The commit ID. Yours is different                    |
| `Add my first notes`                 | Your message                                         |
| `1 file changed, 2 insertions(+)`    | One file, two new lines                              |
| `create mode 100644 notes.txt`       | `notes.txt` is a new file (`100644` means a normal file) |

If you leave out `-m`, Git opens VS Code for the message (Session 02).

Check again

```bash
git status
```

```text
On branch main
nothing to commit, working tree clean
```

**Working tree clean** means everything in the folder is saved in a commit. Here is a first look at the history (Session 05 explains it)

```bash
git log --oneline
```

```text
22a462d Add my first notes
```

---

## Change the File and Commit Again

Add a third line to `notes.txt` and save it

notes.txt

```txt
Things I learned today
- Git saves snapshots called commits
- git status shows what Git sees
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

no changes added to commit (use "git add" and/or "git commit -a")
```

The file is now **modified**: Git saved it before, and it has changed since the last commit. Stage it and commit, the same way as before

```bash
git add notes.txt
git commit -m "Add a note about git status"
```

```text
[main 57d1341] Add a note about git status
 1 file changed, 1 insertion(+)
```

This time there is no `root-commit` and no `create mode`, because the file already existed.

```bash
git log --oneline
```

```text
57d1341 Add a note about git status
22a462d Add my first notes
```

Two commits. The newest one is at the top.

---

## The Everyday Cycle

You have just done what developers do many times every day

![A circle of three steps, edit files, git add, git commit, with git status in the middle to check at any time](images/03-your-first-repository/everyday-cycle.gif)

1. Edit and save your files
2. `git add` the files that belong in the next commit
3. `git commit -m "..."` with a short message
4. Run `git status` whenever you are not sure

| Word        | Meaning                                           |
| ----------- | ------------------------------------------------- |
| Untracked   | A new file that Git has never saved               |
| Modified    | A saved file that changed since the last commit   |
| Staged      | Goes into the next commit                         |
| Committed   | Saved in a commit                                 |

---

## Beginner Mistakes

### Mistake 1: Committing without git add

```bash
git commit -m "Add my first notes"
```

```text
On branch main

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	notes.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Nothing was staged, so no commit was made. Run `git add` first.

![git commit without git add says nothing added to commit; after git add notes.txt, the same commit works](images/03-your-first-repository/forgot-add.gif)

---

### Mistake 2: Forgetting to add a changed file

A file that is already in a commit must be added again after every change

```bash
git commit -m "Add a note about git status"
```

```text
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   notes.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Run `git add notes.txt`, then commit.

---

### Mistake 3: Running Git outside the repository

```bash
git status
```

```text
fatal: not a git repository (or any of the parent directories): .git
```

The terminal is not inside `my-notes`. Open the folder in VS Code (**File > Open Folder**) and open a new terminal.

---

### Mistake 4: Running git init in the wrong folder

If you run `git init` in your home folder or on your Desktop, Git treats everything in it as one project, and `git status` lists hundreds of files. Delete the `.git` folder that you just made there, and only that one, then run `git init` in the right folder. To see hidden folders in File Explorer, use **View > Show > Hidden items**.

---

### Mistake 5: Deleting the .git folder

Your files stay, but every commit is gone. Never delete `.git` in a project you want to keep.

---

## Practice Exercises

### Exercise 1

Create `git-practice/my-notes`, run `git init` inside it, and run `git status`.

### Exercise 2

Create `notes.txt` with two lines. Run `git status` after each step: before `git add`, after `git add`, and after `git commit`.

### Exercise 3

Add a line to `notes.txt`, then stage it and commit it. Run `git log --oneline`: you should see two commits.

### Exercise 4

Create a second file, `todo.txt`, and commit it with the message `Add a todo list`. How many commits does `git log --oneline` show now?

### Exercise 5

Run `git commit -m "Test"` when nothing has changed. What does Git say?

---

## Interview Questions

### What does git init do?

It turns the current folder into a Git repository by creating a hidden `.git` folder, where Git keeps the history.

### What does git status show?

The current branch, and which files are untracked, modified or staged. It never changes anything.

### What is the staging area?

The changes that will go into the next commit. `git add` puts changes there, and `git commit` saves them as a commit.

### What is the difference between an untracked and a modified file?

An untracked file is new: Git has never saved it. A modified file was saved in a commit before and has changed since then.

### Why does Git have a staging area?

So you can choose what goes into each commit and keep commits small and focused, even when you changed many files.

---

| Previous | All sessions | Next |
| :--- | :---: | ---: |
| [02. Installing and Setting Up Git](02-installing-and-setting-up-git.md) | [Course outline](README.md#course-outline) | [04. Everyday Commits and .gitignore](04-everyday-commits-and-gitignore.md) |
