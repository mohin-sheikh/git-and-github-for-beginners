# What Are Git and GitHub?

> **Session 1 of 12** | Part 1: Git on Your Computer | [All sessions](README.md#course-outline)

## Table of Contents
- [The Problem Git Solves](#the-problem-git-solves)
- [What Is Git?](#what-is-git)
- [What a Commit Remembers](#what-a-commit-remembers)
- [What Is GitHub?](#what-is-github)
- [Git and GitHub Side by Side](#git-and-github-side-by-side)
- [How Teams Use Git and GitHub](#how-teams-use-git-and-github)
- [Words You Will Learn](#words-you-will-learn)
- [How This Course Works](#how-this-course-works)
- [Try It Yourself](#try-it-yourself)
- [Beginner Mistakes](#beginner-mistakes)
- [Practice Exercises](#practice-exercises)
- [Interview Questions](#interview-questions)

---

## The Problem Git Solves

You are building a project, and it works. You change some code, and now it does not work. You cannot remember what you changed.

So you start keeping copies: `app-backup.js`, `app-final.js`, `app-final-2.js`. After a week, nobody knows which copy is the right one.

![Without Git, a folder fills with copies like app-final-2.js and nobody knows which one works; with Git, there is one app.js and Git remembers every version](images/01-what-are-git-and-github/final-files.gif)

Git solves this problem. You keep **one** copy of every file, and Git remembers every version of it.

---

## What Is Git?

**Git** is a free tool that you install on your computer. It watches a project folder and saves a **snapshot** of it whenever you ask. Each snapshot is called a **commit**.

With Git you can

* see what changed, and when
* go back to any earlier version
* try a new idea without breaking the version that works (Session 07)

Because it keeps track of versions, Git is called a **version control system**. It is the most used one in the world.

![Four commits on a line, each a snapshot of the project folder; when the newest commit breaks the app, you go back to the commit that worked](images/01-what-are-git-and-github/snapshots.gif)

Think of a commit as a **save point in a game**. If something goes wrong, you go back to the last save point.

---

## What a Commit Remembers

Every commit saves five things

| Part      | Example            | Meaning                                  |
| --------- | ------------------ | ---------------------------------------- |
| Snapshot  | `app.js`, `menu.js` | All your files, as they were at that moment |
| Message   | `Fix price bug`    | Why: a short note that you write         |
| Author    | `Riya Sharma`      | Who made the commit                      |
| Date      | `Mon Nov 2 10:15`  | When it was made                         |
| ID        | `9fceb02`          | A unique name that Git gives every commit |

![One commit opens into a card: its id, its message, its author, its date and a snapshot of the files](images/01-what-are-git-and-github/commit-card.gif)

The full ID is 40 letters and numbers long. Git usually shows only the first 7, like `9fceb02`. You make your first commit in Session 03.

---

## What Is GitHub?

**GitHub** is a website, [github.com](https://github.com), that stores Git projects online. A project that Git keeps track of is called a **repository**, or **repo** for short.

On GitHub you can

* keep a backup of your repositories online
* share your code with anyone, or keep it private
* work on the same project with other people
* show your projects to companies when you look for a job

A free GitHub account can have as many public and private repositories as you want.

---

## Git and GitHub Side by Side

![Git keeps the history of my-notes on your computer; git push sends a copy of the commits to GitHub, a website](images/01-what-are-git-and-github/git-vs-github.gif)

|                  | Git                              | GitHub                                       |
| ---------------- | -------------------------------- | -------------------------------------------- |
| What it is       | A tool                           | A website                                    |
| Where it runs    | On your computer                 | On the internet                              |
| Needs internet   | No                               | Yes                                          |
| What it does     | Saves the history of your project | Stores repositories online, helps people work together |
| Cost             | Free                             | Free for most uses                           |

Git works without GitHub. GitHub needs Git. There are other websites like GitHub, such as GitLab and Bitbucket. What you learn in this course works there too.

---

## How Teams Use Git and GitHub

Every person in a team has a copy of the project on their own computer, with its whole history. GitHub has the shared copy.

1. Riya makes a commit and **pushes** it to GitHub
2. Arjun **pulls** from GitHub, and gets Riya's commit
3. It works the other way round too: Arjun pushes, Riya pulls

![Riya pushes a commit to GitHub and Arjun pulls it; then Arjun pushes a commit and Riya pulls it, so everyone has every commit](images/01-what-are-git-and-github/team-flow.gif)

You push in Session 09 and pull in Session 10. Session 11 teaches **pull requests**, the way teams check each other's work before it is added.

---

## Words You Will Learn

| Word              | Meaning                                                  | Session |
| ----------------- | -------------------------------------------------------- | ------- |
| Repository (repo) | A project folder whose history Git keeps                 | 03      |
| Commit            | A snapshot of the project, with a message                | 03      |
| Staging area      | Where you choose what goes into the next commit          | 03      |
| Branch            | A separate line of work                                  | 07      |
| Merge             | Joining the work of two branches                         | 07      |
| Remote            | A copy of the repository somewhere else, like on GitHub  | 09      |
| Push              | Send your commits to GitHub                              | 09      |
| Clone             | Download a repository from GitHub                        | 10      |
| Pull              | Get new commits from GitHub                              | 10      |
| Pull request      | Ask for your work to be added, so others can review it first | 11      |

You do not need to remember these now. Each word is explained when you need it.

---

## How This Course Works

![The course in four parts: Git on your computer, branches, GitHub, and a final project, followed by your own projects on GitHub](images/01-what-are-git-and-github/learning-path.gif)

| Part                     | Sessions | What you learn                                          |
| ------------------------ | -------- | ------------------------------------------------------- |
| 1. Git on Your Computer  | 01 to 06 | Install Git, make commits, look at history, undo mistakes |
| 2. Branches              | 07 to 08 | Try new ideas safely, fix merge conflicts               |
| 3. GitHub                | 09 to 11 | Put projects online, work from two places, pull requests |
| 4. Final Project         | 12       | Put a Node.js project on GitHub, the way teams do       |

Every session has a short explanation, commands to type, the real output you should see, animated diagrams, beginner mistakes, practice exercises and interview questions.

You type the commands in a **terminal**. The one in VS Code is enough: **Terminal > New Terminal**. The course was tested with Git 2.55 on Windows. Git works the same on macOS and Linux, and the sessions point out the few differences.

Your output will be a little different from the course: your name, your dates and your commit IDs are your own. Everything else should match.

---

## Try It Yourself

Git may already be on your computer. Open a terminal and type

```bash
git --version
```

If Git is installed, it prints its version. Your version may be newer

```text
git version 2.55.0.windows.4
```

If Git is not installed, Windows PowerShell shows an error that starts like this

```text
git : The term 'git' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the
spelling of the name, or if a path was included, verify that the path is correct and try again.
```

On macOS and Linux, the terminal says `command not found` instead.

Both are fine. Session 02 installs Git (or updates it) and sets it up.

---

## Beginner Mistakes

### Mistake 1: Thinking Git and GitHub are the same

Git is a tool on your computer. GitHub is a website that stores Git repositories. You can use Git without GitHub, but not GitHub without Git.

---

### Mistake 2: Thinking Git is only for teams

Git helps even when you work alone. You can go back when something breaks, and see what you changed yesterday.

---

### Mistake 3: Keeping copies instead of commits

`app-final-2.js` is an old habit. With Git, keep one file and make a commit every time something works.

---

### Mistake 4: Waiting until the project is finished

Start using Git on the first day of a project. A history can only help you if it exists.

---

### Mistake 5: Putting passwords on GitHub

Anyone can read everything in a public repository. Passwords and secret keys, like the `.env` file in a Node.js project, must never be committed. Session 04 shows how to keep them out.

---

## Practice Exercises

### Exercise 1

Run `git --version`. Write down whether Git is installed, and which version.

### Exercise 2

Think of a time you lost work, or broke something that used to work. Write down how a list of commits would have helped.

### Exercise 3

Open [github.com/nodejs/node](https://github.com/nodejs/node) in your browser. You do not need an account. Find the number of **Commits** and click it. Read the messages of the three newest commits.

### Exercise 4

On the same page, find who made the newest commit, and when.

### Exercise 5

Explain the difference between Git and GitHub in two sentences, as if to a friend.

---

## Interview Questions

### What is version control?

A way to keep track of every change to a set of files, so you can see who changed what and when, and go back to any earlier version.

### What is the difference between Git and GitHub?

Git is a version control tool that runs on your computer. GitHub is a website that stores Git repositories online and adds tools for working together, like pull requests.

### What is a repository?

A project folder whose history Git keeps. It holds the files and all their commits.

### What is a commit?

A saved snapshot of the project at one moment, with a message, an author, a date and a unique ID.

### Why is Git called a distributed version control system?

Every copy of a repository, on every computer, has the full history. You can commit without internet, and the history does not depend on one server.

---

| Previous | All sessions | Next |
| :--- | :---: | ---: |
| This is the first session | [Course outline](README.md#course-outline) | [02. Installing and Setting Up Git](02-installing-and-setting-up-git.md) |
