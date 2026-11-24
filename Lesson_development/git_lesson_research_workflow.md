# 🎓 Workshop Lesson Plan: Git Basics — A Research Workflow

## Workshop narrative

**Scenario:** A  research team is studying water temperatures at a field site. One temperature is suspiciously high. As the team checks its field notes, it updates its Python script and methodology, records the reasoning for a correction, and ensures that large or restricted raw datasets are not committed accidentally.

The recurring teaching questions are:

1. **What has changed?** — `git status` and `git diff`.
2. **What will be committed?** — `git add` and `git diff --staged`.
3. **What was committed?** — `git log`, `git show`, and file histories.

The same local repository, `field-study`, will be used on Day 2 to create and connect a remote GitHub repository also named `field-study`.

### Learning objectives

By the end of **Day 1**, learners should be able to:

- Explain why automated version control is relevant to reproducibility and research collaboration.
- Configure Git, initialize a repository, and identify its hidden `.git` directory.
- Use the modify–add–commit cycle, including staging multiple files in a single commit.
- Distinguish untracked, modified, staged, and committed changes.
- Write descriptive commit messages and inspect which files belong to a commit.
- Compare versions using `git diff`, `HEAD`, commit IDs, and `git show`.
- Recover an earlier version of a file without rewriting the commit history.
- Explain why empty directories are not recorded and configure `.gitignore`.

**Day 2** continues with GitHub remotes, push/pull, and collaboration as in the original script.

### Suggested Day 1 pacing

| Section | Duration |
| --- | ---: |
| Introduction and motivation | 10 min |
| Step 1 — Setting up Git | 6 min |
| Step 2 — Creating a repository | 4 min |
| Step 3 — First tracked Python script | 10 min |
| Step 4 — Two files, one commit | 12 min |
| Step 5 — Empty directories and local raw data | 5 min |
| Step 6 — Ignoring appropriate files | 7 min |
| Independent hands-on challenge and debrief | 25–30 min |
| Part 2 — Exploring and extending history | 18 min |
| Restoring versions safely | 12 min |
| **Total** | **109–114 min** |

> Instructor Note: These are suggested timings for an approximately two-hour local-Git block; allow extra time for installations or for beginners unfamiliar with shell commands. The 25–30-minute independent challenge replaces the previous 15-minute exercise.

---

## 🛠️ Preparation (Before Start)

**For the instructor:** Prepare a terminal and, optionally, a second terminal to display commands executed during live coding.

```bash
# Main terminal (Bash): append command history after each prompt
export PROMPT_COMMAND="history -a; ${PROMPT_COMMAND:-}"

# Second terminal: follow the history file
tail -f ~/.bash_history
```

> Instructor Note: These Bash history commands are optional. They assume ordinary Bash history configuration. Avoid clearing history during the workshop. If you use Windows/WSL, choose a working directory available in that environment.

For the lesson and the unchanged Day 2 commands, the examples assume `~/Desktop`. Before starting, ensure this directory exists:

```bash
cd ~/Desktop
pwd
```

> Instructor Note: In WSL, `~/Desktop` is in the Linux home directory, not necessarily the Windows Desktop. If you prefer a Windows-mounted or institutional folder, use that location consistently on both days. Avoid paths with spaces unless quoted.

To simplify the displayed prompt (optional):

```bash
export PS1='$ '
```


**Before learners arrive:** Check that Git works and that `python` refers to Python 3. Depending on the machine, the executable may be `python3` instead. This example uses only the Python standard library: **NumPy is not required**.

---

## 📍 Day 1 — Part 1: Getting Started with Git

| Objectives | Guiding questions |
| --- | --- |
| Understand the benefits of automated version control. | What goes wrong when researchers edit and share files without version tracking? |
| Recognize how Git records changes. | What has changed, what is staged, and what is committed? |
| Connect Git to reproducible research. | Can we reconstruct how an analysis result was obtained? |

### 🧊 Introduction [10 min]


**Opening question:** You ran an analysis two months ago and got a different result. How would you determine which code and assumptions produced each result?


> Instructor Note: Present Git as a way to document the evolution of research, not merely as a backup or a software-development requirement. Keep the  field-study story visible throughout the demonstrations.


### 🔧 Step 1: Setting Up Git [6 min]

