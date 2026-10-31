# Clone, Pull and Fetch

> **Session 10 of 12** | Part 3: GitHub | [All sessions](README.md#course-outline)

## Table of Contents
- [Two Places, One GitHub](#two-places-one-github)
- [Clone a Repository](#clone-a-repository)
- [A Change on GitHub](#a-change-on-github)
- [git fetch: Ask GitHub What Is New](#git-fetch-ask-github-what-is-new)
- [git pull: Bring the Changes In](#git-pull-bring-the-changes-in)
- [When Both Sides Have New Commits](#when-both-sides-have-new-commits)
- [One Setting for git pull](#one-setting-for-git-pull)
- [Your Daily Routine](#your-daily-routine)
- [Beginner Mistakes](#beginner-mistakes)
- [Practice Exercises](#practice-exercises)
- [Interview Questions](#interview-questions)

---

## Two Places, One GitHub

In real work, a project lives in more than one place: your laptop and an office computer, or your computer and your teammates' computers. None of them talk to each other directly. They all push to GitHub and get new commits from GitHub.

In this session, a second folder, `my-notes-2`, plays the second computer.

![my-notes on your laptop and my-notes-2 on a second computer both push to GitHub and pull from GitHub; GitHub is in the middle](images/10-clone-pull-and-fetch/two-places.gif)

---

## Clone a Repository

To get a repository from GitHub onto a computer, you **clone** it. Open a terminal in `git-practice`, the folder that contains `my-notes`

```bash
git clone https://github.com/your-username/my-notes.git my-notes-2
```

```text
Cloning into 'my-notes-2'...
```

Git also prints a few progress lines, like `Receiving objects`. You see such lines every time Git downloads or uploads (`clone`, `fetch`, `pull` and `push`); this course leaves them out, because their numbers are different every time. The last part of the command, `my-notes-2`, is the name of the new folder. Without it, Git would call the folder `my-notes`, and that one already exists (Mistake 1).

Open `my-notes-2` in a second VS Code window (**File > New Window**, then **File > Open Folder**)

```bash
git log --oneline -3
```

```text
ab8997b (HEAD -> main, origin/main, origin/HEAD) Add a note about git push
779725a Put the notes on GitHub today
a80363c Merge branch 'change-goal'
```

Everything is there: every file, every commit, the branch `main`. `origin/HEAD` points to GitHub's main branch. Clone also connects the new folder to GitHub by itself

```bash
git remote -v
```

```text
origin	https://github.com/your-username/my-notes.git (fetch)
origin	https://github.com/your-username/my-notes.git (push)
```

```bash
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

![git clone downloads my-notes from GitHub into the folder my-notes-2, with the files, every commit and origin set up](images/10-clone-pull-and-fetch/clone.gif)

You do not need `git init` or `git remote add` for a cloned repository. It is ready to use.

---

## A Change on GitHub

You can edit files right on GitHub. In your browser, open your `my-notes` repository and click `goals.txt`

1. Click the pencil icon (**Edit this file**)
2. Add a third line: `- Read other people's code`
3. Click **Commit changes...**, write the message `Add a goal about reading code`, and click **Commit changes**

GitHub now has a commit that neither of your folders has.

---

## git fetch: Ask GitHub What Is New

Go back to the first folder, `my-notes`

```bash
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Up to date? Git does not ask GitHub by itself. `git status` compares with what Git saw the last time it talked to GitHub. Ask GitHub now

```bash
git fetch
```

```text
From https://github.com/your-username/my-notes
   ab8997b..ac44afc  main       -> origin/main
```

```bash
git status
```

```text
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

`git fetch` downloaded the new commit and moved `origin/main`, your record of GitHub's `main`. Your own `main` and your files did not change: **behind by 1 commit** means GitHub has one commit that your `main` does not have yet.

![GitHub's main has the new commit ac44afc; git fetch brings it into origin/main, and git pull also brings it into your main and your files](images/10-clone-pull-and-fetch/fetch-vs-pull.gif)

---

## git pull: Bring the Changes In

```bash
git pull
```

```text
Updating ab8997b..ac44afc
Fast-forward
 goals.txt | 1 +
 1 file changed, 1 insertion(+)
```

```bash
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

```bash
cat goals.txt
```

```text
- Use Git every day, for every project
- Make my first pull request
- Read other people's code
```

The line you wrote on GitHub is in your file. `git pull` fetches and then merges `origin/main` into your `main`. Here only GitHub had new commits, so the merge was a fast-forward (Session 07).

| Command      | Downloads new commits | Changes your branch and files |
| ------------ | --------------------- | ----------------------------- |
| `git fetch`  | Yes                   | No                            |
| `git pull`   | Yes                   | Yes                           |

![git pull equals git fetch plus git merge origin/main; the output shows the fetch part first and the merge part after it](images/10-clone-pull-and-fetch/pull-equation.gif)

Most of the time you just run `git pull`. Use `git fetch` when you only want to see what is new, without changing anything yet.

---

## When Both Sides Have New Commits

Now both folders work at the same time. In `my-notes-2`, run `git pull` first, so it has the GitHub change too. Then add a line to `commands.txt`, `git pull - get new commits`, commit it and push it

```bash
git add commands.txt
git commit -m "Add git pull to the commands"
```

```text
[main ed78357] Add git pull to the commands
 1 file changed, 1 insertion(+)
```

```bash
git push
```

```text
To https://github.com/your-username/my-notes.git
   ac44afc..ed78357  main -> main
```

Meanwhile, in `my-notes`, add a line to `plan.txt`, `- Learn pull requests`, and commit it

```bash
git add plan.txt
git commit -m "Add pull requests to the plan"
```

```text
[main e445a31] Add pull requests to the plan
 1 file changed, 1 insertion(+)
```

```bash
git push
```

```text
To https://github.com/your-username/my-notes.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/your-username/my-notes.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

GitHub refuses the push. It has a commit from `my-notes-2` that this folder does not have, and your push would throw it away. The hint says what to do: pull first

```bash
git pull --no-edit
```

```text
From https://github.com/your-username/my-notes
   ac44afc..ed78357  main       -> origin/main
Merge made by the 'ort' strategy.
 commands.txt | 1 +
 1 file changed, 1 insertion(+)
```

The two changes are in different files, so Git merged them by itself with a merge commit (Session 07). Without `--no-edit`, VS Code opens with the message `Merge branch 'main' of https://github.com/...` already written: save and close the tab. If both sides had changed the same line, you would get a conflict and fix it as in Session 08.

```bash
git log --oneline --graph -5
```

```text
*   4488d8c (HEAD -> main) Merge branch 'main' of https://github.com/your-username/my-notes
|\  
| * ed78357 (origin/main, origin/HEAD) Add git pull to the commands
* | e445a31 Add pull requests to the plan
|/  
* ac44afc Add a goal about reading code
* ab8997b Add a note about git push
```

Now the push works

```bash
git push
```

```text
To https://github.com/your-username/my-notes.git
   ed78357..4488d8c  main -> main
```

![The push from my-notes is rejected because GitHub has ed78357 from my-notes-2; git pull makes the merge commit 4488d8c, and git push then works](images/10-clone-pull-and-fetch/rejected-push.gif)

---

## One Setting for git pull

When both sides have new commits, `git pull` can combine them in two ways: with a merge commit, as above, or with **rebase**, which this course does not cover. Git for Windows already tells Git to merge. On macOS and Linux, tell it once, or `git pull` stops with an error (Mistake 2)

```bash
git config --global pull.rebase false
```

It prints nothing. `false` means: no rebase, merge. On Windows the command does no harm, so you can run it everywhere.

---

## Your Daily Routine

![Three steps: git pull at the start of work, git add and git commit while you work, git push at the end](images/10-clone-pull-and-fetch/daily-routine.gif)

1. **Start of work:** `git pull`, to get what others pushed
2. **While you work:** `git add` and `git commit`, in small steps
3. **End of work:** `git push`, so your commits are not only on one computer

Pulling before you start keeps conflicts small, because your work starts from the newest version.

---

## Beginner Mistakes

### Mistake 1: Cloning into a folder that already exists

```bash
git clone https://github.com/your-username/my-notes.git my-notes-2
```

```text
fatal: destination path 'my-notes-2' already exists and is not an empty directory.
```

Choose another folder name, or clone somewhere else.

---

### Mistake 2: git pull without the pull.rebase setting (macOS and Linux)

Both sides have new commits, and the setting from [One Setting for git pull](#one-setting-for-git-pull) is missing

```bash
git pull
```

```text
From https://github.com/your-username/my-notes
   4488d8c..41b2b63  main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

Git does not know how to combine the commits, so it stops. Set it once, and pull again

```bash
git config --global pull.rebase false
git pull --no-edit
```

```text
Merge made by the 'ort' strategy.
 goals.txt | 1 +
 1 file changed, 1 insertion(+)
```

---

### Mistake 3: Pulling with changes that are not committed

```bash
git pull
```

```text
From https://github.com/your-username/my-notes
   e2080c5..eb8738b  main       -> origin/main
Updating e2080c5..eb8738b
error: Your local changes to the following files would be overwritten by merge:
	notes.txt
Please commit your changes or stash them before you merge.
Aborting
```

The new commit changes `notes.txt`, and you have changed it too without committing. Commit your change, or throw it away, then pull again

```bash
git restore notes.txt
git pull
```

```text
Updating e2080c5..eb8738b
Fast-forward
 notes.txt | 1 +
 1 file changed, 1 insertion(+)
```

---

### Mistake 4: Trusting git status without fetching

`git status` only knows what Git saw the last time it talked to GitHub. "Up to date" can be old news. Run `git fetch` (or `git pull`) first.

---

### Mistake 5: Forcing a rejected push

When you search the internet for the rejected message, you will find `git push --force`. It replaces GitHub's history with yours and **deletes** the commits you did not have, maybe a teammate's work. Never use it on a shared branch. Pull, then push.

---

## Practice Exercises

### Exercise 1

Clone your `my-notes` into a new folder `my-notes-2`. Run `git log --oneline` and `git remote -v` in it.

### Exercise 2

Edit a file on GitHub in the browser. In `my-notes`, run `git status`, then `git fetch`, then `git status` again. What changed?

### Exercise 3

Run `git pull` and check that the change from GitHub is in your file.

### Exercise 4

Commit and push a change from `my-notes-2`. Then commit a change to another file in `my-notes` and try to push. Fix the rejected push with `git pull --no-edit` and push again.

### Exercise 5

Run `git log --oneline --graph` in both folders after they are both up to date. Are the histories the same?

---

## Interview Questions

### What does git clone do?

It downloads a whole repository, with all files, commits and branches, into a new folder, and sets up `origin` to point to where it came from.

### What is the difference between git fetch and git pull?

`git fetch` downloads new commits and updates `origin/main`, but does not change your branch or files. `git pull` does a fetch and then merges the new commits into your branch.

### Why is a push rejected with "fetch first"?

The remote has commits that you do not have. Pushing would remove them, so Git refuses. Pull first, then push.

### What does "Your branch is behind 'origin/main' by 1 commit" mean?

The remote has one commit that your branch does not have yet. `git pull` brings it in.

### What does git config --global pull.rebase false do?

It tells `git pull` to combine your commits and the new ones with a merge commit when both sides have new commits, instead of rebasing.

---

| Previous | All sessions | Next |
| :--- | :---: | ---: |
| [09. Putting Your Project on GitHub](09-putting-your-project-on-github.md) | [Course outline](README.md#course-outline) | [11. Pull Requests](11-pull-requests.md) |
