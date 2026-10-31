# Merge Conflicts

> **Session 8 of 12** | Part 2: Branches | [All sessions](README.md#course-outline)

## Table of Contents
- [What a Merge Conflict Is](#what-a-merge-conflict-is)
- [Two Branches Change the Same Line](#two-branches-change-the-same-line)
- [The Conflict](#the-conflict)
- [What Git Wrote into the File](#what-git-wrote-into-the-file)
- [Fix It in VS Code](#fix-it-in-vs-code)
- [Finish the Merge](#finish-the-merge)
- [Cancel a Merge: git merge --abort](#cancel-a-merge-git-merge---abort)
- [How to Get Fewer Conflicts](#how-to-get-fewer-conflicts)
- [Beginner Mistakes](#beginner-mistakes)
- [Practice Exercises](#practice-exercises)
- [Interview Questions](#interview-questions)

---

## What a Merge Conflict Is

In Session 07, Git merged two branches by itself, because they changed different files. When two branches change **the same line** of the same file, Git cannot know which version is right. That is a **merge conflict**: Git stops and asks you.

![main and change-goal both change line 1 of goals.txt; git merge cannot choose between them and reports a conflict](images/08-merge-conflicts/why-conflict.gif)

A conflict is not an error, and nothing is lost. It is a question that only you can answer.

---

## Two Branches Change the Same Line

Open `my-notes`. This is `goals.txt` now

```bash
cat goals.txt
```

```text
- Use Git every day
- Make my first pull request
```

Make a branch, and change the first line to `- Use Git for every project`

```bash
git switch -c change-goal
```

```text
Switched to a new branch 'change-goal'
```

```bash
git add goals.txt
git commit -m "Use Git for every project"
```

```text
[change-goal 27fcae9] Use Git for every project
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Go back to `main`, and change the **same line** in a different way: `- Use Git every single day`

```bash
git switch main
```

```text
Switched to branch 'main'
```

```bash
git add goals.txt
git commit -m "Use Git every single day"
```

```text
[main 2dbcb8c] Use Git every single day
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```bash
git log --oneline --graph --all -3
```

```text
* 2dbcb8c (HEAD -> main) Use Git every single day
| * 27fcae9 (change-goal) Use Git for every project
|/  
*   35ceef5 Merge branch 'add-goals'
|\
```

---

## The Conflict

```bash
git merge change-goal
```

```text
Auto-merging goals.txt
CONFLICT (content): Merge conflict in goals.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Git stopped in the middle of the merge, because of `goals.txt`

```bash
git status
```

```text
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   goals.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`both modified` means both branches changed this file. The hints show the two ways forward: fix the conflict and commit, or cancel the merge.

---

## What Git Wrote into the File

```bash
cat goals.txt
```

```text
<<<<<<< HEAD
- Use Git every single day
=======
- Use Git for every project
>>>>>>> change-goal
- Make my first pull request
```

Git put both versions into the file and marked them

![goals.txt during the conflict: main's line between <<<<<<< HEAD and =======, change-goal's line between ======= and >>>>>>> change-goal, and the last line without a conflict](images/08-merge-conflicts/conflict-markers.gif)

| Marker line              | Meaning                                                 |
| ------------------------ | ------------------------------------------------------- |
| `<<<<<<< HEAD`           | Your version starts: the branch you are on (`main`)     |
| `=======`                | The line between the two versions                       |
| `>>>>>>> change-goal`    | The other version ends: the branch you merge in         |

Lines outside the markers have no conflict.

---

## Fix It in VS Code

Open `goals.txt` in VS Code. It colors both versions and shows buttons above them

![VS Code shows the current change in green and the incoming change in blue, with the buttons Accept Current, Accept Incoming, Accept Both and Compare; each choice gives a different result](images/08-merge-conflicts/vscode-buttons.gif)

| Button                    | Result                                          |
| ------------------------- | ----------------------------------------------- |
| Accept Current Change     | Keeps `main`'s line                             |
| Accept Incoming Change    | Keeps `change-goal`'s line                      |
| Accept Both Changes       | Keeps both lines, one after the other           |
| Compare Changes           | Shows the two versions side by side             |

You can also write the result yourself. Here, the best answer combines both ideas. Replace everything from `<<<<<<<` to `>>>>>>>` with one line, and save

goals.txt

```txt
- Use Git every day, for every project
- Make my first pull request
```

When you edit by hand, delete all three marker lines yourself. For bigger conflicts, VS Code also has a **Resolve in Merge Editor** button.

---

## Finish the Merge

Tell Git that the conflict is fixed, by staging the file

```bash
git add goals.txt
git status
```

```text
On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
	modified:   goals.txt
```

Commit to finish the merge. `--no-edit` keeps the message that Git prepared

```bash
git commit --no-edit
```

```text
[main a80363c] Merge branch 'change-goal'
```

```bash
git log --oneline --graph -5
```

```text
*   a80363c (HEAD -> main) Merge branch 'change-goal'
|\  
| * 27fcae9 (change-goal) Use Git for every project
* | 2dbcb8c Use Git every single day
|/  
*   35ceef5 Merge branch 'add-goals'
|\  
| * b903523 Add my learning goals
```

```bash
git branch -d change-goal
```

```text
Deleted branch change-goal (was 27fcae9).
```

![Five steps from conflict to merge commit: git status, fix goals.txt, no markers left, git add, git commit --no-edit](images/08-merge-conflicts/resolve-steps.gif)

---

## Cancel a Merge: git merge --abort

Sometimes you cannot fix a conflict right now, for example because you need to ask a teammate first. Make a second conflict: a branch `reword-plan` changes line 2 of `plan.txt` to `- Put all my notes on GitHub` and commits it, and `main` changes the same line to `- Put my notes on GitHub today` and commits it. Then

```bash
git merge reword-plan
```

```text
Auto-merging plan.txt
CONFLICT (content): Merge conflict in plan.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Cancel the merge

```bash
git merge --abort
git status
```

```text
On branch main
nothing to commit, working tree clean
```

Everything is as it was before `git merge`. The branch `reword-plan` still exists, so you could merge it later. You do not need it in this course: delete it with `git branch -D reword-plan` (Session 07).

![A merge stops with a conflict in plan.txt; git merge --abort brings back the state from before the merge, and git status says working tree clean](images/08-merge-conflicts/merge-abort.gif)

---

## How to Get Fewer Conflicts

![Four tips: keep branches small, merge main into your branch often, one task per branch, and talk to your team](images/08-merge-conflicts/avoid-conflicts.gif)

* Keep branches small, and merge them soon
* Merge `main` into your branch often: `git switch my-branch`, then `git merge main`
* Do one task per branch
* Tell your team which files you are working on

---

## Beginner Mistakes

### Mistake 1: Committing before the conflict is fixed

```bash
git commit --no-edit
```

```text
error: Committing is not possible because you have unmerged files.
hint: Fix them up in the work tree, and then use 'git add/rm <file>'
hint: as appropriate to mark resolution and make a commit.
fatal: Exiting because of an unresolved conflict.
U	goals.txt
```

Fix the file and `git add` it first. Then commit.

---

### Mistake 2: Switching branches in the middle of a merge

```bash
git switch change-goal
```

```text
fatal: cannot switch branch while merging
Consider "git merge --quit" or "git worktree add".
```

Finish the merge (fix, `git add`, `git commit`) or cancel it with `git merge --abort`. The two commands that Git suggests are for special cases; you do not need them.

---

### Mistake 3: Leaving marker lines in the file

`git add` does not check the file. If you commit it with `<<<<<<<` still inside, the markers become part of your project, and in a code file they break the program. Before `git add`, search the file for `<<<<<<<` (**Ctrl+F** in VS Code).

---

### Mistake 4: Choosing a side without reading

**Accept Current Change** throws the other version away, and with it maybe a teammate's work. Read both versions first. Often the right answer combines them.

---

### Mistake 5: Panicking

Deleting files, or the `.git` folder, does not fix a conflict. Nothing is lost during a merge: `git merge --abort` always brings you back.

---

## Practice Exercises

### Exercise 1

Make a conflict yourself: change the same line on a branch and on `main`, commit both, and merge the branch.

### Exercise 2

Open the file in VS Code and click **Accept Both Changes**. Check the file, then finish the merge with `git add` and `git commit --no-edit`.

### Exercise 3

Make another conflict and cancel it with `git merge --abort`. Check `git status`.

### Exercise 4

On two branches, change **different** lines of the same file, and merge. Is there a conflict?

### Exercise 5

Run `git log --oneline --graph` and find all your merge commits.

---

## Interview Questions

### What is a merge conflict?

Two branches changed the same lines of a file, and Git cannot decide which version to keep. It stops the merge and asks you to decide.

### What do <<<<<<<, ======= and >>>>>>> mean?

They mark the two versions in the file: from `<<<<<<< HEAD` to `=======` is the version of the branch you are on, and from `=======` to `>>>>>>>` is the version of the branch you merge in.

### How do you resolve a merge conflict?

Edit the file so it has the right lines, remove the marker lines, `git add` the file, and `git commit` to finish the merge.

### How do you cancel a merge that has conflicts?

`git merge --abort` puts everything back the way it was before the merge.

### How can a team get fewer merge conflicts?

Small branches that are merged soon, merging `main` into branches often, one task per branch, and telling each other who works on which files.

---

| Previous | All sessions | Next |
| :--- | :---: | ---: |
| [07. Branches](07-branches.md) | [Course outline](README.md#course-outline) | [09. Putting Your Project on GitHub](09-putting-your-project-on-github.md) |
