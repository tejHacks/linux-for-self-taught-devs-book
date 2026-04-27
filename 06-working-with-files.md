# Chapter 6: Working with Files

> _"Files are the currency of computing. Master their manipulation, and you master Linux."_

## Introduction

In Chapter 5, you learned about the filesystem structure. Now let's focus on manipulating files themselves—creating, editing, viewing, copying, and managing them.

This chapter covers essential file operations that you'll use daily as a developer.

## Creating Files

### 6.1 The `touch` Command

The simplest way to create an empty file:

```bash
# Create single file
touch myfile.txt

# Create multiple files
touch file1.txt file2.txt file3.txt

# Update timestamp (don't change content)
touch existing-file.txt

# Create with specific timestamp
touch -d "2024-01-01 00:00" newfile.txt
```

### 6.2 Using Redirection

Create files with content directly from the terminal:

```bash
# Create file with content (overwrites existing)
echo "Hello, World!" > hello.txt

# Append to file (creates if doesn't exist)
echo "Line 1" > file.txt
echo "Line 2" >> file.txt
echo "Line 3" >> file.txt

# View contents
cat file.txt
# Output:
# Line 1
# Line 2
# Line 3
```

### 6.3 Here Documents

Create multi-line files easily:

```bash
# Create file with multiple lines
cat > script.sh << 'EOF'
#!/bin/bash
echo "Starting script..."
date
echo "Done!"
EOF

# Execute
chmod +x script.sh
./script.sh
```

## Viewing Files

### 6.4 `cat` — Concatenate and Display

```bash
# Display entire file
cat filename.txt

# Display with line numbers
cat -n filename.txt

# Show non-printing characters
cat -A filename.txt

# Squeeze blank lines
cat -s filename.txt
```

### 6.5 `less` and `more` — Paginated Viewing

For large files, use a pager:

```bash
# View file page by page
less largefile.log

# Navigation in less:
# Space / PageDown    - Next page
# b / PageUp          - Previous page
# /pattern            - Search forward
# ?pattern            - Search backward
# n                   - Next search result
# N                   - Previous search result
# g                   - Go to beginning
# G                   - Go to end
# q                   - Quit

# View last lines (great for logs)
tail -f application.log

# View first lines (great for headers)
head -n 20 data.csv
```

### 6.6 `head` and `tail` — Partial Views

```bash
# First 10 lines (default)
head filename.txt

# First 20 lines
head -n 20 filename.txt

# Last 10 lines (default)
tail filename.txt

# Last 20 lines
tail -n 20 filename.txt

# Follow a growing file (live logs!)
tail -f /var/log/syslog

# Follow with specific lines buffered
tail -n 100 -f application.log
```

### 6.7 `nl` — Numbered Lines

```bash
# Display with line numbers
nl filename.txt

# Numbered, without blank lines numbered
nl -ba filename.txt
```

## Editing Files

### 6.8 Text Editors Overview

Linux offers several text editors:

| Editor  | Complexity | Best For                 |
| ------- | ---------- | ------------------------ |
| `nano`  | Easy       | Beginners, quick edits   |
| `vim`   | Steep      | Power users, programmers |
| `emacs` | Steep      | Heavy text processing    |
| `code`  | Easy       | VS Code-like experience  |

### 6.9 Using Nano

The most beginner-friendly editor:

```bash
# Open file (creates if doesn't exist)
nano filename.txt

# Nano keyboard shortcuts shown at bottom:
# ^G = Ctrl+G = Get help
# ^O = Ctrl+O = Write out (save)
# ^X = Ctrl+X = Exit
# ^W = Ctrl+W = Where (search)
# ^C = Ctrl+C = Current position
# ^Y = Ctrl+Y = Page up
# ^V = Ctrl+V = Page down
```

**Example workflow**:

```bash
nano myscript.py

# Type content:
#!/usr/bin/env python3
print("Hello, World!")

# Save: Ctrl+O, then Enter
# Exit: Ctrl+X
```

### 6.10 Introduction to Vim

Vim (Vi Improved) is the default editor on almost all Linux systems. It has two modes:

```bash
# Open file
vim filename.txt

# MODES:
# - Normal mode: Commands (default)
# - Insert mode: Type text (press i)
# - Command-line mode: Run commands (press :)

# Basic commands:
i           # Enter insert mode
Esc         # Return to normal mode
:w          # Save (write)
:q          # Quit
:wq         # Save and quit
:q!         # Quit without saving
x           # Delete character
dd          # Delete line
yy          # Yank (copy) line
p           # Paste
u           # Undo
Ctrl+r      # Redo
/string     # Search forward
n           # Next search result
```

> **Tip**: Start with `vimtutor` to learn Vim interactively:
>
> ```bash
> vimtutor
> ```

### 6.11 Using VS Code from Terminal

If you prefer a modern editor:

```bash
# Open file in VS Code
code filename.txt

# Open directory
code .

# Open with specific line
code -g filename.txt:42

# Install code command (if needed)
# Ubuntu/Debian:
sudo apt install code

# Or use Snap:
snap install --classic code
```

