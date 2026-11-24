# Unix Shell — Instructor Solutions to the 10 Exercises

**Companion to:** `unix_shell_exercises.md`  
**Level:** Beginner | **Format:** Model answers, explanations, and discussion prompts

These are **example solutions**, not the only possible correct answers. Where students use different commands, assess whether they achieve the intended result **without modifying the original data**. All command examples assume **Bash**.

---

## Exercise 1 — Find your way through a research project

1. Absolute path:

   ```text
   /home/ada/coastal-project/scripts/clean.sh
   ```

2. Relative path from `data/raw`:

   ```text
   ../../scripts/clean.sh
   ```

3. The commands move from `data/raw` up to `coastal-project`, into `scripts`, then across to `data/processed`. So `pwd` prints:

   ```text
   /home/ada/coastal-project/data/processed
   ```

4. **Yes.** `..` moves from `data/raw` to its parent, `data`; `processed` is another directory inside `data`.

**Teaching point:** Paths are interpreted relative to the **current working directory**, not the directory containing the file you last viewed. An absolute path begins at `/`.

---

## Exercise 2 — Unix myths: True or False?

| # | Answer | Explanation or correction |
|---|---|---|
| A | **True** | `ls -a` includes normally hidden names, such as `.config` and `.` / `..`. |
| B | **False** | `cd ..` goes to the **parent** directory; `cd -` returns to the **previous working directory**. |
| C | **True** | `ls -F` adds `/` to directory names. |
| D | **False** | `mv` normally **moves or renames** the file; it does not preserve the original at its old path. |
| E | **False** | `>` **replaces** the destination file's existing contents; `>>` appends. |
| F | **True** | `?` matches exactly one character in an unquoted filename pattern. |
| G | **True** | `history` can show recent commands, depending on shell history configuration. |
| H | **False** | `rm` normally removes the file without using the graphical Trash/Recycle Bin. |

**Discussion:** E and H are particularly high-risk: overwriting a report with `>` or removing files with `rm` may lose work. D can also matter when a researcher mistakenly thinks `mv` makes a backup.

---

## Exercise 3 — Create a safe workspace

From `~/Desktop/shell-lesson-data`, an example solution is:

```bash
cd ~/Desktop/shell-lesson-data
mkdir shell-practice
cd shell-practice
mkdir originals results

# Copy from the Carpentries lesson dataset into our own workspace.
cp ../exercise-data/creatures/unicorn.dat originals/unicorn.dat

# Make a second independent copy for experimentation.
cp originals/unicorn.dat results/unicorn-working.dat

# Rename only the working copy.
mv results/unicorn-working.dat results/unicorn-check.dat

# Inspect both locations.
ls -F originals results
```

Expected names (the ordering and directory headings displayed by `ls` can vary):

```text
originals/:
unicorn.dat

results/:
unicorn-check.dat
```

`cp` creates a new copy and preserves the source file; `mv` changes the filename/location of the existing working copy.

**Teaching point:** `cp` is a better choice than `mv` when making a safe working copy of research data. Students should confirm the current directory before file operations. If the workspace already exists, omit `mkdir shell-practice`; do not delete it just to rerun the exercise.

---

## Exercise 4 — Wildcard detective

1. `sample_?.csv` matches **exactly one character** between `_` and `.csv`:

   ```text
   sample_1.csv
   sample_A.csv
   sample_B.csv
   ```

2. `sample_*.csv` matches **zero or more characters** in that position:

   ```text
   sample_.csv
   sample_1.csv
   sample_A.csv
   sample_A1.csv
   sample_B.csv
   ```

3. `sample_A*.csv` matches names starting with `sample_A` and ending in `.csv`:

   ```text
   sample_A.csv
   sample_A1.csv
   ```

4. `sample_?.csv` excludes `sample_A1.csv` while including `sample_A.csv`. It **also** matches `sample_B.csv` and `sample_1.csv`; it does not restrict the one character to letters.

5. `rm sample_*.csv` would select **all five matching CSV files**, including files that may not have been intended for deletion. A wildcard is expanded to multiple filenames before `rm` executes. **Inspect the selection first** using `ls sample_*.csv`.