> Instructor Note: If Git Bash does not open from a shortcut on a Windows machine, check its installed location (e.g., `C:/Program Files/Git/git-bash.exe`).

Check the installation:

```bash
git --version
```

Explore help:

```bash
git --help
git init -h # each command has its own help 
```

Configure Git identity (usually needed only once per computer):

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global init.defaultBranch main 
```

Check the configuration:

```bash
git config --global --list
```

**Discuss:** A commit records an author name and email; choose an appropriate identity. On GitHub, commit attribution depends on the email settings and verified addresses, and privacy-preserving GitHub noreply addresses are available.

**Line endings (only if relevant to your operating system and setup):**

```bash
# Common Windows setting for Git working files
git config --global core.autocrlf true

# Common macOS/Linux setting
git config --global core.autocrlf input
```

> Instructor Note: Show only the platform-appropriate setting if needed. These commands change global Git configuration; do not ask everyone to execute both.

### 📦 Step 2: Creating a Repository [4 min]

**Scenario:** A  research team is studying water temperatures at a field site. 

**Story:** Our research team needs one location for its analysis script, methods, and project history.

```bash
cd ~/Desktop
mkdir field-study
cd field-study
git init
```

Inspect the repository:

```bash
pwd
ls -a
git status
```

**Ask:** Why can we see `.git` with `ls -a`, but not with ordinary `ls`?

> Instructor Note: Explain that `.git` holds the repository's metadata and history. 

### 📝 Step 3: Tracking Your First Change [10 min]

**Story:** Our team has collected four water-temperature readings. It wants to calculate their arithmetic mean and remember the exact code used.

Create and edit the analysis script:

```bash
nano calculate_mean.py
```

Add this Python code:

```python
# Water-temperature readings (Celsius).
measurements = [16.0, 18.0, 19.0, 30.0]


def calculate_mean(values):
    return sum(values) / len(values)


print(f"Mean water temperature: {calculate_mean(measurements)} C") 
```

> Instructor Note: The exercise is about Git, not learning NumPy or Python syntax. The script is small, executable, and self-contained. The later suspicious `30.0` reading will create a reason to inspect and correct history.

Run the script, then check what Git sees:

```bash
python calculate_mean.py
# If needed on your computer: python3 calculate_mean.py

git status
```

Expected result from Python:

```text
Mean water temperature: 20.75 C
```

**Prediction:** Is Git already tracking `calculate_mean.py`? Has anything been committed yet?

Stage and commit the new file:

```bash
git add calculate_mean.py
git status
git commit -m "Add initial water temperature analysis"
git status
```

> Instructor Note: Draw the **working directory → staging area → local repository** model. Clarify that `git add` prepares a snapshot; `git commit` records the staged snapshot. 
**Checkpoint:**


```bash
git log # Displays the commit history without showing the files that has been modified
git log --name-only # Lists the files changed in that commit, without showing their contents
git log --stat # Summary of file changes
```


### 🛠️ Step 4: Two Files, One Research Decision, One Commit [12 min]

**Story:** The value `30.0 C` looks unusually high. The team wants to document the analysis procedure and flag the reading for verification before changing any data.

First, edit `calculate_mean.py` and add a short docstring to the function, leaving the measurements unchanged:

```bash
nano calculate_mean.py
```

The function should now look like this:

```python
def calculate_mean(values):
    """Return the arithmetic mean of a non-empty sequence of numbers."""
    return sum(values) / len(values)
```

Create a methodology file:

```bash
nano methods.md
```

Content for `methods.md`:

```markdown
# Water temperature field study

