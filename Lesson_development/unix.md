
# Unix lesson

## Intro message

The Unix shell is older than most of the people who use it. It has
survived so long because it is one of the most productive programming
environments ever created --- maybe even *the* most productive. Its syntax
may be cryptic, but people who have mastered it can experiment with
different commands interactively, then use what they have learned to
automate their work. Graphical user interfaces may be easier to use at
first, but once learned, the productivity in the shell is unbeatable.
And as Alfred North Whitehead wrote in 1911, 'Civilization advances by
extending the number of important operations which we can perform
without thinking about them.'

- `export PS1="$ "` to clear long name before the commands

## 09:30–09:40 (10 min) — Introducing the Shell

**Goals:** what the shell is, prompt, first `ls`.
**Format:** quick demo + short discussion.


- Let's get started.

When the shell is first opened, you are presented with a **prompt**,
indicating that the shell is waiting for input.

```bash
$ # Shell prompt: system is ready to receive commands
```

```bash
$ ls # List files in current directory → first inspection step in any workflow
```

**Exercises (rapid picks):**

## Command not found

If the shell can't find a program whose name is the command you typed, it
will print an error message such as:

```bash
$ ks # Attempt to run a command → demonstrates commands must exist and be spelled correctly
```

```output
ks: command not found
```

This might happen if the command was mis-typed or if the program corresponding to that command
is not installed.

---

## 09:40–10:05 (25 min) — Navigating Files & Directories

**Topics:** `pwd`, `ls`, `cd`; absolute vs relative; `-F`, `-a`; `--help`/`man`.
**Format:** demo → guided practice.

::::::::::::::::::::::::::::::::::::::: objectives

- Explain the similarities and differences between a file and a directory.
- Translate an absolute path into a relative path and vice versa.
- Construct absolute and relative paths that identify specific files and directories.
- Use options and arguments to change the behaviour of a shell command.
- Demonstrate the use of tab completion and explain its advantages.

:::::::::::::::::::::::::::::::::::::::: questions

- How can I move around on my computer?
- How can I see what files and directories I have?
- How can I specify the location of a file or directory on my computer?

::::::::::::::::::::::::::::::::::: commands

- Copy the shell-lesson-data folder inside Desktop to start with 

`pwd` shows you where you are, and usually whwre the terminal starts is called the home directory:

```bash
$ pwd # Print Working Directory → always know where your analysis is running
```

Two main concepts of the Unix shell:

