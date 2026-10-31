# Putting Your Project on GitHub

> **Session 9 of 12** | Part 3: GitHub | [All sessions](README.md#course-outline)

## Table of Contents
- [Create a GitHub Account](#create-a-github-account)
- [Create an Empty Repository on GitHub](#create-an-empty-repository-on-github)
- [Connect Your Repository: git remote](#connect-your-repository-git-remote)
- [Signing In](#signing-in)
- [Push for the First Time](#push-for-the-first-time)
- [See Your Project on GitHub](#see-your-project-on-github)
- [Push New Commits](#push-new-commits)
- [Keep Your Email Private](#keep-your-email-private)
- [Beginner Mistakes](#beginner-mistakes)
- [Practice Exercises](#practice-exercises)
- [Interview Questions](#interview-questions)

---

## Create a GitHub Account

Skip this if you already have an account.

1. Open [github.com](https://github.com) and click **Sign up**
2. Use the **same email** as in your Git settings (`git config --global user.email`, Session 02), so GitHub can connect your commits to you
3. Choose a password and a **username**. Your username is public and becomes part of every link to your projects: `github.com/your-username`
4. Confirm your email address

A free account is all you need for this course.

---

## Create an Empty Repository on GitHub

On GitHub, click **+** at the top right, then **New repository**

1. **Repository name**: `my-notes`
2. **Public** or **Private**: either works. Public means anyone can see it
3. Do **not** add a README, a `.gitignore` or a license. Your project already has commits, and the repository on GitHub must start empty
4. Click **Create repository**

![The New repository page: owner your-username, name my-notes, public or private, and README, .gitignore and license all left off, then Create repository](images/09-putting-your-project-on-github/new-repo.gif)

GitHub then shows a page with the commands for your project. In this course, `your-username` stands for your GitHub username

![The page of the empty repository: its URL, and the commands to push an existing repository from the command line](images/09-putting-your-project-on-github/quick-setup.gif)

You already have the `main` branch (Session 02), so the second command, `git branch -M main`, changes nothing. The next two sections run the other two.

---

## Connect Your Repository: git remote

Open `my-notes` in VS Code. Tell Git where the copy on GitHub is, with the URL from GitHub's page

```bash
git remote add origin https://github.com/your-username/my-notes.git
```

It prints nothing. Check it

```bash
git remote -v
```

```text
origin	https://github.com/your-username/my-notes.git (fetch)
origin	https://github.com/your-username/my-notes.git (push)
```

A **remote** is another copy of your repository, and `origin` is its name: a short name for that long URL. `origin` is only the usual name, not a special word. `fetch` and `push` show that Git uses the same URL to get commits and to send them.

![git remote add origin connects my-notes on your computer to my-notes on GitHub; origin is a short name for the URL](images/09-putting-your-project-on-github/remote-origin.gif)

---

## Signing In

The first time you push, GitHub needs to know that you are you.

* **Windows**: Git Credential Manager, which came with Git (Session 02), opens a window by itself. Choose to sign in with your browser, and authorize it on GitHub
* **macOS and Linux**: install the [GitHub CLI](https://cli.github.com/) and run `gh auth login` once. Choose **GitHub.com**, then **HTTPS**, answer **Yes** to authenticate Git with your GitHub credentials, and log in with your web browser

Git remembers you after that, so later pushes do not ask again. Your GitHub **password** does not work in the terminal: GitHub stopped accepting passwords there in 2021.

![The first git push opens a sign-in window; you sign in with your browser, authorize the app, and Git remembers it](images/09-putting-your-project-on-github/sign-in.gif)

---

## Push for the First Time

```bash
git push -u origin main
```

Git first prints a few progress lines, like `Enumerating objects` and `Writing objects`, with numbers that depend on your project. Then

```text
To https://github.com/your-username/my-notes.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

| Part                                        | Meaning                                                  |
| ------------------------------------------- | -------------------------------------------------------- |
| `git push`                                  | Send your commits to a remote                            |
| `origin main`                               | To the remote `origin`, the branch `main`                |
| `-u`                                        | Remember this, so that later a plain `git push` is enough |
| `* [new branch]      main -> main`          | GitHub did not have `main` yet, and now it does          |
| `branch 'main' set up to track 'origin/main'` | Your `main` is now connected to GitHub's `main` (that is what `-u` did) |

![The commits on your computer are copied to GitHub, and the terminal shows To https://github.com/your-username/my-notes.git and main -> main](images/09-putting-your-project-on-github/first-push.gif)

```bash
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

`origin/main` is your computer's record of where `main` is on GitHub. **Up to date** means GitHub has every commit you have

```bash
git log --oneline -2
```

```text
779725a (HEAD -> main, origin/main) Put the notes on GitHub today
a80363c Merge branch 'change-goal'
```

---

## See Your Project on GitHub

Open `github.com/your-username/my-notes` in your browser. You see

* your files: `notes.txt`, `plan.txt`, `commands.txt`, `goals.txt` and `.gitignore`
* **not** `.env`, `debug.log` or `node_modules`, because `.gitignore` kept them out (Session 04)
* the number of commits: click it to see the whole history, the same as `git log`
* the newest commit's message next to each file

---

## Push New Commits

From now on, your daily work has one more step. Add a line to `notes.txt`, `- git push sends my commits to GitHub`, and commit it

```bash
git add notes.txt
git commit -m "Add a note about git push"
```

```text
[main ab8997b] Add a note about git push
 1 file changed, 1 insertion(+)
```

```bash
git status
```

```text
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

**Ahead by 1 commit**: you have a commit that GitHub does not have yet. Push it. Thanks to `-u`, no more `origin main` is needed

```bash
git push
```

```text
To https://github.com/your-username/my-notes.git
   779725a..ab8997b  main -> main
```

`779725a..ab8997b` means GitHub's `main` moved from `779725a` to `ab8997b`.

```bash
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

![git commit, then git status says ahead of origin/main by 1 commit, git push sends it, and git status says up to date](images/09-putting-your-project-on-github/ahead-push.gif)

```bash
git log --oneline -3
```

```text
ab8997b (HEAD -> main, origin/main) Add a note about git push
779725a Put the notes on GitHub today
a80363c Merge branch 'change-goal'
```

Edit, `git add`, `git commit`, `git push`: that is the everyday cycle with GitHub.

---

## Keep Your Email Private

Every commit contains your email, and in a public repository anyone can read it. If you do not want that, GitHub gives you a private address: on GitHub, open **Settings > Emails** and turn on **Keep my email addresses private**. GitHub shows an address that ends in `@users.noreply.github.com`. Use it in Git

```bash
git config --global user.email "your-id+your-username@users.noreply.github.com"
```

Copy the exact address from that GitHub page. It only changes your future commits.

---

## Beginner Mistakes

### Mistake 1: Creating the GitHub repository with a README

The repository on GitHub then has a commit that your computer does not have, and your first push is refused. For a new project, the simplest fix is to delete that repository on GitHub (**Settings > Delete this repository**) and create it again, empty. Session 10 shows how to get commits from GitHub.

---

### Mistake 2: Forgetting -u on the first push

```bash
git push
```

```text
fatal: The current branch main has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin main

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.
```

Git does not know yet where `main` should go. Run `git push -u origin main` once; after that, `git push` is enough.

---

### Mistake 3: Adding origin twice

```bash
git remote add origin https://github.com/your-username/my-notes.git
```

```text
error: remote origin already exists.
```

`origin` is already there. Check it with `git remote -v`.

---

### Mistake 4: A typo in the URL

In another project, the URL ended in `my-note.git` instead of `my-notes.git`

```bash
git push -u origin main
```

```text
remote: Repository not found.
fatal: repository 'https://github.com/your-username/my-note.git/' not found
```

Fix the URL of `origin`, and check it

```bash
git remote set-url origin https://github.com/your-username/my-notes.git
git remote -v
```

```text
origin	https://github.com/your-username/my-notes.git (fetch)
origin	https://github.com/your-username/my-notes.git (push)
```

The same message appears when the repository is private and you are signed in as somebody else.

---

### Mistake 5: Typing your GitHub password

When the terminal asks for a password, your GitHub password does not work. Sign in the way [Signing In](#signing-in) describes.

---

## Practice Exercises

### Exercise 1

Create a GitHub account, if you do not have one, and an empty repository `my-notes`.

### Exercise 2

Connect your `my-notes` with `git remote add origin ...`, check it with `git remote -v`, and push with `git push -u origin main`.

### Exercise 3

Open your repository on GitHub. Find your commits, and check that `.env` is not there.

### Exercise 4

Change a file, commit it, and run `git status` before and after `git push`. What changes?

### Exercise 5

On GitHub, click a file, then **History**. Compare it with `git log --oneline` for that file.

---

## Interview Questions

### What is a remote in Git?

Another copy of the repository, usually on a server like GitHub. Its name, often `origin`, stands for its URL.

### What does git push -u origin main do?

It sends the commits of `main` to the remote `origin`, and with `-u` it remembers that `main` belongs with `origin/main`, so later a plain `git push` works.

### What is origin/main?

Your computer's record of where `main` is on the remote `origin`. Git updates it when you push or get new commits.

### What does "Your branch is ahead of 'origin/main' by 1 commit" mean?

You have one commit that GitHub does not have yet. `git push` sends it.

### Why does the terminal not accept your GitHub password?

GitHub stopped accepting account passwords for Git in 2021. You sign in through the browser, with Git Credential Manager or the GitHub CLI, or with a token.

---

| Previous | All sessions | Next |
| :--- | :---: | ---: |
| [08. Merge Conflicts](08-merge-conflicts.md) | [Course outline](README.md#course-outline) | [10. Clone, Pull and Fetch](10-clone-pull-and-fetch.md) |