- All measurements are expressed in degrees Celsius.
- We calculate the arithmetic mean of four readings using `calculate_mean.py`.
- The reading of 30.0 C is unexpectedly high and must be checked against field notes.
```

Check the state of the repository:

```bash
git diff # it shows changes that are not staged yet
git status # Note that methods.md is not yet tracked that is why git diff does not show it 
```

> Instructor Note: `git diff` shows the unstaged change in `calculate_mean.py`, but does **not** display the content of the new untracked `methods.md`. Use `git status` to spot that untracked file.

Stage **only** the script:

```bash
git add calculate_mean.py
git status
git diff # it should give no answer since the changes are staged
git diff --staged # it shows the changes that are staged ready to be commited 
```

**Prediction:** If we commit now, does `methods.md` get included? Why not?

Stage the second file, inspect the planned commit, then commit once:

```bash
git add methods.md
git status
git diff --staged # what have changed in all staged files 
git diff --staged --stat # summary of the differences in all commited files 
git commit -m "Document the analysis method and suspicious reading"
```

Verify that **both files** belong to the one commit:

```bash
git log --name-only
git log -1 --name-only
git log -1 --stat
```

**Key point:** You can stage as many related files as necessary before creating one commit. The commit contains the staged versions of those files, not automatically every change in the working directory.

> Instructor Note: This is the main conceptual checkpoint. Ask someone to explain why two `git add` commands did not create two commits. Explain that staged changes can be reviewed before committing.

### 📁 Step 5: Empty Directories and Research Data [5 min]

**Story:** The team receives raw measurements from an instrument and creates a directory to store them locally.

```bash
mkdir raw-data
git status
```

**Ask:** Why does Git not mention `raw-data/`?

> Instructor Note: Git tracks file paths and contents, not empty directory objects. Attempting `git add raw-data` at this point does not add an empty folder; with no matching file, Git may report a pathspec error.

Now create a small CSV file inside the folder:

```bash
nano raw-data/temperature_readings.csv
```

```csv
sample,temperature_c
A,16.0
B,18.0
C,19.0
D,30.0
```

```bash
git status
ls raw-data
```

**Ask:** Now does Git notice the directory? What exactly has changed?

> Instructor Note: The CSV is deliberately untracked for the next step. Do not add it to Git before demonstrating `.gitignore`.

### 🚫 Step 6: Decide What to Ignore [7 min]

**Story:** In this project, the real field datasets may be large or subject to access restrictions, so the team decides to keep those files outside the Git repository. Git will track the analysis code and method documentation, but not local copies of raw datasets.

**Discussion prompt:** Should researchers *always* ignore raw data? **No.** Small, non-sensitive data can be useful in Git. This is a project-specific decision; for data kept elsewhere, document where authorized collaborators can obtain the data and which version supports the analysis.

Create `.gitignore`:

```bash
nano .gitignore
```

```gitignore
# Local raw datasets are managed outside this Git repository.
raw-data/


