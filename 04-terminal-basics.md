# Chapter 4: Terminal Basics

> _"The terminal isn't just a throwback to the past—it's a time machine for your productivity."_

## Introduction

If there's one skill that defines a Linux developer, it's comfort with the command line. The terminal (also called shell, console, or command prompt) is where you'll spend most of your time as a Linux user.

This chapter introduces the terminal, basic commands, and builds your foundation for command-line work.

## What Is the Terminal?

The terminal is a text-based interface to your computer. Instead of clicking icons, you type commands. Instead of menus, you get precise control.

**Why bother?**:

- Speed: Type faster than you click
- Automation: Script repetitive tasks
- Precision: Exact commands, exact results
- Remote work: SSH into servers
- Understanding: Learn how systems actually work

## Accessing the Terminal

### On Linux

```bash
# Keyboard shortcut
Ctrl + Alt + T

# Or search for "terminal" in your applications
```

### On macOS

```bash
# Keyboard shortcut
Cmd + Space, then type "Terminal"

# Or find Terminal in /Applications/Utilities/
```

### On Windows (WSL)

```bash
# Open WSL from Start menu
# Or use Windows Terminal (recommended)
```

## Your First Commands

### 4.1 Exploring: `ls` (List)

```bash
# List files in current directory
ls

# List with details (long format)
ls -l

# List including hidden files
ls -la

# List with human-readable sizes
ls -lh

# List with colors (if enabled)
ls --color=auto
```

**Output example**:

```
drwxr-xr-x  5 user user  4096 Apr 26 10:30 .
drwxr-xr-x  5 user user  4096 Apr 26 10:30 ..
drwxr-xr-x  3 user user  4096 Apr 26 09:15 Documents
drwxr-xr-x  2 user user  4096 Apr 26 08:20 Downloads
-rw-r--r--  1 user user   220 Apr 26 07:30 .bashrc
```

**Understanding the output**:

- `d` = directory (folder)
- `-` = regular file
- `l` = symbolic link
- `rwx` = read/write/execute permissions

### 4.2 Navigation: `cd` (Change Directory)

```bash
# Go to home directory
cd ~

# Go to specific directory
cd /home/user/Documents

# Go to parent directory
cd ..

# Go to previous directory
cd -

# Go to current directory (stay here)
cd .
```

**Path types**:

- **Absolute**: `/home/user/Documents` (starts from root)
- **Relative**: `Documents` (starts from current location)

### 4.3 Creating: `mkdir` (Make Directory)

```bash
# Create single directory
mkdir myproject

# Create nested directories
mkdir -p project/src/components

# Create multiple directories
mkdir dir1 dir2 dir3
```

### 4.4 Viewing: `pwd` (Print Working Directory)

```bash
# Show current directory
pwd

# Output: /home/user/Documents
```

### 4.5 Reading Files: `cat` and `less`

```bash
# Display entire file
cat filename.txt

# Display file with pagination
less filename.txt

# Display first lines
head -n 20 filename.txt

# Display last lines
tail -n 20 filename.txt

# Follow a growing file (great for logs)
tail -f application.log
```

**Navigation in `less`**:

- `Space` = next page
- `b` = previous page
- `/pattern` = search forward
- `n` = next search result
- `q` = quit

## Understanding the Prompt

Your terminal prompt might look like this:

```
user@hostname:~$ _
```

**Parts**:

