# Installing and Setting Up Git

> **Session 2 of 12** | Part 1: Git on Your Computer | [All sessions](README.md#course-outline)

## Table of Contents
- [Install Git](#install-git)
- [Check That Git Works](#check-that-git-works)
- [Tell Git Who You Are](#tell-git-who-you-are)
- [Why Git Needs Your Name](#why-git-needs-your-name)
- [Name the First Branch main](#name-the-first-branch-main)
- [Choose VS Code as Git's Editor](#choose-vs-code-as-gits-editor)
- [See All Your Settings](#see-all-your-settings)
- [Changing a Setting](#changing-a-setting)
- [Beginner Mistakes](#beginner-mistakes)
- [Practice Exercises](#practice-exercises)
- [Interview Questions](#interview-questions)

---

## Install Git

Every system installs Git a little differently. Follow the steps for yours.

![Windows downloads Git from git-scm.com, macOS installs it with the developer tools, Linux with apt; then git --version prints the version](images/02-installing-and-setting-up-git/install-paths.gif)

### Windows

1. Open [git-scm.com](https://git-scm.com/), go to **Downloads**, choose **Windows** and download **Git for Windows**
2. Run the installer. It asks many questions: keep the default answer on every screen, click **Next**, and at the end **Install**
3. Close VS Code completely and open it again, so that its terminal can find Git

You choose the editor and the branch name with commands later in this session, so the installer's answers for those do not matter. The installer also adds **Git Credential Manager**, which signs you in to GitHub in Session 09.

### macOS

Open the **Terminal** app and type `git --version`. If Git is missing, macOS offers to install the **command line developer tools**. Click **Install**, wait until it finishes, and run `git --version` again.

### Linux

On Ubuntu or Debian

```bash
sudo apt update
sudo apt install git
```

On Fedora, use `sudo dnf install git`.

---

## Check That Git Works

In a new terminal

```bash
git --version
```

```text
git version 2.55.0.windows.4
```

Your number may be different. Any version from 2.28 up works with this course.

---

## Tell Git Who You Are

Use your own name and email in these two commands

```bash
git config --global user.name "Riya Sharma"
git config --global user.email "riya@example.com"
```

They print nothing when they work. To see what Git saved, ask for each setting without a value

```bash
git config --global user.name
git config --global user.email
```

```text
Riya Sharma
riya@example.com
```

| Part             | Meaning                                                    |
| ---------------- | ---------------------------------------------------------- |
| `git config`     | Read or change a Git setting                               |
| `--global`       | For every project on this computer                         |
| `user.name`      | The name of the setting                                    |
| `"Riya Sharma"`  | The new value. Use quotes when it has a space              |

Use the email of your GitHub account, or the one you will use for it in Session 09. GitHub uses this email to connect your commits to your profile.

---

## Why Git Needs Your Name

Every commit stores who made it (Session 01). Without a name and an email, Git refuses to make a commit. This is what you would see if you skipped the last step (you make your first commit in Session 03)

```text
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'Riya@RIYA-LAPTOP.(none)')
```

Git even shows the two commands that fix it. The last line shows the name of your computer.

![Before the setup, git commit stops with Author identity unknown; after setting the name and email, every commit shows Riya Sharma as its author](images/02-installing-and-setting-up-git/identity.gif)

---

## Name the First Branch main

Every repository starts with one **branch**. Session 07 explains branches; for now, a branch is a line of commits with a name.

Git still names the first branch `master` unless you tell it otherwise. GitHub, and most new projects, call it `main`. Set it once

```bash
git config --global init.defaultBranch main
```

You will see the difference in Session 03, when you create a repository and run `git status`

| Setup                  | The first line of `git status` |
| ---------------------- | ------------------------------ |
| Without this setting   | `On branch master`             |
| With this setting      | `On branch main`               |

![Without the setting, a new repository is on branch master while GitHub uses main; with the setting, both are main](images/02-installing-and-setting-up-git/master-main.gif)

---

## Choose VS Code as Git's Editor

Sometimes Git needs you to write a message, for example when you make a commit without a message (Session 03) or undo a commit (Session 06). Then it opens an **editor**. If you have not chosen one, that editor is usually **Vim**, which runs inside the terminal and is confusing at first.

Tell Git to use VS Code instead

```bash
git config --global core.editor "code --wait"
```

`code` opens VS Code, and `--wait` makes Git wait until you close the tab. When the tab opens

1. Write your message on the first line
2. Save with **Ctrl+S** (**Cmd+S** on macOS)
3. Close the tab

Lines that start with `#` are only hints, and Git ignores them.

![git commit without a message: with no editor set, Vim opens in the terminal; with code --wait, VS Code opens COMMIT_EDITMSG, you write the message, save and close the tab](images/02-installing-and-setting-up-git/editor.gif)

On macOS, VS Code's `code` command must be installed once: in VS Code, open the Command Palette (**Cmd+Shift+P**) and run **Shell Command: Install 'code' command in PATH**.

Stuck in Vim anyway? Press **Esc**, type `:q!` and press **Enter**. Git cancels the commit and says

```text
Aborting commit due to empty commit message.
```

---

## See All Your Settings

You have done everything Git needs. You only do this once on every computer.

![A checklist of five steps with their commands: Git installed, your name, your email, the first branch main, and VS Code as the editor](images/02-installing-and-setting-up-git/setup-checklist.gif)

To see all your settings at once

```bash
git config --global --list
```

```text
user.name=Riya Sharma
user.email=riya@example.com
init.defaultbranch=main
core.editor=code --wait
```

The list writes `defaultbranch` in small letters. That is fine: capitals do not matter in setting names.

Git keeps these settings in a file called `.gitconfig` in your home folder: `C:\Users\<your name>\.gitconfig` on Windows, `~/.gitconfig` on macOS and Linux. Inside, it looks like this

```text
[user]
	name = Riya Sharma
	email = riya@example.com
[init]
	defaultBranch = main
[core]
	editor = code --wait
```

![Four git config --global commands save their settings into the .gitconfig file, and every project on the computer uses that one file](images/02-installing-and-setting-up-git/global-config.gif)

---

## Changing a Setting

Made a typo? Run the same command again with the right value. It replaces the old one

```bash
git config --global user.name "Riya Shrama"
git config --global user.name "Riya Sharma"
git config --global user.name
```

```text
Riya Sharma
```

---

## Beginner Mistakes

### Mistake 1: A name without quotes

```bash
git config --global user.name Riya Sharma
git config --global user.name
```

```text
Riya
```

There is no error, but only `Riya` was saved. Put quotes around every value that has a space.

---

### Mistake 2: Forgetting --global

```bash
git config user.name "Riya"
```

```text
fatal: not in a git directory
```

Without `--global`, a setting is only for the repository you are in. Outside a repository, Git stops. Use `--global` for the settings in this session.

---

### Mistake 3: Using an old terminal after installing

A terminal that was open before you installed Git still says that `git` is not recognized. Close VS Code completely and open it again.

---

### Mistake 4: An email that is not on your GitHub account

Your commits still work, but GitHub cannot connect them to your profile. Use the email of your GitHub account, or add this email to your account in Session 09.

---

### Mistake 5: Getting stuck in Vim

The terminal shows text with `~` lines, and typing does strange things: Git opened Vim. Press **Esc**, type `:q!` and press **Enter**. Then set VS Code as your editor.

---

## Practice Exercises

### Exercise 1

Install Git, or check that it is installed, and write down what `git --version` prints.

### Exercise 2

Set your name and email. Check both with `git config --global user.name` and `git config --global user.email`.

### Exercise 3

Set `main` as the first branch name and VS Code as the editor. Run `git config --global --list` and compare it with this session.

### Exercise 4

Run `git config --global --edit`. VS Code opens your `.gitconfig` file. Find each of your settings, then close the tab without changing anything.

### Exercise 5

Change your name to something else, check it, and change it back.

---

## Interview Questions

### What does git config --global do?

It saves a setting for every repository on your computer, in the `.gitconfig` file in your home folder.

### Why does Git need your name and email?

Every commit stores who made it. Git refuses to make a commit until it knows your name and email.

### What is the difference between a global and a local setting?

A global setting (`--global`) is for every repository on the computer. A local setting, made inside a repository without `--global`, is only for that repository, and it wins over the global one.

### Why do most projects use main instead of master?

`main` is the default branch name on GitHub and in most new projects. Git still uses `master` unless `init.defaultBranch` is set, so developers set it to `main`.

### How do you check your Git settings?

`git config --global --list` shows all global settings. `git config --global user.name` shows one of them.

---

| Previous | All sessions | Next |
| :--- | :---: | ---: |
| [01. What Are Git and GitHub?](01-what-are-git-and-github.md) | [Course outline](README.md#course-outline) | [03. Your First Repository](03-your-first-repository.md) |