```

Check whether Git recognizes the ignore rule:

```bash
git status # the raw-data should not appear in the list 
git status --ignored
git check-ignore -v raw-data/temperature_readings.csv # Check whether the CSV file is ignored by Git.
```

Stage and commit the ignore configuration:

```bash
git add .gitignore
git commit -m "Ignore local raw data"
git status
git log --stat # to see a summary of the commits and which files has been modified
```

> Instructor Note: `.gitignore` does not delete local files, does not retroactively untrack previously committed files, and does not make a public repository safe for sensitive data already committed.


## Hands-On Exercise: Modify, Add, Commit \[20 min]


- Go to the Challenge at Lesson_development/Material_for_participants/git_research_workflow_exercises.md PART 1-3. 

BREAK - 15 mins

## 🔍 Day 1 — Part 2: Exploring Git History [18 min]

### a. View the Log

At this point the instructor's `field-study` repository should have **three commits**: initial script, documentation of the suspicious reading, and ignore configuration.

```bash
git log
git log --oneline
git log --graph --oneline
git log -1 --name-only
```

> Instructor Note: Explain that `-1` limits output to one commit, `--oneline` is compact, and `--name-only` lists affected files. In the pager, use `q` to quit. Avoid redirecting the log into an untracked file during this demonstration.

### b. Understand `HEAD` and Older Commits

`HEAD` names the currently checked-out commit in this simple workflow. `HEAD~1` names its first parent, and `HEAD~2` the parent before that.

```bash
git log --oneline
git show --stat HEAD # show what was changed in the HEAD
git show --stat HEAD~1 # show what was changed in the commit before the last one
```

**Ask:** Which commit introduced `methods.md`? Which commit introduced `.gitignore`?

> Instructor Note: The exact commit hashes differ between computers. Do not write a fixed hash on the slides. For the instructor's three-commit history, `HEAD~1` is the two-file documentation commit.

### c. The Research Twist: Verify an Unexpected Observation

**Story:** A researcher checks the field notebook and learns that `30.0 C` was a transcription error; the validated reading was `20.0 C`. The team should change both the computational input and the written method explanation, keeping a record of why.

Edit `calculate_mean.py`:

```bash
nano calculate_mean.py
```

Change only the list of measurements:

```python
measurements = [16.0, 18.0, 19.0, 20.0]
```

Edit `methods.md`:

```bash
nano methods.md
```

Replace the final bullet with:

```markdown
- Field notes verified that the last reading was 20.0 C, not 30.0 C; the original value was a transcription error.
```

Compare the files with their last committed versions:

```bash
git status
git diff
git diff HEAD -- calculate_mean.py #compare last commited file with current version of the script 
```

Run the corrected script:

```bash
python calculate_mean.py
```

Expected result:

```text
Mean water temperature: 18.25 C
```

Stage the related changes together:

```bash
git add calculate_mean.py
git add methods.md
git diff --staged
git commit -m "Correct temperature error after field-note check"
```

> Instructor Note: The changing numeric result makes the value of an audit trail tangible. Emphasize that the reason for the change belongs in the commit message and the research documentation, not only in someone's memory.

### d. Compare Committed Versions 

```bash
git log --oneline
git diff HEAD~1 HEAD -- calculate_mean.py
git diff HEAD~1 HEAD -- methods.md
```

The command above compares **two committed versions**. In contrast:

```bash
git diff HEAD -- calculate_mean.py
```

...compares the version at `HEAD` with the file currently in the working tree (including staged changes). It should show no difference if the file is clean.

### e. Use a Specific Commit ID

```bash
git log --oneline -- calculate_mean.py
# Substitute a real commit hash from your own output:
git show <commit-hash> -- calculate_mean.py
```

> Instructor Note: Use the output from `git log` to identify which commits touched the script. `git show` displays the changes introduced *by a commit*; `git diff` compares two states or versions. A commit hash identifies a specific point in history.

---

### Exercise (5 mins)

Go back to your `mini-field-study` folder and continue with exercises(Lesson_development/Material_for_participants/git_research_workflow_exercises.md) Part 4. 



## ⏪ Restoring Changes and Earlier Versions [12 min]

> Instructor Note: This segment deliberately separates (1) discarding an unwanted unstaged change and (2) retrieving an older committed version. Explain the consequences of overwriting uncommitted work before demonstrating `git restore`.

### a. Introduce an Accidental Change

```bash
echo "# TODO: accidental debug note" >> calculate_mean.py
git status
git diff
```

**Prediction:** If we discard only the unstaged change, will Git delete our previous commits?

### b. Discard the Unstaged Change

```bash
git restore calculate_mean.py
git status
```

`git restore <file>` restores an unstaged working-tree file from the staging area by default. Because this file was not staged separately here, it returns to the committed version.

### c. Recover an Older Committed Script

Recall that the previous commit (`HEAD~1`) still contains the uncorrected `30.0 C` reading.

```bash
git log --oneline -- calculate_mean.py
git restore --source=HEAD~1 -- calculate_mean.py
python calculate_mean.py
git diff # the version now in the working directory is the one from the older commit . The restored file is not automatically  commited
git log --oneline
```

Expected output from the restored older script:

```text
Mean water temperature: 20.75 C
```

**Ask:** Have the commits disappeared? Is the restored older file automatically committed?

> Instructor Note: No. This changes only the working-tree version of `calculate_mean.py`. The complete commit history remains available. `git status` reports the file as modified, assuming the old and current versions differ.

### d. Return to the Latest Committed Script

```bash
git restore --source=HEAD -- calculate_mean.py
python calculate_mean.py
git status # the version of the script is restored the last comitted version , so the working tree is clean
```

Expected output:

```text
Mean water temperature: 18.25 C
```

> Instructor Note: This final restore discards the temporary old-version working-tree contents. **Do not** run `git add` and `git commit` here: restoring the latest version results in no new changes to commit.


### Exercise

Go back to your `mini-field-study` folder and continue with exercises (Lesson_development/Material_for_participants/git_research_workflow_exercises.md) Part 5. 

✅ **End of Day 1.** Keep the `field-study` repository: it becomes the local repository connected to GitHub on Day 2.

## Recap
- we have learned how to create a local git repository from scratch 
- we have learned the cycle of modified, add and commit a file 
- we have learned that we can tell git what to ignore using the .gitignore file 
    - also pptx, word documents , pdfs, excel spreadsheets are not recognizable line by line by Git
    - only files that when opened in a text editor are human readable are suitable for git to track, plain text files
- we have learned how to explore the history of commits
- we have learned how to move in the history without changing it 

Tomorrow,

- we will turn this solo project into a collaborative project with Github. 

## Add your feedback in the collaborative document 

- +1 one thing you like/keep it 
- -1 one thing you want to improve
---

## 🛰️ Day 2: Working with Remotes and Collaborating on GitHub

### 🧭  — Recap of local Git
### 🧭  — Intro to Remotes
### 🧭  — Push/Pull

> **Instructor Note:** Introduce the idea of remote repositories using slides. Explain what GitHub is, how it connects to local repositories, and why remotes are important for collaboration and backup.

---
### 🌐 Connecting to GitHub [20 min]
#### a. Create a GitHub Repository
> Ask participants to log into GitHub and create an **empty public repository** named `field-study`.

* Description suggestion: **Analysis of different water temperature measurements**

#### b. Connect Local Repo to Remote
##### b0. SSH Setup for GitHub Access (**Technical Break**, 30 min)
> **Instructor Note:** Use slides or a diagram to explain SSH authentication, its benefits over HTTPS, and GitHub’s two-factor authentication requirements.

Steps to generate and configure an SSH key:

```bash
cd ~
ssh-keygen -t ed25519 -C "your_email@example.com"
# Choose default location: ~/.ssh/id_ed25519
```

✅ **Email requirements:**

* The email in this command doesn't need to match your GitHub account.

* Still, using a verified GitHub email is recommended for clarity and organization.

Check generated files:

```bash
ls ~/.ssh/
```

Start the SSH agent and add the key:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

Copy the public key:

```bash
clip < ~/.ssh/id_ed25519.pub  # Windows
# or
cat ~/.ssh/id_ed25519.pub     # For manual copy
```

On GitHub:

* Go to **Settings → SSH and GPG keys → New SSH key**

* Paste the key and name it clearly (e.g., `workshop-laptop`)

Test the SSH connection:

```bash
ssh -T git@github.com
```

🔗 Resources:

* [GitHub SSH Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

* [Troubleshooting SSH](https://docs.github.com/en/authentication/troubleshooting-ssh)

#### b1. Add the Remote and Push
Return to your project folder:

```bash
cd ~/Desktop/field-study
```

Link to the remote repository:

```bash
git remote add origin git@github.com:<your-username>/field-study.git
git remote -v
```

Set default branch and push:

```bash
git branch -M main
git push -u origin main
```

To pull future updates:

- Explore the GUI of Github
- shows the commits, hashes and how to go back in time with <>
- edit the .gitignore file with `Presentations/`
- commit it
- pull to local 

```bash
git pull origin main
```

> Questions? Clarify remote tracking, default branches, and pushing/pulling.

---
### 🖥️ Optional: Explore GitHub Web GUI
> **Instructor Note:** Quickly tour the GitHub interface: file browser, commit history, branches, issues, pull requests.

---
## ☕ 10:45 - 11:00 – Break
Invite participants to join `workshop-check-in` repo as collaborators before resuming.

---
11:00 - 12:00 

## 🤝 Collaboration Practice: Hands-On Session issues? 
### a. Clone a Shared Repository
> Instructor shares repo URL: `https://github.com/leilaicruz/workshop-checkin`

