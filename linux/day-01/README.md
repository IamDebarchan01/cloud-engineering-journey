
## 📚 Linux Learning Roadmap

                              LINUX
                                │
                    ┌───────────┴───────────┐
                    │                       │
                 LEVEL 1                 LEVEL 2
                    │                       │
              File System            Users & Groups
              Files                  Permissions
              Directories            Processes
              Commands               Services
                    │                       │
                    └───────────┬───────────┘
                                │
                             LEVEL 3
                                │
                    ┌───────────┼───────────┐
                    │           │           │
                Networking      SSH        Logs
                    │           │           │
                    └───────────┼───────────┘
                                │
                             LEVEL 4
                                │
                    ┌───────────┼───────────┐
                    │           │           │
                  Storage      Bash       Packages
                    │           │           │
                    └───────────┼───────────┘
                                │
                             LEVEL 5
                                │
                     Security + Troubleshooting
                                │
                                ↓
                           CLOUD / AWS
```

---

# 🎯 Day 01 Goals

Today I focused on building a strong foundation in Linux.

### Topics Covered

* Basic Linux commands
* Files and directories
* Creating, deleting and moving files
* Reading file contents
* Searching text with `grep`
* Counting data with `wc`
* Basic text editing with `vi`
* Text processing with `sed`

---

# 1. 🖥️ Basic Linux Commands

## `whoami`

Displays the username of the currently logged-in user.

```bash
whoami
```

---

## `pwd`

Displays the **Present Working Directory**.

```bash
pwd
```

Example:

```text
/home/user
```

---

## `date`

Displays the current date and time.

```bash
date
```

---

## `cal`

Displays the calendar for the current month.

```bash
cal
```

Display the calendar for a specific year:

```bash
cal 2025
```

> Note: The year can be changed as required.

---

## `clear`

Clears the terminal screen.

```bash
clear
```

---

# 2. 📁 Working with Directories

## `mkdir`

Creates a new directory.

```bash
mkdir s3
mkdir dev
mkdir qa
```

Create multiple directories at once:

```bash
mkdir dev test prod
```

---

## `rmdir`

Removes an **empty** directory.

```bash
rmdir qa
```

> `rmdir` will not remove a directory containing files.

---

## `rm -rf`

Removes a directory and all of its contents.

```bash
rm -rf devops
```

> ⚠️ **Caution:** `rm -rf` permanently deletes files/directories. Always verify the path before executing it.

---

# 3. 📋 Listing Files and Directories

## `ls`

Lists files and directories in the current working directory.

```bash
ls
```

---

## `ls -l`

Displays files and directories in long/detailed format.

```bash
ls -l
```

---

## `ls -lr`

Displays the long listing in reverse order.

```bash
ls -lr
```

---

## `ls -lt`

Lists files with the newest entries first.

```bash
ls -lt
```

---

## `ls -ltr`

Lists files with the oldest entries first.

```bash
ls -ltr
```

---

## `ls -la`

Displays all files, including hidden files.

```bash
ls -la
```

### Commonly used combination

```bash
ls -ltr
```

This is especially useful when checking files based on modification time.

---

# 4. 📄 Creating Files

## `touch`

Creates an empty file.

```bash
touch t1.txt
```

Create multiple files:

```bash
touch t2.txt t3.txt
```

---

# 5. 🚶 Changing Directories

## `cd`

Changes the current working directory.

```bash
cd dir-name
```

Move to the parent directory:

```bash
cd ..
```

Go to the home directory:

```bash
cd ~
```

---

# 6. 🗑️ Removing Files

## `rm`

Removes a file.

```bash
rm t1.txt
```

Remove a directory and its contents:

```bash
rm -rf <dir-name>
```

Example:

```bash
rm -rf devops
```

> ⚠️ Be careful with `rm` and especially `rm -rf`.

---

# 7. 🔄 Moving and Renaming Files

## `mv`

The `mv` command is used to:

* Rename files
* Rename directories
* Move files
* Move directories

### Rename a file

```bash
mv existing-name new-name
```

Example:

```bash
mv old.txt new.txt
```

### Move a file

```bash
mv current-location new-location
```

Example:

```bash
mv t1.txt /tmp/
```

---

# 8. 📖 Reading and Creating File Content

## `cat`

The `cat` command can be used to create, append and display file contents.

### Create a file and enter data

```bash
cat > t1.txt
```

Type your content and press:

```text
Ctrl + D
```

to save and exit.

---

### Append data to an existing file

```bash
cat >> t1.txt
```

Type the additional content and press:

```text
Ctrl + D
```

---

### Display file contents

```bash
cat t1.txt
```

---

### Display file contents with line numbers

```bash
cat -n t1.txt
```

---

# 9. 🔃 `tac`

Displays file contents from bottom to top.

```bash
tac t1.txt
```

Example:

```text
Line 3
Line 2
Line 1
```

---

# 10. 📑 Copying Files

## `cp`

Copies a file.

```bash
cp t1.txt t2.txt
```

This creates `t2.txt` as a copy of `t1.txt`.

---

## Combining Files with `cat`

Multiple files can be combined into one file.

```bash
cat t1.txt t2.txt > t3.txt
```

This combines the contents of `t1.txt` and `t2.txt` into `t3.txt`.

---

# 11. 🔝 `head`

Displays the first 10 lines of a file by default.

```bash
head t1.txt
```

Display the first 20 lines:

```bash
head -n 20 t1.txt
```

Display the first 50 lines:

```bash
head -n 50 t1.txt
```

---

# 12. 🔚 `tail`

Displays the last 10 lines of a file by default.

```bash
tail t1.txt
```

Display the last 25 lines:

```bash
tail -n 25 t1.txt
```

### Real-world DevOps usage

`tail` is commonly used to monitor log files.

```bash
tail -f application.log
```

The `-f` option continuously displays new lines added to the file.

Press:

```text
Ctrl + C
```

to stop following the file.

---

# 13. 🔎 `grep`

`grep` is used to search for specific text within files.

It is commonly used for filtering and searching logs.

### Search for a keyword

```bash
grep 'hello' test.txt
```

---

### Case-insensitive search

```bash
grep -i 'hello' test.txt
```

This matches:

```text
hello
Hello
HELLO
```

---

### Show matching lines with line numbers

```bash
grep -n 'hello' test.txt
```

---

### Show lines that do NOT contain the keyword

```bash
grep -v 'hello' test.txt
```

---

### Search all files in the current directory

```bash
grep 'hello' *
```

---

### Combine `tail` and `grep`

Search for `hello` within the last 10 lines of a file:

```bash
tail test.txt | grep 'hello'
```

---

# 14. 🔢 `wc` — Word Count

`wc` is used to count lines, words and characters/bytes in a file.

## Display line, word and byte counts

```bash
wc test.txt
```

Example output:

```text
10  25  150 test.txt
```

The output represents:

```text
Lines   Words   Bytes   Filename
```

---

## Count lines

```bash
wc -l test.txt
```

---

## Count words

```bash
wc -w test.txt
```

---

## Count bytes

```bash
wc -c test.txt
```

---

## Count characters

```bash
wc -m test.txt
```

---

# 15. ✏️ Text Editors in Linux

## `vi`

`vi` is a classic text editor available on Unix/Linux systems.

It can be used to:

* Create files
* Edit files
* View files
* Modify existing content

Open or create a file:

```bash
vi t3.txt
```

If the file does not exist, `vi` creates it when the changes are saved.

---

# 16. ⌨️ Vi Modes

The basic `vi` workflow involves different modes.

### 1. Command Mode

This is the default mode when `vi` opens.

```bash
vi filename
```

In command mode, you can navigate and execute editor commands.

---

### 2. Insert Mode

Used to enter or modify text.

Press:

```text
i
```

Other commonly used insert commands include:

```text
a
o
```

---

### 3. Return to Command Mode

Press:

```text
Esc
```

This returns from Insert Mode to Command Mode.

---

## Save and Exit

From command mode:

```text
:wq
```

Meaning:

```text
:w  → write/save
:q  → quit
```

---

## Exit Without Saving

```text
:q!
```

---

# 17. 📝 File Creation Commands — Quick Reference

| Command      | Purpose                         |
| ------------ | ------------------------------- |
| `touch`      | Create an empty file            |
| `cat > file` | Create a file and enter content |
| `cp`         | Copy a file                     |
| `vi`         | Create/open a file for editing  |

Examples:

```bash
touch t1.txt
cat > t2.txt
cp t1.txt t2.txt
vi t3.txt
```

---

# 18. 👀 Reading File Data — Quick Reference

| Command | Purpose                             |
| ------- | ----------------------------------- |
| `cat`   | Display file contents top-to-bottom |
| `tac`   | Display file contents bottom-to-top |
| `head`  | Display beginning of a file         |
| `tail`  | Display end of a file               |
| `vi`    | Open a file for viewing/editing     |

Examples:

```bash
cat file.txt
tac file.txt
head file.txt
tail file.txt
vi file.txt
```

---

# 19. 🛠️ SED — Stream Editor

## What is `sed`?

`sed` stands for **Stream Editor**.

It is used for text processing and can perform operations such as:

* Substitution
* Deletion
* Printing
* Insertion
* Modification

One major advantage is that `sed` can process text **without opening the file in an editor**.

---

# 20. 🔄 SED — Substituting Text

Assume we have:

```text
data.txt
```

### Replace the first occurrence of `linux`

```bash
sed 's/linux/unix/' data.txt
```

---

### Replace the second occurrence

```bash
sed 's/linux/unix/2' data.txt
```

---

### Replace the third occurrence

```bash
sed 's/linux/unix/3' data.txt
```

---

### Replace all occurrences

```bash
sed 's/linux/unix/g' data.txt
```

The `g` means **global**, so every matching occurrence on each processed line is replaced.

---

## Save the Changes to the Original File

```bash
sed -i 's/linux/unix/g' data.txt
```

> ⚠️ `-i` modifies the original file. Use it carefully, especially on important configuration files.

---

# 21. 🗑️ SED — Deleting Lines

## Delete the first line

```bash
sed -i '1d' data.txt
```

---

## Delete the fourth line

```bash
sed -i '4d' data.txt
```

---

## Delete the last line

```bash
sed -i '$d' data.txt
```

---

## Delete from line `n` to the end

```bash
sed -i 'n,$d' data.txt
```

Replace `n` with the required line number.

Example:

```bash
sed -i '5,$d' data.txt
```

This deletes from line 5 through the last line.

---

## Delete lines 5 through 15

```bash
sed -i '5,15d' data.txt
```

---

# 22. 🖨️ SED — Printing Specific Lines

Print lines 10 through 20:

```bash
sed -n '10,20p' data.txt
```

Where:

```text
-n  → suppress normal output
p   → print the selected lines
```

---

# 23. ➕ SED — Inserting Text

Insert text before line 2:

```bash
sed '2i\I love Linux' data.txt
```

---

# 24. ➕ SED — Appending Text

Append text after the last line:

```bash
sed '$a\I am learning Linux' data.txt
```

---

# 25. 🧠 Day 01 Command Cheat Sheet

| Command   | Purpose                             |
| --------- | ----------------------------------- |
| `whoami`  | Show current user                   |
| `pwd`     | Show current directory              |
| `date`    | Show date and time                  |
| `cal`     | Display calendar                    |
| `clear`   | Clear terminal                      |
| `mkdir`   | Create directory                    |
| `rmdir`   | Remove empty directory              |
| `ls`      | List files/directories              |
| `ls -l`   | Long listing                        |
| `ls -la`  | Long listing including hidden files |
| `touch`   | Create empty file                   |
| `cd`      | Change directory                    |
| `rm`      | Remove files                        |
| `rm -rf`  | Remove directory recursively        |
| `mv`      | Move/rename                         |
| `cp`      | Copy                                |
| `cat`     | Create/display/concatenate          |
| `tac`     | Reverse file output                 |
| `head`    | Show beginning of file              |
| `tail`    | Show end of file                    |
| `tail -f` | Follow a changing file              |
| `grep`    | Search text                         |
| `wc`      | Count lines/words/bytes             |
| `vi`      | Edit files                          |
| `sed`     | Stream-based text processing        |

---

# 🧪 Day 01 Practice

## Exercise 1 — Directories

Create the following structure:

```text
linux-day01/
├── dev/
├── test/
└── prod/
```

Commands:

```bash
mkdir linux-day01
cd linux-day01
mkdir dev test prod
ls -la
```

---

## Exercise 2 — Files

Inside `dev`, create three files:

```text
app.txt
config.txt
log.txt
```

Commands:

```bash
cd dev
touch app.txt config.txt log.txt
ls -l
```

---

## Exercise 3 — File Content

Add some data to `app.txt`:

```bash
cat > app.txt
```

Enter some lines and press:

```text
Ctrl + D
```

Read the file:

```bash
cat app.txt
```

---

## Exercise 4 — Search

Search for a keyword:

```bash
grep 'linux' app.txt
```

Try:

```bash
grep -i 'linux' app.txt
grep -n 'linux' app.txt
grep -v 'linux' app.txt
```

---

## Exercise 5 — File Statistics

Check the number of lines, words and bytes:

```bash
wc app.txt
```

Then try:

```bash
wc -l app.txt
wc -w app.txt
wc -c app.txt
```

---

## Exercise 6 — Head and Tail

```bash
head app.txt
tail app.txt
```

Try:

```bash
head -n 5 app.txt
tail -n 5 app.txt
```

---

## Exercise 7 — Copy and Rename

Copy the file:

```bash
cp app.txt app_backup.txt
```

Rename the backup:

```bash
mv app_backup.txt backup.txt
```

---

## Exercise 8 — SED

Create a test file:

```bash
cat > data.txt
```

Add several lines containing the word:

```text
linux
```

Then try:

```bash
sed 's/linux/unix/g' data.txt
```

Delete line 2:

```bash
sed '2d' data.txt
```

Print lines 2 through 4:

```bash
sed -n '2,4p' data.txt
```

---

# 💡 DevOps Connection

The commands learned today are foundational for Cloud Engineering and DevOps.

### Why these commands matter

```text
Linux Commands
      │
      ├── File Management
      │
      ├── Log Analysis
      │
      ├── Configuration Management
      │
      ├── Troubleshooting
      │
      ├── Automation
      │
      └── Server Administration
                │
                ↓
             DevOps
                │
                ↓
          Cloud Engineering
                │
                ↓
               AWS
