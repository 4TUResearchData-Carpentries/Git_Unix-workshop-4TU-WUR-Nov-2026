## Exercise: Improve a Research Project Through Issues and Pull Requests (Day 2)

**Estimated time:** 30–40 minutes (in pairs)  
**Scenario:** Your `field-study` project is now on GitHub. Another researcher wants to reuse your water-temperature analysis, but notices that something could be improved. Instead of editing the owner's `main` branch directly, the team will propose, review, and integrate a change.



### 🔬 Pair challenge: Maka real improvement to your `field-study` repository

Follow the collaborative workflow : **1. Owner: describe a problem using a GitHub issue** -> **2. Collaborator: fork, clone, and create a branch** -> **3. Collaborator: open a pull request** -> **4. Owner: review before merging** -> **5. Owner: merge, then synchronize the original local repository**

Switch roles so both learners experience proposing and reviewing changes. Create an issue in the owner's repository, then complete the same contribution workflow. 

Title of the issue: **Make the project reusable: create `README.md`**

Description: Explain the research question, contents, requirements, how to run the analysis, what results to expect, and how the data and methods are documented. Use the starter template below, replacing all placeholders with accurate information. In this lesson, the four fictional measurements are embedded in `calculate_mean.py`; the ignored `raw-data/` file is **not automatically read** by the script.


### README starter template

Create a file named `README.md` in the repository root. Replace the bracketed prompts rather than leaving them as-is.

````markdown
# Field Study — Water Temperature Analysis

[One sentence explaining what the project investigates and why.]

## Research question

[What question does this small field study explore?]

## Repository contents

- `calculate_mean.py` — [What does the Python script do?]
- `methods.md` — [Where are the measurement and quality-control decisions explained?]
- `raw-data/` — [Explain that this local directory is ignored by Git.]

## Requirements

- Python 3 [Add any additional dependencies, if applicable.]

## Run the analysis

From the repository directory:

```bash
python calculate_mean.py
```

Expected output after the correction:

```text
Mean water temperature: 18.25 C
```

````

---

### 💬 Final reflection

1. What information in the issue helped the collaborator understand what to do?
2. How did the PR show the owner **what changed** before merging?
3. Why is reviewing a change different from accepting it immediately?
4. How did `git pull origin main` change the owner's local repository **after** the merge?
5. What would another researcher need in the README to reproduce the calculation?

> **Instructor Note:** For a shorter session, demonstrate the README issue and let pairs choose just one of Options B–E. If there is not enough time to merge, completing the review and explaining the next step still meets most of the learning goals. If a contributor already has write access to the owner's repository, they can work on a feature branch there instead of forking; the review principle remains the same.
