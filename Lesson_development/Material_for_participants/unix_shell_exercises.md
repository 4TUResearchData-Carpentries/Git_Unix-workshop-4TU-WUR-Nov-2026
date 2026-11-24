# Unix Shell — 10 Reasoning & Practice Exercises

**Learner worksheet** | Beginner level | Approximately **50–60 minutes**

These exercises revisit the main ideas from the Unix Shell workshop without reproducing the instructor's demonstrations. Some are **mental exercises** (no terminal needed), some are **True/False**, and others ask you to **write or test Bash commands**. More than one valid command sequence may exist.

**Before you begin**

- Use **Bash** (for example, Git Bash on Windows).
- For the practical tasks, you need the `shell-lesson-data` directory from the [Software Carpentry Unix Shell lesson](https://swcarpentry.github.io/shell-novice/index.html), extracted on your Desktop.
- Keep the original lesson data unchanged. Create and use a separate `shell-practice/` directory for the tasks that modify files.
- For mental exercises, **predict first**, then optionally verify in a terminal.
- Write a short explanation wherever a question asks *why*; a correct command without understanding its effect is only half the answer.

---

## Exercise 1 — Find your way through a research project

**Type:** Mental reasoning · **Focus:** Relative and absolute paths, `cd`, `pwd` · **~5 min**

Imagine this directory structure:

```text
/home/ada/
└── coastal-project/
    ├── data/
    │   ├── raw/
    │   │   ├── station_A.csv
    │   │   └── station_B.csv
    │   └── processed/
    ├── scripts/
    │   └── clean.sh
    └── reports/
```

Your current directory is `/home/ada/coastal-project/data/raw`.

1. What is the absolute path of `clean.sh`?
2. What relative path would you use to access `clean.sh` **without changing directory**?
3. Predict the directory printed by `pwd` after these commands:

   ```bash
   cd ../..
   cd scripts
   cd ../data/processed
   pwd
   ```

4. Starting from `data/raw`, would `cd ../processed` work? Explain why.

---

## Exercise 2 — Unix myths: True or False?

**Type:** True/False + explain · **Focus:** Basic concepts and safe file handling · **~5 min**

Mark each statement **T** or **F**. For every false statement, correct it in one sentence.

| # | Statement | T / F |
|---|---|---|
| A | `ls -a` can reveal names beginning with a dot (`.`). | |
| B | `cd -` and `cd ..` always go to the same directory. | |
| C | In `ls -F`, a `/` after a name indicates a directory. | |
| D | `mv results.csv archive/` always leaves the original file in its previous location. | |
| E | `>` appends new output to the end of an existing file. | |
| F | In a filename pattern, `?` represents exactly one character. | |
| G | `history` can help you recover commands used earlier in a session. | |
| H | `rm` normally sends deleted files to your graphical Recycle Bin / Trash. | |

**Discuss:** Which false statement could cause the most trouble in a real research project?

---

## Exercise 3 — Create a safe workspace

**Type:** Develop commands (hands-on) · **Focus:** `mkdir`, `cp`, `mv`, `ls`, paths · **~6 min**

A colleague wants to test changes to a data file without modifying the researcher's original file.

Starting from `~/Desktop/shell-lesson-data`, write and run commands to:

1. Create a directory called `shell-practice`, and enter it.
2. Inside it, create **two** directories: `originals` and `results`.
3. Copy `unicorn.dat` from `../exercise-data/creatures/` into `originals/`, keeping its filename.
4. Create a **second copy** in `results/` named `unicorn-working.dat`.
5. Rename the file in `results/` to `unicorn-check.dat` **without changing the copy in `originals/`**.
6. Inspect both directories to verify the result.

**Reasoning question:** Why is `cp` appropriate in steps 3 and 4, while `mv` is appropriate in step 5?

> If you have already created `shell-practice/`, enter that directory and continue; do not delete or overwrite existing work.

---

## Exercise 4 — Wildcard detective

**Type:** Mental reasoning · **Focus:** `*` versus `?`, matching filenames · **~4 min**

Suppose a directory contains **only** these files:

```text
sample_A.csv
sample_B.csv
sample_A1.csv
sample_1.csv
sample_.csv
sample_X.txt
notes.csv
```

Without running the commands, list the filenames selected by each pattern:

1. `ls sample_?.csv`
2. `ls sample_*.csv`
3. `ls sample_A*.csv`

Then answer:

4. Which pattern above excludes `sample_A1.csv` but still selects `sample_A.csv`?
5. Why would `rm sample_*.csv` be a risky way to clean up this directory?

> **Safety:** Do not run the `rm` command in question 5. It is included for reasoning only.

---

## Exercise 5 — Does the order of a pipeline matter?

**Type:** Predict the output · **Focus:** `head`, `sort -n`, pipes · **~4 min**

The file `measurements.txt` contains one number per line, in this order:

```text
12
4
9
1
8
3
```

Predict the output of each command:

**A**

```bash
head -n 3 measurements.txt | sort -n
```

**B**

```bash
sort -n measurements.txt | head -n 3
```

1. Write the exact three lines of output for A and B.
2. Which command would you use to obtain the **three smallest measurements from the entire file**?
3. Explain in your own words why these pipelines produce different results.

---

## Exercise 6 — Save a report without losing information

**Type:** Develop commands + reasoning · **Focus:** `wc`, `>`, `>>`, `cat` · **~5 min**

Assume your current directory contains `records/` (with several `.csv` files) and an existing empty directory `reports/`.

Write commands that:

1. Save the **line counts** for all CSV files in `records/` to `reports/checks.txt` (replacing the previous report if it exists).
2. **Append** the **word counts** for those same files to `reports/checks.txt`.
3. Display the resulting report in the terminal.

**Reasoning questions:** What would happen if you used `>` in step 2? What would happen if you ran only step 2 twice?

---

## Exercise 7 — Count many files without a summary total

**Type:** Develop a `for` loop · **Focus:** Variables, wildcards, loops, quoting · **~6 min**

In a new, hypothetical project, `data/` contains several files named `sample*.csv`, and `reports/` already exists.

You want `reports/line-counts.txt` to have **one line per sample file**, showing its number of lines and filename, but **no `total` line**.

1. Write a Bash `for` loop that uses `wc -l` on each matching file separately, and saves all results to the report. Assume at least one file matches.
2. How is the output different from running `wc -l data/sample*.csv` once, assuming at least two files match?
3. Why is it good practice to write `"$file"` rather than `$file` within the loop?

---

## Exercise 8 — Debug a reusable shell script

**Type:** Find and fix the mistakes · **Focus:** `$1`, `"$@"`, shell scripts, `bash -x` · **~6 min**

A colleague has saved this as `preview.sh`:

```bash
# Show the first two lines of every file given to this script.
for file in "$1"
do
    echo $file
    head -n 2 $file
done
```

They run:

```bash
bash preview.sh data/sample_A.csv data/sample_B.csv
```

1. Why does the script process only one file?
2. Rewrite the script so it prints the name and first two lines of **every** file supplied on the command line. Make it safe for filenames containing spaces.
3. Write the command to run your corrected script on **both** example files.
4. If the script behaves unexpectedly, how can `bash -x` help?

---

## Exercise 9 — Finding a file is not the same as finding text

**Type:** Develop commands + explain · **Focus:** `find`, `grep`, pattern quoting · **~6 min**

Imagine you are in this `study/` directory:

```text
study/
├── field/
│   ├── note_A.txt
│   └── sample_A.csv
├── lab/
│   └── note_B.txt
└── README.md
```

1. Write a command to locate **all `.txt` files** anywhere beneath the current directory. Show only files, not directories.
2. Write a command to search **recursively** inside `field/` and `lab/` for lines containing the **whole word `warning`**, regardless of case, including line numbers in the output.
3. Explain why this command can behave unexpectedly:

   ```bash
   find . -name *.txt
   ```

4. In one sentence, describe the main difference between `find` and `grep`.

---

## Exercise 10 — Mini-challenge: a reproducible data audit

**Type:** Integrated hands-on challenge · **Focus:** Copies, wildcards, loops, pipes, `grep`, scripts, redirection · **~10–12 min**

A researcher has three `.dat` files describing imaginary creatures. Each file contains a `CLASSIFICATION` line and several lines of sequence data. They want a repeatable audit **without modifying the source files**.

Start in the `shell-practice/` directory created in Exercise 3, inside `~/Desktop/shell-lesson-data`.

**Part A — Prepare the workspace**

1. Create a `data/` directory.
2. Copy **all** `.dat` files from `../exercise-data/creatures/` into `data/`. Do not move the originals.

**Part B — Write a reusable script**

Create a file `audit.sh` that accepts **one or more filenames as command-line arguments**. When run, it should create two reports in the existing `results/` directory:

- `file-lengths.txt`: one line count per file, **numerically sorted from shortest to longest**, with **no total line**.
- `classifications.txt`: the filename, line number, and line containing `CLASSIFICATION` for each input file.

Use commands covered in the lesson, including a loop, a pipe, and output redirection. **Hint:** `grep -H` can force filename display even when searching just one file; discover its meaning with `grep --help`. Add a short comment describing what your script does.

**Part C — Use and reflect**

3. Write the command to run `audit.sh` on **all `.dat` files** in `data/`.
4. Write commands to inspect the two reports.
5. Why is keeping this workflow in a script more reproducible than relying only on commands typed from memory?

**Optional discussion:** What would you inspect first if one of the data files had far fewer lines than the others?

---

## Reflection

Which idea changed the way you think about the shell most: **paths**, **wildcards**, **pipes**, **loops**, or **scripts**? Give one concrete example of how you might use it in your own research or data-support work.

---

*Designed for the Unix Shell workshop, drawing on the themes and learning objectives of [Software Carpentry: The Unix Shell](https://swcarpentry.github.io/shell-novice/index.html). Scenarios and exercises are newly written for this worksheet.*
