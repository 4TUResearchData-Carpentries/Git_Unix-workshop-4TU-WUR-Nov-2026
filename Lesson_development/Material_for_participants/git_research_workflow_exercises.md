# Git Challenge: A Mini Research Project

**Suggested time:** 25–30 minutes (plus optional bonus)  
**Goal:** Build a small research project and explain what is **modified**, **staged**, and **committed**.

You are investigating rainfall in a city. Your colleague asks whether the result has changed since the last analysis. You need a clear record of the code, methods, and research decisions. Try solving each part before opening its hints.

**Important:** This is an **independent practice repository**. First navigate outside your instructor's `field-study` repository and create a new repository called `mini-field-study`.

## Part 1 — Create your research project

1. Create a new directory called `mini-field-study` outside any existing Git repository and initialize Git inside it.
2. Create `analysis.py` with the following starting code:

   ```python
   rainfall = [2.0, 6.0, 8.0]

   def calculate_mean(values):
       return sum(values) / len(values)

   print(f"Mean rainfall: {calculate_mean(rainfall):.2f} mm")
   ```

3. Run the script with Python. What mean do you obtain?
4. Create `methods.md` containing three lines: the project question, the units (`mm`), and how you calculate the mean.
5. Use `git status` to inspect both new files.
6. Stage **only `analysis.py`**. Check `git status` again. Is `methods.md` staged too?
7. Stage `methods.md` and create **one meaningful commit** containing both files.
8. **Checkpoint:** Identify which files were included in the first commit *without opening their contents*.

<details>
<summary>🔍 Click here for hints!</summary>

- Use `cd ..` to leave your current repository, then check `pwd`.
- Use `mkdir mini-field-study`, `cd mini-field-study`, and `git init`.
- Edit a file with `nano analysis.py` or `nano methods.md`.
- Run the script with `python analysis.py` (or `python3 analysis.py`).
- Check the repository with `git status`.
- Use `git add analysis.py` and, separately, `git add methods.md`.
- Create one commit with `git commit -m "your descriptive message"`.
- Try `git log -1 --name-only` to inspect the files included in the latest commit.

</details>

## Part 2 — The research results change

You discover that the last measurement was recorded incorrectly: it should have been `10.0 mm`, not `8.0 mm`.

1. Update the last number in `analysis.py` from `8.0` to `10.0`.
2. Add one line to `methods.md` explaining the correction and why it was made.
3. Before staging, inspect your changes. Can you identify the modified lines?
4. Stage **only `analysis.py`**.
5. Compare the results of `git status`, `git diff`, and `git diff --staged`.
6. **Predict:** What would be recorded if you committed *now*?
7. Stage `methods.md`, review everything staged, and create **one commit** for the correction.
8. Run the script again. Did the calculated mean change?



<details>
<summary>🔍 Click here for hints!</summary>

- Open files with `nano filename` and save them before running Git commands.
- `git diff` shows **unstaged changes** in tracked files.
- `git diff --staged` shows changes prepared for the next commit.
- `git status` identifies files that are modified or staged.
- Stage a single file with `git add analysis.py`.
- Only the content staged when `git commit` runs will enter that commit.
- Use `git add methods.md` before the final commit if you want both files included.
- Use `git log -1 --name-only` to verify the changed file names.

</details>

## Part 3 — Some files should stay local

A new folder will contain original raw observations that you will store somewhere else, rather than in this demonstration repository.

1. Create an empty directory named `raw-data`.
2. Run `git status`. Does Git show your empty directory? Why?
3. Create `raw-data/rainfall.csv` with the following content:

   ```csv
   site,rainfall_mm
   A,2.0
   B,6.0
   C,10.0
   ```

4. Check `git status` again.
5. Create `.gitignore` and configure it to ignore the entire `raw-data/` directory.
6. Check that Git recognizes the ignore rule.
7. Commit **only the `.gitignore` file**.


**Research reflection:** Ignoring raw data is a choice for this exercise, not a universal best practice. Small, non-sensitive datasets are often appropriate to version-control. Sensitive or large datasets may need controlled storage and clear documentation of how to access them.

<details>
<summary>🔍 Click here for hints!</summary>

- Use `mkdir raw-data` to create the directory.
- Remember that Git does not record empty directories.
- Create a file using `nano raw-data/rainfall.csv`.
- Create the ignore file with `nano .gitignore`.
- Write `raw-data/` on a line in `.gitignore`.
- Inspect ignored paths with `git status --ignored`.
- For a specific path, try `git check-ignore -v raw-data/rainfall.csv`.
- Stage only `.gitignore` using `git add .gitignore` and commit it.

</details>

## Part 4 — Can you reconstruct what happened?

You should now have **exactly three commits**.

1. Display your full commit history.
2. Display the same history in a compact one-line format.
3. Identify which commit first introduced `analysis.py` and `methods.md`.
4. Identify which commit changed the rainfall measurement.
5. Display the files changed in the latest commit.
6. Compare the **first and latest committed versions** of `analysis.py`. Can you locate the corrected measurement?
7. Verify that your working tree is clean.

**Checkpoint:** Explain the difference between `git log -1 --name-only` and `git diff HEAD~2 HEAD -- analysis.py`.

<details>
<summary>🔍 Click here for hints!</summary>

- Use `git log` to explore history; press `q` to exit its pager.
- Use `git log --oneline` to show one line per commit.
- `HEAD` refers to the current commit, while `HEAD~2` is two first-parent steps older.
- Use `git log --oneline -- analysis.py` to view commits that changed the script.
- Use `git log -1 --name-only` to list files changed in the latest commit.
- Use `git diff HEAD~2 HEAD -- analysis.py` to compare the first commit with the latest one.
- Use `git status` to check for pending changes.

</details>

## Part 5 — Recover a previous version safely

You want to understand the analysis **before** the rainfall correction, but you do not want to delete any commits.

1. Locate the commit in which `analysis.py` originally contained `8.0`.
2. Restore **only the working copy** of `analysis.py` from that older commit.
3. Run the script. What result do you get now?
4. Inspect `git status` and `git diff`.
5. Does `git log` still show all three commits?
6. Restore `analysis.py` to its latest committed version and check that the working tree is clean.

**Checkpoint:** What changed when you used `git restore`? What did *not* change?

> **Caution:** Restoring a file can discard uncommitted work. For this exercise, make sure you have nothing else in `analysis.py` that you still need to keep.

<details>
<summary>🔍 Click here for hints!</summary>

- Use `git log --oneline -- analysis.py` to find the relevant older version.
- If you followed the exercise exactly, the initial version is `HEAD~2`.
- Restore the older working file with `git restore --source=HEAD~2 -- analysis.py`.
- Inspect the changes with `git diff` and inspect commit history with `git log --oneline`.
- Return to the latest version with `git restore --source=HEAD -- analysis.py`.
- `git restore --source=... -- file` changes a working file; it does not delete older commits or create a new commit automatically.

</details>




Adapted from the [Software Carpentry Git lesson](https://swcarpentry.github.io/git-novice/), using a new fictional research scenario and original challenges.