```

For example:

```bash
tail -f application.log
```

can be useful when troubleshooting an application.

And:

```bash
grep -i "error" application.log
```

can help find error messages inside logs.

Commands such as `sed`, `grep`, `awk`, `find`, `cut`, `sort` and `xargs` will later become important for Linux automation and shell scripting.

---

# 📝 Day 01 Summary

Today I learned the fundamentals of Linux command-line usage.

### I can now:

* Navigate the Linux filesystem
* Create and remove directories
* Create, copy, move and delete files
* Read file contents
* Search for text using `grep`
* Count file data using `wc`
* Inspect files using `head` and `tail`
* Edit files using `vi`
* Perform text processing using `sed`

### Next Step

```text
Day 01
  ↓
Linux Basics
  ↓
Day 02
  ↓
Users & Groups
  ↓
Permissions
  ↓
Processes
  ↓
Services
  ↓
Networking
  ↓
SSH
  ↓
Logs
  ↓
Storage
  ↓
Bash
  ↓
Packages
  ↓
Security & Troubleshooting
  ↓
AWS / Cloud Engineering
```

---

## 🚀 Learning Principle

> **Don't just memorize Linux commands. Practice them until you understand what they do, why they are used, and how they help solve real server and cloud problems.**

---


```

**Day 01 Status:** ✅ Completed