Clone it locally:

```bash
cd ~/Desktop
git clone git@github.com:leilaicruz/workshop-checkin.git workshop-checkin  
```

> **Instructor Note:** Now contrast cloning vs forking using the table below.

| Aspect                | 🌀 Cloning                      | 🍴 Forking                               |
| --------------------- | ------------------------------- | ---------------------------------------- |
| **What it does**      | Local copy of existing repo     | Your own copy under your GitHub account  |
| **Who owns the repo** | The original owner              | You (your GitHub username)               |
| **Use case**          | Team projects with write access | Contributing to external/public projects |
| **How to contribute** | Direct push or PRs to original  | PRs from fork to upstream                |
| **Remote origin**     | Points to source repo           | Points to your fork                      |

### b. Make a Check-in Entry
Create a copy of the template and edit it:

```bash
cd workshop-checkin
cp template.md <first-last-letters-name-first-two-numbers-postal-code>.md
nano la26.md
```

> Example name: `la26.md`. Add your name, research field and your ideal job an ideal world.

### c. Collaborative Git Workflow
A typical workflow:

```bash
git pull origin main
git add .
git commit -m "check <your-name> in"
git push origin main
```

> **Note:** This requires that participants have been added as **collaborators**.

---
### 🪄Branches and Pull Requests 