## Copying Files

### 6.12 The `cp` Command

```bash
# Copy file to same directory
cp original.txt copy.txt

# Copy to different directory
cp file.txt /home/user/Documents/

# Copy with new name
cp /path/to/source.txt /path/to/destination.txt

# Copy directory recursively
cp -r myfolder/ newfolder/

# Preserve attributes (timestamp, permissions)
cp -p file.txt backup.txt

# Interactive (ask before overwriting)
cp -i file.txt destination.txt

# Verbose (show what was copied)
cp -v file.txt destination.txt
```

**Common options**:

| Flag | Meaning                 |
| ---- | ----------------------- |
| `-r` | Recursive (directories) |
| `-i` | Interactive (prompt)    |
| `-f` | Force (overwrite)       |
| `-p` | Preserve attributes     |
| `-v` | Verbose                 |
| `-a` | Archive (preserve all)  |

## Moving and Renaming

### 6.13 The `mv` Command

```bash
# Move file to directory
mv file.txt /path/to/directory/

# Rename file
mv oldname.txt newname.txt

# Move and rename
mv /old/path/file.txt /new/path/newname.txt

# Interactive (confirm overwrite)
mv -i file.txt destination.txt

# Force (don't prompt)
mv -f file.txt destination.txt

# Don't overwrite newer files
mv -n file.txt destination.txt
```

## Deleting Files

### 6.14 The `rm` Command

```bash
# Delete single file
rm filename.txt

# Delete with confirmation
rm -i filename.txt

# Delete multiple files
rm file1.txt file2.txt file3.txt

# Delete directory and contents
rm -r directory/

# Force delete (no prompts)
rm -rf directory/

# Delete only files older than X days
find /tmp -type f -mtime +7 -delete
```

> ⚠️ **Danger**: `rm -rf /` would delete your entire system! Never run commands you don't understand.

## File Metadata

### 6.15 The `stat` Command

```bash
# Detailed file information
stat filename.txt

# Output:
#   File: filename.txt
#   Size: 1024       Blocks: 8          IO Block: 4096   regular file
# Device: 803h/2048d Inode: 1234567      Links: 1
# Access: (0644/-rw-r--r--)  Uid: ( 1000/   user)   Gid: ( 1000/   user)
# Access: 2024-04-26 10:00:00.000000000 +0000
# Modify: 2024-04-26 10:00:00.000000000 +0000
# Change: 2024-04-26 10:00:00.000000000 +0000
#  Birth: 2024-04-26 10:00:00.000000000 +0000
```

### 6.16 Timestamps Explained

| Timestamp  | Meaning                                        |
| ---------- | ---------------------------------------------- |
| **Access** | Last time file was read                        |
| **Modify** | Last time file content changed                 |
| **Change** | Last time metadata (permissions, name) changed |

```bash
# Touch to update timestamps
touch filename.txt    # Updates all to now

# Update only access time
touch -a filename.txt

# Update only modification time
touch -m filename.txt

# Set specific time
touch -d "2024-01-01 12:00" filename.txt
```

## Searching Inside Files

### 6.17 `grep` — Pattern Matching

```bash
# Search for pattern in file
grep "error" application.log

# Case-insensitive search
grep -i "error" application.log

# Show line numbers
grep -n "error" application.log

# Show only matching part
grep -o "error[0-9]*" application.log

# Recursive search in directory
grep -r "function" /src/

# Invert match (lines WITHOUT pattern)
grep -v "debug" file.txt

# Context (lines before/after)
grep -C 3 "error" application.log
```

### 6.18 `rg` (Ripgrep) — Faster Alternative

```bash
# Install
sudo apt install ripgrep

# Much faster than grep
rg "pattern" .

# Search with file type filter
rg --type py "def " .

# Search with ignore patterns
rg --ignore-case "TODO" .
```

## Practice Exercises

```bash
# 1. Create a file with multiple lines
cat > poem.txt << 'EOF'
Roses are red,
Violets are blue,
Linux is awesome,
And so are you!
EOF

# 2. View the file with line numbers
nl poem.txt

# 3. Copy the file
cp poem.txt poem-backup.txt

# 4. Rename the backup
mv poem-backup.txt poem-saved.txt

# 5. Search for a word
grep "Linux" poem.txt

# 6. View file stats
stat poem.txt

# 7. Clean up
rm poem-saved.txt
```

## Summary

You've learned essential file manipulation:

- **Creating**: `touch`, redirection, `cat`
- **Viewing**: `cat`, `less`, `head`, `tail`
- **Editing**: `nano`, `vim`, `code`
- **Copying**: `cp`
- **Moving**: `mv`
- **Deleting**: `rm`
- **Searching**: `grep`, `rg`

These commands form the daily toolkit of any Linux developer. Practice until they become muscle memory.

In the next chapter, we'll explore essential commands that every developer should know—beyond basic file operations.

---

**Previous Chapter**: [Chapter 5: The Filesystem](05-filesystem.md)
**Next Chapter**: [Chapter 7: Essential Commands](07-essential-commands.md) →