**Teaching point:** `*` can match **zero** characters, which is why `sample_.csv` is included. Neither wildcard understands the scientific meaning of a filename.

---

## Exercise 5 — Does the order of a pipeline matter?

**A** — `head` first selects `12`, `4`, `9`; `sort -n` then sorts those three:

```text
4
9
12
```

**B** — `sort -n` first sorts **all six** values; `head` selects the first three:

```text
1
3
4
```

To retrieve the smallest three values in the entire dataset, choose **B**:

```bash
sort -n measurements.txt | head -n 3
```

**Teaching point:** A pipeline sends one program's output into the next, so **order changes the meaning** of the operation. `sort` without `-n` would sort values lexicographically rather than numerically.

---

## Exercise 6 — Save a report without losing information

```bash
# Start (or replace) a report with line counts.
wc -l records/*.csv > reports/checks.txt

# Add word counts beneath the existing line counts.
wc -w records/*.csv >> reports/checks.txt

# Inspect the combined report.
cat reports/checks.txt
```

- Using `>` in step 2 would **overwrite** the line counts saved in step 1, leaving only word counts.
- Running step 2 twice would **append the word-count section twice**, creating duplicated information.
- Since `wc` is called on multiple files at once, its output normally includes a `total` row for each section.

**Teaching point:** Shell redirection is powerful but silent: a command can overwrite a file without asking for confirmation.

---

## Exercise 7 — Count many files without a summary total

One solution (assuming at least one match) is:

```bash
for file in data/sample*.csv
do
    wc -l "$file"
done > reports/line-counts.txt
```

Or, equivalently, on one line:

```bash
for file in data/sample*.csv; do wc -l "$file"; done > reports/line-counts.txt
```

The output has one line per matching file, such as:

```text
12 data/sample_A.csv
18 data/sample_B.csv
```

**Why no total?** Each `wc` invocation receives **one file**, unlike `wc -l data/sample*.csv` which receives several filenames in a single invocation and normally adds a `total` row when at least two files are supplied.

**Why quote `"$file"`?** Quoting makes the expanded filename a **single argument**, even if it contains spaces or wildcard-like characters. `for file in data/sample*.csv` intentionally leaves the initial pattern unquoted, so Bash can expand it to matching filenames.

**Teaching point:** A loop gives you finer control over *per-file* operations than one command run on all files together.

---

## Exercise 8 — Debug a reusable shell script

1. `"$1"` represents **only the first argument** passed to the script. The loop therefore receives a one-item list, even if more filenames were supplied.

2. Corrected `preview.sh`:

   ```bash
   # Show the first two lines of every input file.
   for file in "$@"
   do
       echo "$file"
       head -n 2 "$file"
   done
   ```

3. Run it with:

   ```bash
   bash preview.sh data/sample_A.csv data/sample_B.csv
   ```

   If a filename contains spaces, quote that argument too, e.g. `bash preview.sh "data/sample A.csv"`.

4. To trace the commands as Bash executes them:

   ```bash
   bash -x preview.sh data/sample_A.csv data/sample_B.csv
   ```

   Bash displays the commands and their expanded arguments as it runs the script, which helps identify incorrect variable expansions or paths.

**Teaching point:** `"$@"` is the safe way to represent **all positional arguments as separate arguments**. Do not substitute `"$*"`: it behaves differently by combining them into one word.

---

## Exercise 9 — Finding a file is not the same as finding text

1. Find text files by **name**, recursively from the current directory:

   ```bash
   find . -type f -name "*.txt"
   ```

   It matches `./field/note_A.txt` and `./lab/note_B.txt` in the given tree.

2. Search recursively within both subdirectories for the whole word `warning`, case-insensitively, with line numbers:

   ```bash
   grep -r -n -i -w "warning" field/ lab/
   ```

   Equivalent short-option form:

   ```bash
   grep -rniw "warning" field/ lab/
   ```

3. In `find . -name *.txt`, **Bash may expand** `*.txt` using files in the *current directory* before `find` receives the pattern. That may pass an unintended filename or multiple arguments. With **no match**, Bash's usual default is to pass the literal expression onward, so it can appear to work by accident. Using `"*.txt"` ensures `find` receives the pattern and matches it at every level.

