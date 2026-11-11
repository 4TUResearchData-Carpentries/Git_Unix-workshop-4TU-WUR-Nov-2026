# 🎓 Workshop Lesson Plan: Git Basics

**Note:** Instructor comments are marked as `> Instructor Note:` and should help guide the pacing, demonstration strategy, and what to emphasize. Everything else is intended for participants or for use during live demos.

## 🛠️ Preparation (Before Start)

To prepare your terminal for dual-pane command tracking:

```bash
# Main terminal (record command history live)
export PROMPT_COMMAND="history -a; $PROMPT_COMMAND"

# Second terminal (display ongoing history)
tail -f ~/.bash_history

# Clear history if needed
history -c
```

For Windows/WSL setup:

```bash
cd /mnt/c/Users/<your-username>/OneDrive - Delft University of Technology/
export PS1='$ '
```
For mirror a window using ([Ontopreplica](https://github.com/LorenzCK/OnTopReplica))

---

## 📍 Part 1: Getting Started with Git

| Objectives                                                                 | Questions                                      |
|---------------------------------------------------------------------------|-----------------------------------------------|
| Understand the benefits of an automated version control system.           | What is version control and why should I use it? |
| Understand the basics of how automated version control systems work.      |                                               |


### 🧊 Introduction \[10 min]

* Start with [HackMD collaborative notes](https://edu.nl/xhydh) and icebreaker.
* Transition to Git introduction with [slides](https://zenodo.org/records/19603828).
* Use [Mentimeter poll](https://www.menti.com/alx2mugaqr1n) in  presenter mode .

### 🔧 Step 1: Setting Up Git \[6 min]

> Instructor Note: Sometimes Git Bash won't open via shortcut. Direct students to `C:/Git/git-bash.exe` if needed.

Test installation:

```bash
git --version
```

Get help:

```bash
git --help
# or for specific command
git init -h
```

Configure Git (only needed once):

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

Comments:
- Use the email associated with your GitHub, GitLab, Bitbucket, etc., account.
- Ensures commits are linked to your user profile.
- Enables activity tracking, avatars, and contribution graphs.
- If using Git in a university/research group context:
 - Use your institutional email if commits should be traceable to that identity.

### Configure line endings:

```bash
# Windows
git config --global core.autocrlf true
# Mac/Linux
git config --global core.autocrlf input
```

### Default branch name

By default Git will create a branch called master when you create a new repository with git init. From Git version 2.28 onwards, you can set a different name for the initial branch

```bash

git config --global init.defaultBranch main

```

### Check configuration:

```bash
git config --global --list
```

### 📦 Step 2: Creating a Repository \[4 min]

Create and initialize project folder:

```bash
mkdir patients
cd patients
git init
```
This creates a new subdirectory named .git that contains all of your necessary repository files — a Git repository skeleton. At this point, nothing in your project is tracked yet.

Verify repo status:

```bash
ls -a
# Shows .git folder
git status
```

> Instructor Note: Explain that `.git` is the hidden folder tracking everything.
> Do **not** initialize repos inside subfolders—this breaks versioning logic.

---

### 📝 Step 3: Tracking Your First Change \[10 min]

Create a script:

```bash
touch calculate_mean.py
nano calculate_mean.py
```

Add the code:

```python
import numpy as np

def calculate_mean(data):
    mean = np.mean(data)
    return mean
```

Check status:

```bash
git status
```

Track and commit (to the unmodified state):

```bash
git add calculate_mean.py # git add is a multipurpose command for start tracking a file and stage it (once it is already track). It may be helpful to think of it more as “add precisely this content to the next commit” rather than “add this file to the project”
# Git may show CRLF warning (safe to ignore)



# commit the file
git commit -m "create script for analysis"
```

> Instructor Note: Explain staging area vs history. Use slides to illustrate the **modify → add → commit** cycle.

---

### 🛠️ Step 4: Making Additional Changes \[5 min]

Add documentation to the script:

```python
""" This function computes the mean of an array.
Input: A one dimensional array
Output: The mean of the array
"""
```

Check and review differences:

```bash
git status # you want to know which files you have changed
git diff  #you want to know exactly what you changed, not just which files were changed 
```

`git diff` command compares what is in your working directory with what is in your staging area. The result tells you the changes you’ve made that you haven’t yet staged.

Add and commit the changes:

```bash
git add calculate_mean.py


##(optional)

# add another change to calculate_mean.py and run git status 

# What the heck? Now calculate_mean.py is listed as both staged and unstaged. How is that possible? It turns out that Git stages a file exactly as it is when you run the git add command. If you commit now, the version of calculate_mean.py as it was when you last ran the git add command is how it will go into the commit, not the version of the file as it looks in your working directory when you run git commit. If you modify a file after you run git add, you have to run git add again to stage the latest version of the file


git add calculate_mean.py
```

Check and review differences:

```bash
git diff --staged # if you want to see what you’ve staged that will go into your next commit, you can use git diff --staged. This command compares your staged changes to your last commit
git commit -m "add docstring"
```

Remember that the **commit records the snapshot you set up in your staging area**. Anything you didn’t stage is still sitting there modified; you can do another commit to add it to your history. **Every time you perform a commit, you’re recording a snapshot of your project that you can revert to or compare to later.**


### 📁 Step 5: Working with Empty Directories \[4 min]

Git won't track empty directories:

```bash
mkdir treatments
git status
git add treatments
git status  # Still no changes detected
```

Add files to track the folder:

```bash
touch treatments/aspirin.txt treatments/advil.txt
git add treatments/ # track all folder recursively
git commit -m "add some treatments for patients"
ls -R
```

---

### 🚫 Step 6: Ignoring Files \[6 min]

> Instructor note explain why these ignoring files are necessary and not to include into the repository

Simulate a scenario with large/unwanted files:

```bash
mkdir data
touch data/a.dat data/b.dat big-data.zip
```

Create a `.gitignore`:

```bash
nano .gitignore
```

Contents:

```bash
# data files:
big-data.zip
data/
```

Track the ignore file:

```bash
git add .gitignore
git commit -m "Ignoring data files"
git status --ignored
```

---

## 🧪 Hands-On Exercise: Modify, Add, Commit \[15 min]

> Show the slide with the exercise

Assignment:

**Create new repository and use the modify-add-commit cycle.**
- Create and initialize a repository called ‘my-repo’
- Create a file ‘research.txt’ with the sentence “Science is awesome”.
- Track the file and commit the changes. Remember to use a meaningful message.
- Change sentence in ‘research.txt’ to “Science is messy”
- Stage the change and commit.
- Create a folder called “raw-data”
- Ignore the raw-data folder and commit it 
- Check the history of your project and verify the number of commits. 


Invite someone to do it lively


Steps:

```bash
mkdir my-repo && cd my-repo
git init
nano research.txt  # Add "Science is awesome"
git add research.txt
git commit -m "add awesome science"

nano research.txt  # Change to "Science is messy"
git add research.txt
git commit -m "change to messy science"

mkdir raw-data
touch .gitignore
nano .gitignore  # Add: raw-data/
git add .gitignore
git commit -m "ignore raw data folder"

git log --oneline --graph
```

---

## 🔍 Part 2: Exploring Git History \[16 min]

### a. View the Log

By default, with no arguments, git log lists the commits made in that repository in reverse chronological order; that is, the most recent commits show up first. As you can see, this command lists each commit with its SHA-1 checksum, the author’s name and email, the date written, and the commit message.

```bash
git log
git log --oneline
git log --graph
git log > changelog.txt
```

> Instructor Note: Explain paging (Q = quit, / = search, N = next match).

### b. Understanding HEAD

> HEAD is a reference to the most recent commit in the current branch (`main` or `master`).
> As we saw in the previous episode, we can refer to commits by their identifiers. You can refer to the most recent commit of the working directory by using the identifier _HEAD_.
>We’ve been adding small changes at a time to calculate_mean.py, so it’s easy to track our progress by looking, so let’s do that using our HEADs. Before we start, let’s make a change to calculate_mean.py, adding some lines.
> Visualize with https://edu.nl/bvu6x 

### c. Add a Usage Example

```bash
nano calculate_mean.py
```

```python
# Usage:
# data=np.array([0,10,20,30])
# mean=calculate_mean(data)
```

### d. Compare Differences

```bash
git diff HEAD calculate_mean.py
# or previous versions:
git diff HEAD~1 calculate_mean.py
git diff HEAD~3 calculate_mean.py
```

### e. Use Specific Commit IDs

```bash
git log --oneline  # Copy a commit ID
git diff <commit-id> calculate_mean.py
```

### f. Use `git show`

```bash
git show HEAD~2 calculate_mean.py
```
>Explain that git show is used for viewing a commit(history and why something was written) and git diff to compare files.
---

## ⏪ Reverting Changes \[9 min]

> Instructor Note: Let participants follow if they are confident. Otherwise, demo it.

Ensure changes are committed:

```bash
git add calculate_mean.py

git restore calculate_mean.py

cat calculate_mean.py # inspect the file

git status 

```
if nothing is modified in the file , proceed to add and commit the file


Revert to last commit:

```bash
git add calculate_mean.py
git commit -m "add usage"
git restore calculate_mean.py
cat calculate_mean.py
git status
```
It’s important to understand that git restore <file> is a dangerous command. Any local changes you made to that file are gone — Git just replaced that file with the last staged or committed version. Don’t ever use this command unless you absolutely know that you don’t want those unsaved local changes.

> showcase the lost of information (optional)



```bash
git add calculate_mean.py

```
- change the local file with some dummy text

```bash

git restore calculate_mean.py
cat calculate_mean.py 
```

what has happened?  the local modifications should have been erased

-----------------------

Revert to specific commit:

```bash
git log --oneline
git restore -s <commit-id> calculate_mean.py
```

Restore the file to the state of the last commit:

```bash
git restore calculate_mean.py
git add calculate_mean.py
git commit -m "usage message added"
```

> Instructor Note: Wrap up with summary of `git log`, `diff`, `show`, and `restore`

---

✅ End of day one



