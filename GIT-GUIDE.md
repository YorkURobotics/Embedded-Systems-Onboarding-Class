```
████ TOP SECRET ████   GIT, FROM ZERO
```

Never used git? Start at step 1 and do every command. Nothing is assumed.

Used it before? You only need **step 4**: branch named after you, never commit to `main`.

Every line starting with `$` is a command you type. Do not type the `$`.

---

## What git is, in four sentences

Git records snapshots of your files over time. Each snapshot is called a **commit**, and it has a message saying what changed.

A **branch** is a separate line of commits, so several people can work on the same project without writing over each other. GitHub is a website that stores a copy of your repository so other people (us) can see it.

You work on **your own branch**. You never touch `main`.

---

## STEP 1: Install git

**Windows:** download from https://git-scm.com/download/win and run the installer. Accept every default. This also gives you "Git Bash", which is where you type these commands.

**macOS:** open Terminal and type `git --version`. If it is not installed, macOS offers to install it. Otherwise install Homebrew from https://brew.sh then:
```
$ brew install git
```

**Linux (Ubuntu/Debian):**
```
$ sudo apt update
$ sudo apt install git
```

Check it worked:
```
$ git --version
```
You should see something like `git version 2.43.0`.

---

## STEP 2: Tell git who you are

Do this once, ever. Your name and email go on every commit you make.

```
$ git config --global user.name "Your Name"
$ git config --global user.email "your.email@example.com"
```

Use the same email as your GitHub account or your commits will not link to your profile.

Check it:
```
$ git config --global --list
```

---

## STEP 3: Get the repository onto your computer

**First, get access.** Ask your Handler to add you to the repository on GitHub. You need write access to push a branch.

**Then clone it.** Cloning means downloading a full copy, history and all.

```
$ cd ~/Documents
$ git clone https://github.com/<ORG>/<REPO>.git
$ cd <REPO>
```

Replace `<ORG>` and `<REPO>` with the real ones. Your Handler will give you the URL, or copy it from the green **Code** button on GitHub.

**If it asks for a password:** GitHub stopped accepting account passwords in 2021. You need one of these:

- **Personal access token (easier).** Create one at https://github.com/settings/tokens, tick the `repo` scope, copy the token, and paste it when git asks for your password. Save it somewhere, GitHub will not show it again.
- **SSH key (nicer long term).** Follow https://docs.github.com/en/authentication/connecting-to-github-with-ssh then clone using the `git@github.com:...` URL instead.

---

## STEP 4: Make your branch

**This is the important one.** Everything you do lives on a branch named after you.

```
$ git checkout -b firstname-lastname
```

So if your name is Alex Chen:
```
$ git checkout -b alex-chen
```

`checkout -b` means "make a new branch and switch to it". Check which branch you are on at any time:

```
$ git branch
```
The one with a `*` next to it is where you are.

> **Never commit to `main`.** `main` is the task itself. If `git branch` shows `* main`, stop and run the `checkout -b` command above.

---

## STEP 5: Do some work, then commit it

This is the loop you repeat for the whole project.

**See what changed:**
```
$ git status
```
Red files are changed but not staged. Green files are staged and ready to commit.

**Stage the files you want to commit:**
```
$ git add firmware/Core/Src/main.c
$ git add FIELD-LOG.md
```
Or stage everything that changed:
```
$ git add .
```

**Commit, with a message saying what you did:**
```
$ git commit -m "Read CO2 from the sensor over I2C"
```

Good messages say what changed. `fix stuff` is a commit message in the same sense that `it doesn't work` is a bug report.

**See your history:**
```
$ git log --oneline
```

---

## STEP 6: Push to GitHub

Pushing uploads your commits so we can see them.

**The first time on a new branch**, you have to tell git where to put it:
```
$ git push -u origin firstname-lastname
```

**Every time after that**, just:
```
$ git push
```

Push at the end of every session. We open your branch from GitHub, not from your laptop.

---

## STEP 7: Check it worked

Go to the repository on GitHub. Click the branch dropdown (it says `main` by default) and pick your branch. Your files and commits should be there.

If they are not, you have not pushed. Run `git status` and see what it says.

---

## The whole loop, once you are set up

```
$ git status                              # what changed?
$ git add .                               # stage it
$ git commit -m "what I did"              # save a snapshot
$ git push                                # upload it
```

Four commands. That is the job.

---

## Getting updates from main

If your Handler changes the task while you are working, pull the changes into your branch:

```
$ git checkout main
$ git pull
$ git checkout firstname-lastname
$ git merge main
```

If git reports a conflict, open the file it names. You will see markers like:

```
<<<<<<< HEAD
your version
=======
their version
>>>>>>> main
```

Delete the markers, keep the text you want, save, then:
```
$ git add <the file>
$ git commit
```

---

## When something goes wrong

**`fatal: not a git repository`**
You are in the wrong folder. `cd` into the cloned repository first.

**`Please tell me who you are`**
You skipped step 2. Run those two `git config` commands.

**`Updates were rejected because the remote contains work that you do not have`**
Somebody pushed to your branch, or you pushed from another machine. Run:
```
$ git pull
$ git push
```

**`error: failed to push some refs`, and you are on `main`**
You are not allowed to push to `main`, and that is deliberate. Make your branch:
```
$ git checkout -b firstname-lastname
$ git push -u origin firstname-lastname
```

**I staged a file I did not mean to**
```
$ git restore --staged <file>
```

**I want to throw away my changes to a file**
```
$ git restore <file>
```
This deletes your edits to that file. There is no undo.

**I typed the wrong commit message**
Only if you have not pushed yet:
```
$ git commit --amend -m "the message I meant"
```

**I committed to `main` by accident**
Do not panic, nothing is lost:
```
$ git branch firstname-lastname      # save your work to a new branch
$ git reset --hard origin/main       # put main back how it was
$ git checkout firstname-lastname    # carry on
```

**Everything is on fire and I do not understand what I did**
Stop. Do not run anything else, especially anything you found online with `--force` in it. Message your Handler with the exact output of:
```
$ git status
$ git log --oneline -5
```
Nobody has ever been in trouble for a broken repository.

---

## References

- **Official git book**, free, genuinely good: https://git-scm.com/book/en/v2
- **GitHub quickstart:** https://docs.github.com/en/get-started/quickstart
- **Interactive branching visualiser**, 20 minutes well spent: https://learngitbranching.js.org/
- **Oh Shit, Git!?!** for fixing mistakes: https://ohshitgit.com/

---

## What we check

- You worked on your own branch and never committed to `main`.
- At least 8 commits across at least 4 different days.
- `FIELD-LOG.md` committed alongside the code it describes, not written up at the end.
- Everything pushed before your debrief.

Commit your mistakes. A history with no bugs in it describes a project that did not happen.
