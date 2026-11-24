
## 🛰️ Day 2: Working with Remotes and Collaborating on Gitlab WUR instance

### 🧭  — Recap of local Git
### 🧭  — Intro to Remotes
### 🧭  — Push/Pull

> **Instructor Note:** Introduce the idea of remote repositories using slides. Explain what Gitlab WUR instance is, how it connects to local repositories, and why remotes are important for collaboration and backup.

---

### 🌐 Connecting to Gitlab WUR instance \[20 min]

#### a. Create a Gitlab WUR instance Repository

> Ask participants to log into Gitlab WUR instance and create an **empty public repository** named `field-study`.

* Description suggestion: **Analysis of different water temperature measurements**
* Instructor should ensure that the Issues (Work items) are enabled when you create the repository

#### b. Connect Local Repo to Remote

##### b0. SSH Setup for Gitlab WUR instance Access (**Technical Break**, 30 min)

> **Instructor Note:** Use slides or a diagram to explain SSH authentication, its benefits over HTTPS, and Gitlab WUR instance’s two-factor authentication requirements.

Steps to generate and configure an SSH key:

```bash
cd ~
ssh-keygen -t ed25519 -C "your_email@example.com"
# Choose default location: ~/.ssh/id_ed25519
```

✅ **Email requirements:**

* The email in this command doesn't need to match your Gitlab WUR instance account.
* Still, using a verified Gitlab WUR instance email is recommended for clarity and organization.

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
clip < ~/.ssh/id_ed25519.pub  # Windows
# or
cat ~/.ssh/id_ed25519.pub     # For manual copy
```

On Gitlab WUR instance:

* Go to **Settings → SSH and GPG keys → New SSH key**
* Paste the key and name it clearly (e.g., `workshop-laptop`)

Test the SSH connection:

```bash
ssh -T git@git.wur.nl 
```

🔗 Resources:

* [Gitlab WUR instance SSH Docs](https://docs.gitlab.com/user/ssh/)
* [Troubleshooting SSH](https://docs.gitlab.com/user/ssh_troubleshooting/)

#### b1. Add the Remote and Push

Return to your project folder:

```bash
cd ~/Desktop/field-study
```

Link to the remote repository:

```bash
git remote add origin git@git.wur.nl.com:<your-username>/field-study.git
git remote -v
```

Set default branch and push:

```bash
git branch -M main
git push -u origin main
```

To pull future updates:

```bash
git pull origin main
```

> Questions? Clarify remote tracking, default branches, and pushing/pulling.

---

### 🖥️ Optional: Explore Gitlab WUR instance Web GUI

> **Instructor Note:** Quickly tour the Gitlab WUR instance interface: file browser, commit history, branches, issues, pull requests.

---

## ☕ 10:45 - 11:00 – Break

Invite participants to join `workshop-check-in` repo as collaborators before resuming.

---
11:00 - 12:00 
## 🤝 Collaboration Practice: Hands-On Session issues? 

### a. Clone a Shared Repository

> Instructor shares repo URL: `https://git.wur.nl/lingtong.gai/workshop-checkin`

Clone it locally:

```bash
cd ~/Desktop
git clone git@git.wur.nl:lingtong.gai/workshop-checkin.git
```

> **Instructor Note:** Now contrast cloning vs forking using the table below.

| Aspect                | 🌀 Cloning                      | 🍴 Forking                               |
| --------------------- | ------------------------------- | ---------------------------------------- |
| **What it does**      | Local copy of existing repo     | Your own copy under your Gitlab WUR instance account  |
| **Who owns the repo** | The original owner              | You (your Gitlab WUR instance username)               |
| **Use case**          | Team projects with write access | Contributing to external/public projects |
| **How to contribute** | Direct push or PRs to original  | PRs from fork to upstream                |
| **Remote origin**     | Points to source repo           | Points to your fork                      |

### b. Make a Check-in Entry

Create a copy of the template and edit it:

```bash
cd workshop-checkin
cp template.md <first-two-letters-name-last-two-postal-code>.md
nano lgxx.md
```

> Example name: `lg26.md`. Add your name, research field and your ideal job an ideal world.

### c. Collaborative Git Workflow

A typical workflow:

```bash
git pull origin main
git add lg26.md
git commit -m "check <your-name> in"
git push origin main
```

> **Note:** This requires that participants have been added as **collaborators**.

---

### 🪄 Optional: Branches and Pull Requests (if time allows)

Create and switch to a new branch:

```bash
git switch -c <your-branch-name>
```

Edit your check-in file, stage, commit, and push:

```bash
git add check-in/<your-id>.md
git commit -m "add check-in info"
git push -u origin <your-branch-name>
```

> Instructor can then demonstrate how to open a Merge Request (MR) via the Gitlab WUR instance interface.

---

## 🧩 Final Exercise: Working with Issues and MRs
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


---

### 🪪 12. Licensing & Citation \[Optional, 5 min]

> Instructor Note: Briefly explain why software licensing matters for reuse and collaboration.


Encourage:

* Adding a LICENSE file
* Including CITATION metadata in the repo : https://citation-file-format.github.io/cff-initializer-javascript/#/


---

### ❓ 13. Q\&A

Open floor for questions, clarifications, and sharing experiences.