4. **`find` searches for filesystem entries based on properties such as names and types; `grep` searches for matching lines inside file contents.**

**Teaching point:** There are two different pattern-matching stages: shell expansion for unquoted wildcards, and the pattern interpretation performed by a tool such as `find`.

---

## Exercise 10 — Mini-challenge: a reproducible data audit

### Part A — Prepare the workspace

Assuming `shell-practice/` and `results/` were created in Exercise 3:

```bash
cd ~/Desktop/shell-lesson-data/shell-practice
mkdir data
cp ../exercise-data/creatures/*.dat data/
ls -F data
```

The copies should be `basilisk.dat`, `minotaur.dat`, and `unicorn.dat`. The files in `../exercise-data/creatures/` remain untouched.

### Part B — Write `audit.sh`

Use `nano audit.sh` to create this script:

```bash
#!/usr/bin/env bash
# Audit all files supplied as arguments without modifying them.

# Count each file separately to avoid wc's summary total.
for file in "$@"
do
    wc -l "$file"
done | sort -n > results/file-lengths.txt

# Save matching classification lines, with filenames and line numbers.
grep -Hn "CLASSIFICATION" "$@" > results/classifications.txt
```

**Why it works:**

- `"$@"` supplies all filenames individually, including names with spaces.
- The `for` loop runs `wc -l` **once per file**, so there is no combined `total` line.
- `sort -n` orders the line counts numerically.
- `>` writes each report file (replacing any previous report of the same name).
- `grep -n` prints matching lines with line numbers; the additional `-H` option ensures filenames appear **even when only one file** is supplied. This option can be found via `grep --help`.

### Part C — Run, inspect, reflect

```bash
bash audit.sh data/*.dat
cat results/file-lengths.txt
cat results/classifications.txt
```

For the classification report, each matching output line has the general form:

```text
data/basilisk.dat:2:CLASSIFICATION: ...
```

The corresponding lines for the other files are likewise included. Exact counts and text depend on the files distributed with the lesson.

A script records the sequence of operations and can be **reused consistently**, inspected, shared, and maintained. A researcher can rerun the same audit after receiving updated data without remembering a series of interactive commands.

**Optional discussion:** A much shorter file could indicate incomplete acquisition, a processing problem, or a legitimate biological difference. First inspect its beginning and end (`head`, `tail`) and compare its structure with other files. The shell reveals a potential issue; scientific context determines whether it is an actual error.

**Teaching point:** This is an end-to-end example of *automation without losing provenance*: original files remain intact; derived results are stored separately; a script documents how they were produced.

---

## Suggested facilitation notes

- **Exercises 1, 4, 5:** Ask pairs to *predict* before checking; these work well as quick whole-room questions.
- **Exercise 2:** Ask participants to justify the false statements. Pay particular attention to `rm` and `>`.
- **Exercises 3, 6, 7, 9:** Let participants compare alternative valid commands. Focus on the effect, not identical syntax.
- **Exercise 8:** Ask why quotes around `"$@"` and `"$file"` solve **different** potential problems.
- **Exercise 10:** Check both the generated reports and the **unchanged source files**. Invite participants to explain each stage as a research workflow rather than a list of commands.

### Topics covered at a glance

| Exercise | Main topics |
|---|---|
| 1 | Filesystem hierarchy; absolute/relative paths; `cd`, `pwd` |
| 2 | `ls` options; navigation; `mv`; `>`; wildcards; history; `rm` safety |
| 3 | `mkdir`, `cp`, `mv`, `ls`; non-destructive workflows |
| 4 | `*`, `?`; shell filename expansion; safe selection |
| 5 | `head`, `sort -n`, pipelines; command order |
| 6 | `wc`, `>`, `>>`, `cat`; report generation |
| 7 | `for`, variables, quoting; per-file processing |
| 8 | Scripts, `$1`, `"$@"`, `bash`, `bash -x` |
| 9 | `find`, `grep`, `-type`, `-name`, `-r`, `-n`, `-i`, `-w` |
| 10 | Integrated data-audit workflow and reproducibility |

*References for teaching context: [Software Carpentry — The Unix Shell](https://swcarpentry.github.io/shell-novice/index.html). Exercises and explanations in this handout are original adaptations to the user's workshop scope.*