- home directory (I can show the figure of the Carpentry - https://swcarpentry.github.io/shell-novice/fig/filesystem.svg)

    The filesystem looks like an upside down tree. 
    The topmost directory  is the **root directory**
    that holds everything else.
    We refer to it using a slash character, `/`, on its own;
    this character is the leading slash in `/Users/nelle`.

    Inside that directory are several other directories:
    `bin` (which is where some built-in programs are stored),
    `data` (for miscellaneous data files),
    `Users` (where users' personal directories are located),
    `tmp` (for temporary files that don't need to be stored long-term),
    and so on.

- Slashes

Notice that there are two meanings for the `/` character.
When it appears at the front of a file or directory name,
it refers to the root directory. When it appears *inside* a path,
it's just a separator.


Now let's learn the command that will let us see the contents of our
own filesystem.  We can see what's in our home directory by running `ls`:

```bash
$ ls # Inspect current directory contents
```

```bash
$ ls -F # Adds markers to distinguish file types → helps understand structure
```

    - a trailing `/` indicates that this is a directory
    - `@` indicates a link
    - `*` indicates an executable

`ls` has lots of other **options**. There are two common ways to find out how
to use a command and what options it accepts 

```bash
  $ ls --help # Built-in documentation → essential for self-learning
```
- Examples
```bash
$ ls -l -h   # Detailed listing + human-readable sizes → useful for dataset inspection
```

- Exploring other directories

```bash
$ ls -F Desktop
```

- Change our location to a different directory, so we are no longer located in
our home directory.

Let's say we want to move into the `exercise-data` directory we saw above. We can
use the following series of commands to get there:

```bash
$ cd Desktop
$ cd shell-lesson-data
$ cd exercise-data
```

- We now know how to go down the directory tree (i.e. how to go into a subdirectory),
but how do we go up (i.e. how do we leave a directory and go into its parent directory)?

There is a shortcut in the shell to move up one directory level. It works as follows:

```bash
$ cd .. # `..` is a special directory name meaning "the directory containing this one",or the **parent** of the current directory.
```

- How to find the parent directory shortcut

```bash
$ ls -F -a # Show hidden files and structure
```

    - it also displays another special directory that's just called `.`,
which means 'the current working directory'

These three commands are the basic commands for navigating the filesystem on your computer:
`pwd`, `ls`, and `cd`. Let's explore some variations on those commands. What happens
if you type `cd` on its own, without giving
a directory?

```bash
$ cd  # Return to home directory → safe reset point
```

## Two More Shortcuts

The shell interprets a tilde (`~`) character at the start of a path to
mean "the current user's home directory". For example, if Nelle's home
directory is `/Users/nelle`, then `~/data` is equivalent to
`/Users/nelle/data`. This only works if it is the first character in the
path; `here/there/~/elsewhere` is *not* `here/there/Users/nelle/elsewhere`.

Another shortcut is the `-` (dash) character. `cd` will translate `-` into
*the previous directory I was in*, which is faster than having to remember,
then type, the full path.  This is a *very* efficient way of moving
*back and forth between two directories* -- i.e. if you execute `cd -` twice,
you end up back in the starting directory.

The difference between `cd ..` and `cd -` is
that the former brings you *up*, while the latter brings you *back*.

Try it!
First navigate to `~/Desktop/shell-lesson-data` (you should already be there).

```bash
$ cd ~/Desktop/shell-lesson-data # Use ~ as home shortcut → portable paths
```

Then `cd` into the `exercise-data/creatures` directory

```bash
$ cd exercise-data/creatures # Navigate using relative path
```

Now if you run

```bash
$ cd -  # Return to previous directory → efficient navigation
```

you'll see you're back in `~/Desktop/shell-lesson-data`.
Run `cd -` again and you're back in `~/Desktop/shell-lesson-data/exercise-data/creatures`

## Exercises

## Relative Path Resolution (https://swcarpentry.github.io/shell-novice/instructor/02-filedir.html)

Using the filesystem diagram below (https://swcarpentry.github.io/shell-novice/fig/filesystem-challenge.svg), if `pwd` displays `/Users/thing`,
what will `ls -F ../backup` display?

1. `../backup: No such file or directory`
2. `2012-12-01 2013-01-08 2013-01-27`
3. `2012-12-01/ 2013-01-08/ 2013-01-27/`
4. `original/ pnas_final/ pnas_sub/`

## Exercise 1 — Find your way through a research project 


## 10:05–10:25 (20 min) — Working with Files & Dirs (Part 1)

**Topics:** `mkdir`, editors (`nano`), `mv`.
**Format:** do-along.

::::::::::::::::::::::::::::::::::::::: objectives

- Delete, copy and move specified files and/or directories.
- Create files in that hierarchy using an editor or by copying and renaming existing files.
- Create a directory hierarchy that matches a given diagram.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I create, copy, and delete files and directories?
- How can I edit files?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: commands

## Creating directories
### Step one: see where we are and what we already have

We should still be in the `shell-lesson-data` directory on the Desktop,
which we can check using:

```bash
$ pwd  # Confirm location before modifying files
```


Next we'll move to the `exercise-data/writing` directory and see what it contains:

```bash
$ cd exercise-data/writing/
$ ls -F
# Enter directory and inspect contents
```

```output
haiku.txt  LittleWomen.txt
```

### Create a directory

Let's create a new directory called `thesis` using the command `mkdir thesis`
(which has no output):

```bash
$ mkdir thesis # Create directory → organizing outputs
```

## Good names for files and directories

Complicated names of files and directories can make your life painful
when working on the command line. Here we provide a few useful
tips for the names of your files and directories.

1. Don't use spaces.

Spaces can make a name more meaningful,
but since spaces are used to separate arguments on the command line
it is better to avoid them in names of files and directories.
You can use `-` or `_` instead (e.g. `north-pacific-gyre/` rather than `north pacific gyre/`).
To test this out, try typing `mkdir north pacific gyre` and see what directory (or directories!)
are made when you check with `ls -F`.

2. Don't begin the name with `-` (dash).

Commands treat names starting with `-` as options.

3. Stick with lowercase letters, numbers, `.` (period or 'full stop'), `-` (dash) and `_` (underscore).

### Create a text file

Let's change our working directory to `thesis` using `cd`,
then run a text editor called Nano to create a file called `draft.txt`:

```bash
$ cd thesis
$ nano draft.txt  # Create/edit text file → scripts and data are text
```

write in nano: "It's not publish or perish any more, it's share and thrive :)"


## Creating Files a Different Way

We have seen how to create text files using the `nano` editor.
Now, try the following command:

```bash
$ touch my_file.txt # Create empty file
```
## Removing a file 

```bash
$ rm my_file.txt # Remove file → irreversible, be cautious
```
## Moving files and directories

Returning to the `shell-lesson-data/exercise-data/writing` directory,

```bash
$ cd ~/Desktop/shell-lesson-data/exercise-data/writing
```

In our `thesis` directory we have a file `draft.txt`
which isn't a particularly informative name,
so let's change the file's name using `mv`,
which is short for 'move':

```bash
$ mv thesis/draft.txt thesis/quotes.txt  # Rename file (move within same directory)
```

Let's move `quotes.txt` into the current working directory.
We use `mv` once again,
but this time we'll use just the name of a directory as the second argument
to tell `mv` that we want to keep the filename
but put the file somewhere new.
(This is why the command is called 'move'.)
In this case,
the directory name we use is the special directory name `.` that we mentioned earlier.

```bash
$ mv thesis/quotes.txt . # Move file into current directory
```

test by :

```bash
$ ls thesis # Verify directory contents
```

```output
$
```

## Copying files and directories

```bash
$ cp quotes.txt thesis/quotations.txt # Copy file → safe experimentation
$ ls quotes.txt thesis/quotations.txt
```
We can also copy a directory and all its contents by using the
[recursive](https://en.wikipedia.org/wiki/Recursion) option `-r`,
e.g. to back up a directory:

```bash
$ cp -r thesis thesis_backup # Recursive copy → backup directory
```

test:

```bash
$ ls thesis thesis_backup
```

## Removing files and directories

Returning to the `shell-lesson-data/exercise-data/writing` directory,
let's tidy up this directory by removing the `quotes.txt` file we created.
The Unix command we'll use for this is `rm` (short for 'remove'):

```bash
$ rm quotes.txt # Delete file
```

```bash
$ rm -r thesis_backup/ # Delete directory recursively
```




## Wildcards

`*` is a **wildcard**, which represents zero or more other characters.
Let's consider the `shell-lesson-data/exercise-data/alkanes` directory:

```bash

ls *.pdb # Match all files ending in .pdb → batch processing

```

 On the other hand, `p*.pdb` only represents
`pentane.pdb` and `propane.pdb`, because the 'p' at the front can only
represent filenames that begin with the letter 'p'.

```bash

ls p*.pdb # every file that begin with p and end in .pdb  

```

`?` is also a wildcard, but it represents exactly one character.

```bash

ls ?ethane.pdb # Exactly one character before 'ethane', only represent `methane.pdb`

```

```bash

ls *ethane.pdb # # Any prefix ending in 'ethane' , represents both `ethane.pdb` and `methane.pdb`.

```

```bash

ls ???ethane.pdb # indicates three characters followed by `ane.pdb`, giving `cubane.pdb  ethane.pdb  octane.pdb`

```



When the shell sees a wildcard, it expands the wildcard to create a
list of matching filenames *before* running the preceding command.

As an exception, if a wildcard expression does not match
any file, Bash will pass the expression as an argument to the command
as it is. For example, typing `ls *.pdf` in the `alkanes` directory
(which contains only files with names ending with `.pdb`) results in
an error message that there is no file called `*.pdf`.

However, generally commands like `wc` and `ls` see the lists of
file names matching these expressions, but not the wildcards
themselves. It is the shell, not the other programs, that expands
the wildcards.

- `*` matches zero or more characters in a filename, so `*.txt` matches all files ending in `.txt`.
- `?` matches any single character in a filename, so `?.txt` matches `a.txt` but not `any.txt`.

:::::::::::::::::::::::::::::::::::::::  challenge

## List filenames matching a pattern

https://swcarpentry.github.io/shell-novice/instructor/03-create.html#operations-with-multiple-files-and-directories


When run in the alkanes directory, which ls command(s) will produce this output?

ethane.pdb methane.pdb

1. ls *t*ane.pdb
2. ls *t?ne.*
3. ls *t??ne.pdb
4. ls ethane.*

The solution is 3.

---

## 10:25–10:45 (20 min) — Pipes & Filters (Essentials)

**Topics:** `wc`, `cat`, `sort -n`, `head`, `tail`; redirection `>` `>>`; pipes `|`.
**Format:** instructor demo → pair exercise.

::::::::::::::::::::::::::::::::::::::: objectives

- Explain the advantage of linking commands with pipes and filters.
- Combine sequences of commands to get new output
- Redirect a command's output to a file.
- Explain what usually happens if a program or pipeline isn't given any input to process.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I combine existing commands to produce a desired output?
- How can I show only part of the output? 

:::::::::::::::::::::::::::::::::::::::::::::::::: commands 

We'll start with the directory `shell-lesson-data/exercise-data/alkanes`
that contains six files describing some simple organic molecules.
The `.pdb` extension indicates that these files are in Protein Data Bank format,
a simple text format that specifies the type and position of each atom in the molecule.


```bash
$ ls
```

```output
cubane.pdb    methane.pdb    pentane.pdb
ethane.pdb    octane.pdb     propane.pdb
```

Let's run an example command:

```bash
$ wc cubane.pdb # Count lines, words, characters → basic data summary
```

```output
20  156 1158 cubane.pdb
```

`wc` is the 'word count' command:
it counts the number of lines, words, and characters in files (returning the values
in that order from left to right).

If we run the command `wc *.pdb`, the `*` in `*.pdb` matches zero or more characters,
so the shell turns `*.pdb` into a list of all `.pdb` files in the current directory:

```bash
$ wc *.pdb # Count lines, words, characters in multiple files → dataset size comparison
```

If we run `wc -l` instead of just `wc`,
the output shows only the number of lines per file:

```bash
$ wc -l *.pdb # Count lines in multiple files → dataset size comparison
```
The `-m` and `-w` options can also be used with the `wc` command to show
only the number of characters or the number of words, respectively.

## Capturing output from commands

Which of these files contains the fewest lines?
It's an easy question to answer when there are only six files,
but what if there were 6000?
Our first step toward a solution is to run the command:

```bash
$ wc -l *.pdb > lengths.txt # Redirect output to file → reusable data
```
```bash
$ cat lengths.txt # Display file contents
```
## Sorting and filtering output

```bash
$ sort -n lengths.txt # Sort numerically

```

```bash
$ sort -n lengths.txt > sorted-lengths.txt
$ head -n 1 sorted-lengths.txt
```
## Passing output to another command

In our example of finding the file with the fewest lines,
we are using two intermediate files `lengths.txt` and `sorted-lengths.txt` to store output. but we dont need to do that, for that we have the pipe functionality of the unix shell. 

```bash
$ wc -l *.pdb | sort -n |  head -n 1 # Pipeline → combine tools without intermediate files
```

The vertical bar, `|`, between the two commands is called a **pipe**.
It tells the shell that we want to use
the output of the command on the left
as the input to the command on the right.


### Coming back to Nelle's pipeline

## Nelle's Pipeline: Checking Files

Nelle has run her samples through the assay machines
and created 17 files in the `north-pacific-gyre` directory described earlier.
As a quick check, starting from the `shell-lesson-data` directory, Nelle types:

```bash
$ cd north-pacific-gyre
$ wc -l *.txt
```

The output is 18 lines that look like this:

```output
300 NENE01729A.txt
300 NENE01729B.txt
300 NENE01736A.txt
300 NENE01751A.txt
300 NENE01751B.txt
300 NENE01812A.txt
... ...
```

Now she types this:

```bash
$ wc -l *.txt | sort -n | head -n 5
```

```output
 240 NENE02018B.txt
 300 NENE01729A.txt
 300 NENE01729B.txt
 300 NENE01736A.txt
 300 NENE01751A.txt
```

Whoops: one of the files is 60 lines shorter than the others.
When she goes back and checks it,
she sees that she did that assay at 8:00 on a Monday morning --- someone
was probably in using the machine on the weekend,
and she forgot to reset it.
Before re-running that sample,
she checks to see if any files have too much data:

```bash
$ wc -l *.txt | sort -n | tail -n 5
```

```output
 300 NENE02040B.txt
 300 NENE02040Z.txt
 300 NENE02043A.txt
 300 NENE02043B.txt
5040 total
```

Those numbers look good --- but what's that 'Z' doing there in the third-to-last line?
All of her samples should be marked 'A' or 'B';
by convention,
her lab uses 'Z' to indicate samples with missing information.
To find others like it, she does this:

```bash
$ ls *Z.txt
```

```output
NENE01971Z.txt    NENE02040Z.txt
```

Sure enough,
when she checks the log on her laptop,
there's no depth recorded for either of those samples.
Since it's too late to get the information any other way,
she must exclude those two files from her analysis.
She could delete them using `rm`,
but there are actually some analyses she might do later where depth doesn't matter,
so instead, she'll have to be careful later on to select files using the wildcard expressions
`NENE*A.txt NENE*B.txt`.

:::::::::::::::::::::::::::::::::::::::: keypoints

- `wc` counts lines, words, and characters in its inputs.
- `cat` displays the contents of its inputs.
- `sort` sorts its inputs.
- `head` displays the first 10 lines of its input by default without additional arguments.
- `tail` displays the last 10 lines of its input by default without additional arguments.
- `command > [file]` redirects a command's output to a file (overwriting any existing content).
- `command >> [file]` appends a command's output to a file.
- `[first] | [second]` is a pipeline: the output of the first command is used as the input to the second.
---

### ☕ 10:45–11:00 — Coffee break

---

## 11:00–11:25 (25 min) — Loops (Intro)

**Topics:** `for` loops over files; variables; quoting; history `↑`, `Ctrl-R`.
**Format:** demo → short practice.

::::::::::::::::::::::::::::::::::::::: objectives

- Write a loop that applies one or more commands separately to each file in a set of files.
- Trace the values taken on by a loop variable during execution of the loop.
- Explain the difference between a variable's name and its value.
- Explain why spaces and some punctuation characters shouldn't be used in file names.
- Demonstrate how to see what commands have recently been executed.
- Re-run recently executed commands without retyping them.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I perform the same actions on many different files?

::::::::::::::::::::::::::::::::::::::::::::::::::

**Loops** are a programming construct which allow us to repeat a command or set of commands
for each item in a list.
As such they are key to productivity improvements through automation.
Similar to wildcards and tab completion, using loops also reduces the
amount of typing required (and hence reduces the number of typing mistakes).

Suppose we have several hundred genome data files named `basilisk.dat`, `minotaur.dat`, and
`unicorn.dat`.
For this example, we'll use the `exercise-data/creatures` directory which only has three
example files, but the principles can be applied to many many more files at once.

The structure of these files is the same: the common name, classification, and updated date are
presented on the first three lines, with DNA sequences on the following lines.
Let's look at the files:

```bash
$ head -n 5 basilisk.dat minotaur.dat unicorn.dat
```

Problem: We would like to print out the classification for each species, which is given on the second
line of each file.

Solution: For each file, we would need to execute the command `head -n 2` and pipe this to `tail -n 1`.
We'll use a loop to solve this problem, but first let's look at the general form of a loop,
using the code below:

```bash
$ for filename in basilisk.dat minotaur.dat unicorn.dat  # Loop over files → automate repetitive tasks
> do
>     echo $filename
>     head -n 2 $filename | tail -n 1
> done
```


## Those Who Know History Can Choose to Repeat It

Another way to repeat previous work is to use the `history` command to
get a list of the last few hundred commands that have been executed, and
then to use `!123` (where '123' is replaced by the command number) to
repeat one of those commands. For example, if Nelle types this:

```bash
$ history | tail -n 5 # Show recent commands → reproducibility and debugging
```

- A `for` loop repeats commands once for every thing in a list.
- Every `for` loop needs a variable to refer to the thing it is currently operating on.
- Use `$name` to expand a variable (i.e., get its value). `${name}` can also be used.
- Give files consistent names that are easy to match with wildcard patterns to make it easy to select them for looping.
- Use `history` to display recent commands, and `!number` to repeat a command by number.

---

## 11:25–11:55 (30 min) — Shell Scripts

**Topics:** writing/running scripts; `$1`, `$2`, `$@`; quoting args; comments; simple pipelines in scripts; debugging with `bash -x`.
**Format:** mini-lecture → build one script → practice.

::::::::::::::::::::::::::::::::::::::: objectives

- Write a shell script that runs a command or series of commands for a fixed set of files.
- Run a shell script from the command line.
- Write a shell script that operates on a set of files defined by the user on the command line.
- Create pipelines that include shell scripts you, and others, have written.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I save and re-use commands?

::::::::::::::::::::::::::::::::::::::::::::::::::

Let's start by going back to `alkanes/` and creating a new file, `middle.sh` which will
become our shell script:

```bash
$ cd alkanes
$ nano middle.sh
```

The command `nano middle.sh` opens the file `middle.sh` within the text editor 'nano'
(which runs within the shell).
If the file does not exist, it will be created.
We can use the text editor to directly edit the file by inserting the following line:

```source
head -n 15 octane.pdb | tail -n 5
```source

Once we have saved the file,
we can ask the shell to execute the commands it contains.
Our shell is called `bash`, so we run the following command:

```bash
$ bash middle.sh
```

**What if we want to select lines from an arbitrary file?**

We could edit `middle.sh` each time to change the filename,
but that would probably take longer than typing the command out again
in the shell and executing it with a new file name.
Instead, let's edit `middle.sh` and make it more versatile:

```bash
$ nano middle.sh
```

Now, within "nano", replace the text `octane.pdb` with the special variable called `$1`:

```source
head -n 15 "$1" | tail -n 5
```

Inside a shell script,
`$1` means 'the first filename (or other argument) on the command line'.
We can now run our script like this:

```bash
$ bash middle.sh octane.pdb
```

or on a different file like this:

```bash
$ bash middle.sh pentane.pdb
```

**What if we want to select a different range of lines?**

Currently, we need to edit `middle.sh` each time we want to adjust the range of
lines that is returned.
Let's fix that by configuring our script to instead use three command-line arguments.
After the first command-line argument (`$1`), each additional argument that we
provide will be accessible via the special variables `$1`, `$2`, `$3`,
which refer to the first, second, third command-line arguments, respectively.

Knowing this, we can use additional arguments to define the range of lines to
be passed to `head` and `tail` respectively:

```bash
$ nano middle.sh
```

```source
head -n "$2" "$1" | tail -n "$3"
```

We can now run:

```bash
$ bash middle.sh pentane.pdb 15 5
```

**What if we want to process many files in a single pipeline?**

For example, if we want to sort our `.pdb` files by length, we would type:

```bash
$ wc -l *.pdb | sort -n
```

because `wc -l` lists the number of lines in the files
(recall that `wc` stands for 'word count', adding the `-l` option means 'count lines' instead)
and `sort -n` sorts things numerically.
We could put this in a file,
but then it would only ever sort a list of `.pdb` files in the current directory.
If we want to be able to get a sorted list of other kinds of files,
we need a way to get all those names into the script.
We can't use `$1`, `$2`, and so on
because we don't know how many files there are.
Instead, we use the special variable `$@`,
which means,
'All of the command-line arguments to the shell script'.
We also should put `$@` inside double-quotes
to handle the case of arguments containing spaces
(`"$@"` is special syntax and is equivalent to `"$1"` `"$2"` ...).

Here's an example:

```bash
$ nano sorted.sh
```

```source
# Sort files by their length.
# Usage: bash sorted.sh one_or_more_filenames
wc -l "$@" | sort -n
```

```bash
$ bash sorted.sh *.pdb ../creatures/*.dat
```
## Nelle's Pipeline: Creating a Script (optional)

Nelle's supervisor insisted that all her analytics must be reproducible.
The easiest way to capture all the steps is in a script.

First we return to Nelle's project directory:

```bash
$ cd ../../north-pacific-gyre/
```

She creates a file using `nano` ...

```bash
$ nano do-stats.sh
```

...which contains the following:

```bash
# Calculate stats for data files.
for datafile in "$@"
do
    echo $datafile
    bash goostats.sh $datafile stats-$datafile
done
```

She saves this in a file called `do-stats.sh`
so that she can now re-do the first stage of her analysis by typing:

```bash
$ bash do-stats.sh NENE*A.txt NENE*B.txt
```
bash 
She can also do this:

```bash
$ bash do-stats.sh NENE*A.txt NENE*B.txt | wc -l
```

so that the output is just the number of files processed
rather than the names of the files that were processed.


---

## 11:55–12:25 (30 min) — Finding Things (grep + find)

**Topics:** `grep` basics (`-n`, `-w`, `-i`, `-v`, `-r`), regex teaser; `find` (`-type`, `-name` with quotes), command substitution `$()`, combining `find` + `grep`.
**Format:** demo → two focused exercises.

::::::::::::::::::::::::::::::::::::::: objectives

- Use `grep` to select lines from text files that match simple patterns.
- Use `find` to find files and directories whose names match simple patterns.
- Use the output of one command as the command-line argument(s) to another command.
- Explain what is meant by 'text' and 'binary' files, and why many common tools don't handle the latter well.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I find files?
- How can I find things in files?

In the same way that many of us now use 'Google' as a
verb meaning 'to find', Unix programmers often use the
word 'grep'.
'grep' is a contraction of 'global/regular expression/print',
a common sequence of operations in early Unix text editors.
It is also the name of a very useful command-line program.

`grep` finds and prints lines in files that match a pattern.

```bash
$ cd
$ cd Desktop/shell-lesson-data/exercise-data/writing
$ cat haiku.txt
```

Let's find lines that contain the word 'not':

```bash
$ grep not haiku.txt # Search for pattern in file → text querying
```

By default, grep searches for a pattern in a **case-sensitive way**.
In addition, the search pattern we have selected does not have to form a complete word,
as we will see in the next example.

Let's search for the pattern: 'The'.

```bash
$ grep The haiku.txt
```

To restrict matches to lines containing the word 'The' on its own,
we can give `grep` the `-w` option.
This will limit matches to word boundaries.

Later in this lesson, we will also see how we can change the search behavior of grep
with respect to its case sensitivity.

```bash
$ grep -w The haiku.txt
```


Note that a 'word boundary' includes the start and end of a line, so not
just letters surrounded by spaces.
Sometimes we don't
want to search for a single word, but a phrase. We can also do this with
`grep` by putting the phrase in quotes.

```bash
$ grep -w "is not" haiku.txt
```

Another useful option is `-n`, which numbers the lines that match:

```bash
$ grep -n "it" haiku.txt
```

```output
5:With searching comes loss
9:Yesterday it worked
10:Today it is not working
```

Now, we want to use the option `-v` to invert our search, i.e., we want to output
the lines that do not contain the word 'the'.

```bash
$ grep -n -w -v "the" haiku.txt
```

If we use the `-r` (recursive) option,
`grep` can search for a pattern recursively through a set of files in subdirectories.

Let's search recursively for `Yesterday` in the `shell-lesson-data/exercise-data/writing` directory:

```bash
$ grep -r Yesterday .
```

While `grep` finds lines in files,
the `find` command finds files themselves. we'll use the `shell-lesson-data/exercise-data`
directory tree shown below.

```bash

cd shell-lesson-data/exercise-data

ls -r . 

```

```output
.
├── animal-counts/
│   └── animals.csv
├── creatures/
│   ├── basilisk.dat
│   ├── minotaur.dat
│   └── unicorn.dat
├── numbers.txt
├── alkanes/
│   ├── cubane.pdb
│   ├── ethane.pdb
│   ├── methane.pdb
│   ├── octane.pdb
│   ├── pentane.pdb
│   └── propane.pdb
└── writing/
    ├── haiku.txt
    └── LittleWomen.txt
```

For our first command,
let's run `find .` (remember to run this command from the `shell-lesson-data/exercise-data` folder).

```bash
$ find .
```

The first option in our list is
`-type d` that means 'things that are directories'.
Sure enough, `find`'s output is the names of the five directories (including `.`):

```bash
$ find . -type d
```


Now let's try matching by name:

```bash
$ find . -name *.txt
```




---- exercise

How can we combine that with `wc -l` to count the lines in all those files?

The simplest way is to put the `find` command inside `$()`:

```bash
$ wc -l $(find . -name "*.txt")
```


:::::::::::::::::::::::::::::::::::::::: keypoints

- `find` finds files with specific properties that match patterns.
- `grep` selects lines in files that match patterns.