Create and switch to a new branch:

```bash
git switch -c <your-branch-name>
```

Edit your check-in file, stage, commit, and push:

```bash
git add check-in/<your-id>.md
git commit -m "add check-in info"
git push origin <your-branch-name>
```

> Instructor can then demonstrate how to open a Pull Request (PR) via the GitHub interface.

---
## 🧩 Final Exercise: Working with Issues and PRs
> Instructor Note: Display this on a shared screen or slide.

**demo before assignment**

### Instructor demonstration: One issue, one proposed solution, one review

**1. Owner: describe a problem using a GitHub issue**

In your `field-study` repository, open **Issues → New issue**.

**Example title:** `Add a README for reproducibility of the analysis`

**Example description:**

> A new team member cannot tell what this repository contains or how to run the Python analysis. Add a `README.md` containing the research question, a description of the files, instructions for running `calculate_mean.py`, the expected result, and notes about data provenance. Link to `methods.md` for methodological details.
>


**2. Collaborator: fork, clone, and create a branch**

Fork the owner's repository on GitHub. Then, from outside any existing `field-study` repository:

```bash
cd ~/Desktop
git clone https://github.com/<collaborator-username>/field-study.git field-study-contribution
cd field-study-contribution
git switch -c improve_readme
```

Make the requested change:

```bash
nano README.md
```
Inside nano writes: 

```markdown

# Water Temperature Field Study

## Research question

What is the average water temperature recorded during our field study?


## How to reproduce the analysis

**Requirements:** Python 3 (no external packages needed).

Run the following command from the repository folder: `python calculate_mean.py`


**Expected result:** Mean water temperature:18.25 C

See methods.md for further methodological details.
```


```bash
git status
git add README.md
git diff --staged
git commit -m "Document how to reproduce the field study"
git push -u origin improve_readme

``` 

**3. Collaborator: open a pull request**

On GitHub, open a PR with:

- **Base repository:** the owner's `field-study`; **base branch:** `main`.
- **Head repository:** the collaborator's fork; **compare branch:** `improve-readme`.
- **PR description:** Explain what changed, why, and how it was checked. Link the issue (for example, `Closes #12`, replacing `12` with the real issue number).

**4. Owner: review before merging**

On GitHub, open the PR's **Files changed** tab. Look for missing instructions, confusing claims about the source data, or undocumented assumptions. Leave a comment or approve the PR.

**Optional — test the collaborator's branch locally using commands from this course:**

```bash
cd ~/Desktop
git clone --branch improve_readme https://github.com/<collaborator-username>/field-study.git  field-study-review
cd field-study-review
git branch -a
git switch improve-readme
git log -1 --name-only
cat Readme.md
```

Here, `--branch improve-readme` selects the proposed branch from the collaborator's **fork**. The clone goes into a separate `field-study-review` directory, so it does not overwrite the owner's local project. For a README-only PR, verify that the documented command and output are correct.

**5. Owner: merge, then synchronize the original local repository**

After review, merge the PR in GitHub and reply a friendly message to the collaborator. Then return to your **original** local project:

```bash
cd ~/Desktop/field-study
git pull origin main
git log --oneline
git status
```


**Assignment:**

 Go to Lesson_development/Material_for_participants/field_study_issues_pull_requests_exercise.md

---
### 🪪 12. Licensing & Citation [Optional, 5 min]
> Instructor Note: Briefly explain why software licensing matters for reuse and collaboration.

* Share TU Delft software template repo: [https://github.com/manuGil/fair-code](https://github.com/manuGil/fair-code)

Encourage:

* Adding a LICENSE file

* Including CITATION metadata in the repo

---
### ❓ 13. Q&A
Open floor for questions, clarifications, and sharing experiences.