- `user` = your username
- `@` = separator
- `hostname` = computer name
- `:` = separator
- `~` = current directory (home = `~`)
- `$` = regular user (# would be root)

**Examples**:

```
user@dev:~$ cd /var/log
user@dev:/var/log$ ls
user@dev:/var/log$ sudo su
root@dev:/var/log# _
```

## Working with Files

### 4.6 Copying: `cp` (Copy)

```bash
# Copy file
cp source.txt destination.txt

# Copy to directory
cp file.txt /path/to/directory/

# Copy recursively
cp -r source_folder/ destination_folder/

# Preserve attributes
cp -p file.txt backup.txt
```

### 4.7 Moving/Renaming: `mv` (Move)

```bash
# Move file
mv file.txt /path/to/destination/

# Rename file
mv oldname.txt newname.txt

# Move and rename
mv /old/path/file.txt /new/path/newname.txt
```

### 4.8 Removing: `rm` (Remove)

```bash
# Remove file
rm unwanted.txt

# Remove directory
rm -r directory/

# Remove without prompt
rm -f force.txt

# Remove empty directory
rmdir empty_folder/
```

> ⚠️ **Warning**: Linux has no trash bin in terminal. `rm` permanently deletes. Use `-i` flag for safety: `rm -i file.txt`

## Getting Help

### 4.9 The `man` Command

```bash
# Read manual for a command
man ls

# Search manual pages
man -k "search term"
```

**Navigation**:

- `Space` = next page
- `Enter` = next line
- `/` = search
- `q` = quit

### 4.10 The `--help` Flag

```bash
# Quick help for most commands
ls --help

# Or
command -h
```

### 4.11 The `info` Command

```bash
# Detailed info pages
info command
```

### 4.12 Whatis and Apropos

```bash
# One-line description
whatis ls

# Search by keyword
apropos "network"
```

## Tab Completion and History

### Tab Completion

Type partial command, press `Tab`:

```bash
# Type this:
cd Doc<Tab>

# System completes to:
cd Documents/
```

**Double Tab**: If multiple options exist, press Tab twice to see them.

### Command History

```bash
# Previous command
Up Arrow

# Next command
Down Arrow

# Search history
Ctrl + R

# Run last command
!!

# Run last command starting with "git"
!git
```

## Wildcards and Patterns

### Common Wildcards

```bash
# * = any characters
ls *.txt          # All .txt files

# ? = single character
ls file?.txt      # file1.txt, file2.txt, etc.

# [] = character class
ls [abc]*.txt     # a*.txt, b*.txt, c*.txt
```

### Examples

```bash
# All Python files
ls *.py

# All files starting with "report"
ls report*

# Files numbered 1-5
ls file[1-5].txt
```

## Environment Variables

### Viewing Variables

```bash
# List all variables
env

# View specific variable
echo $HOME
echo $PATH
echo $USER
```

### Common Variables

| Variable | Meaning                | Example                   |
| -------- | ---------------------- | ------------------------- |
| `HOME`   | Your home directory    | `/home/user`              |
| `USER`   | Your username          | `john`                    |
| `PATH`   | Where to find commands | `/usr/local/bin:/usr/bin` |
| `PWD`    | Current directory      | `/home/user`              |
| `SHELL`  | Your default shell     | `/bin/bash`               |

## Practice Exercises

Try these commands to build muscle memory:

```bash
# 1. Navigate your home directory
cd ~
pwd
ls

# 2. Create a practice directory
mkdir -p practice/level1/level2
cd practice
ls -R

# 3. Create some files
touch file1.txt file2.txt file3.txt
ls

# 4. Copy and move files
cp file1.txt file1-copy.txt
mv file2.txt renamed.txt
ls

# 5. Clean up
rm file1-copy.txt
rm renamed.txt
cd ..
rm -r practice
```

## Summary

You've learned the fundamentals of the Linux terminal:

- **Navigation**: `cd`, `pwd`, `ls`
- **File operations**: `cp`, `mv`, `rm`, `mkdir`
- **Reading files**: `cat`, `less`, `head`, `tail`
- **Getting help**: `man`, `--help`
- **Productivity**: Tab completion, history

These commands form the foundation for everything else in Linux. Practice until they become second nature.

In the next chapter, we'll explore the Linux filesystem—understanding how files are organized and how to navigate effectively.

---

**Previous Chapter**: [Chapter 3: Choosing Your Setup](03-choosing-setup.md)
**Next Chapter**: [Chapter 5: The Filesystem](05-filesystem.md) →
