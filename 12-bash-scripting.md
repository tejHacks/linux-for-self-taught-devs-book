# Chapter 12: Bash Scripting

> _"The true power of Linux lies in automation—and bash scripting is the key."_

## Introduction

Every time you type commands manually, you're doing work that a script could do for you. Bash scripts let you:

- Automate repetitive tasks
- Combine multiple commands
- Create custom tools
- Build deployment pipelines

This chapter covers writing effective bash scripts.

## Script Basics

### 12.1 Your First Script

```bash
#!/bin/bash
# This is my first script

echo "Hello, World!"
echo "Today is $(date)"
```

**Save and run**:

```bash
# Save as hello.sh
nano hello.sh

# Make executable
chmod +x hello.sh

# Run
./hello.sh

# Or run with bash directly
bash hello.sh
```

### 12.2 The Shebang

The first line tells the system which interpreter to use:

```bash
#!/bin/bash      # Bash shell
#!/bin/sh        # POSIX shell (more portable)
#!/usr/bin/env python3  # Run with env
```

> **Best practice**: Use `#!/bin/bash` for bash-specific features, `#!/bin/sh` for maximum portability.

### 12.3 Comments

```bash
# Single line comment

# Multi-line comments:
# This script does the following:
# 1. Checks disk usage
# 2. Cleans old logs
# 3. Sends notification
```

## Variables

### 12.4 Defining Variables

```bash
# No spaces around =
name="John"
age=25
echo "Name: $name, Age: $age"

# Read-only variable
readonly PI=3.14159

# Array
colors=(red green blue)
echo ${colors[0]}    # red
echo ${colors[@]}    # all colors
```

### 12.5 Variable Substitution

```bash
name="Linux"

# Basic
echo $name

# With braces
echo ${name}

# Default value if not set
echo ${undefined:-"default"}

# Set default if not set
echo ${undefined:="default"}

# Show error if not set
# echo ${undefined:?"Error: variable not set"}
```

### 12.6 Command Substitution

```bash
# Store command output
current_date=$(date +%Y-%m-%d)
echo $current_date

# Or backticks (older)
uptime=$(uptime)
echo $uptime
```

## Input and Output

### 12.7 Reading User Input

```bash
#!/bin/bash

# Simple input
echo "What is your name?"
read name
echo "Hello, $name!"

# Prompt on same line
read -p "Enter your age: " age
echo "You are $age years old"

# With timeout
read -t 5 -p "Quick! Enter something: " quick
echo "You entered: $quick"
```

### 12.8 Command Line Arguments

```bash
#!/bin/bash
# script.sh arg1 arg2 arg3

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Third argument: $3"
echo "Number of arguments: $#"
echo "All arguments: $@"

# Loop through arguments
for arg in "$@"; do
    echo "Argument: $arg"
done
```

### 12.9 Exit Codes

```bash
#!/bin/bash

# Exit codes: 0 = success, non-zero = error
# Check previous command's exit code
ls /nonexistent
echo "Exit code: $?"

# Exit with code
if [ -f "file.txt" ]; then
    echo "File exists"
    exit 0
else
    echo "File not found"
    exit 1
fi
```

## Conditionals

### 12.10 If Statements

```bash
#!/bin/bash

# Basic if
if [ "$1" -gt 10 ]; then
    echo "Number is greater than 10"
fi

# If-else
if [ -f "file.txt" ]; then
    echo "File exists"
else
    echo "File not found"
fi

# Elif
grade=85

if [ $grade -ge 90 ]; then
    echo "A"
elif [ $grade -ge 80 ]; then
    echo "B"
elif [ $grade -ge 70 ]; then
    echo "C"
else
    echo "F"
fi
```

### 12.11 Test Conditions

| Test  | Meaning             |
| ----- | ------------------- |
| `-f`  | File exists         |
| `-d`  | Directory exists    |
| `-z`  | String is empty     |
| `-n`  | String is not empty |
| `-eq` | Numbers equal       |
| `-ne` | Numbers not equal   |
| `-gt` | Greater than        |
| `-lt` | Less than           |
| `-r`  | File is readable    |
| `-w`  | File is writable    |
| `-x`  | File is executable  |

### 12.12 Case Statements

```bash
#!/bin/bash

read -p "Enter a choice (1-3): " choice

case $choice in
    1)
        echo "You chose option 1"
        ;;
    2)
        echo "You chose option 2"
        ;;
    3)
        echo "You chose option 3"
        ;;
    *)
        echo "Invalid choice"
        ;;
esac
```

## Loops

### 12.13 For Loops

```bash
# Loop through list
for color in red green blue; do
    echo "Color: $color"
done

# Loop through files
for file in *.txt; do
    echo "Processing: $file"
done

# C-style for loop
for ((i=0; i<5; i++)); do
    echo "i = $i"
done

# Loop through arguments
for arg in "$@"; do
    echo $arg
done
```

### 12.14 While Loops

```bash
# Read file line by line
while read line; do
    echo "Line: $line"
done < file.txt

# Counter
count=0
while [ $count -lt 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Infinite loop (break to exit)
while true; do
    read -p "Enter 'quit' to exit: " input
    if [ "$input" = "quit" ]; then
        break
    fi
done
```

### 12.15 Until Loops

```bash
# Until condition is true
count=0
until [ $count -ge 5 ]; do
    echo "Count: $count"
    ((count++))
done
```

## Functions

### 12.16 Defining Functions

```bash
# Function definition
greet() {
    echo "Hello, $1!"
}

# Call function
greet "World"
greet "Linux"

# With return value
add() {
    echo $(($1 + $2))
}

result=$(add 5 3)
echo "5 + 3 = $result"
```

### 12.17 Function Best Practices

```bash
#!/bin/bash

# Use local variables
my_function() {
    local result=0
    result=$(( $1 * 2 ))
    echo $result
}

# Use exit codes for errors
validate_input() {
    if [ -z "$1" ]; then
        echo "Error: No input provided" >&2
        return 1
    fi
    return 0
}

# Check return code
if validate_input "$var"; then
    echo "Valid"
else
    echo "Invalid"
fi
```

## String Manipulation

### 12.18 Common Operations

```bash
# String length
str="Hello World"
echo ${#str}

# Substring
echo ${str:0:5}      # Hello
echo ${str:6:5}      # World

# Remove pattern (front)
str="Hello World"
echo ${str#Hello}    # World
echo ${str#*o}       # World (shortest match)

# Remove pattern (back)
echo ${str%World}    # Hello
echo ${str%o*}       # Hello W (shortest match)

# Replace
echo ${str/Hello/Hi} # Hi World
echo ${str//o/O}    # HellO WOrld

# Case conversion
echo ${str,,}        # hello world
echo ${str^^}        # HELLO WORLD
```

## Arrays in Depth

### 12.19 Array Operations

```bash
# Create array
fruits=(apple banana cherry)

# Add element
fruits+=(date)

# Remove element
unset fruits[1]     # Remove by index

# Array length
echo ${#fruits[@]}

# Loop through array
for fruit in "${fruits[@]}"; do
    echo $fruit
done

# All indices
echo ${!fruits[@]}
```

## Practical Scripts

### 12.20 Backup Script

```bash
#!/bin/bash
# Automated backup script

BACKUP_DIR="/backup"
SOURCE_DIR="/home/user/documents"
DATE=$(date +%Y%m%d_%H%M%S)

# Create backup filename
BACKUP_FILE="$BACKUP_DIR/backup_$DATE.tar.gz"

# Create backup
tar -czf "$BACKUP_FILE" "$SOURCE_DIR"

# Check if successful
if [ $? -eq 0 ]; then
    echo "Backup created: $BACKUP_FILE"

    # Remove backups older than 7 days
    find "$BACKUP_DIR" -name "backup_*.tar.gz" -mtime +7 -delete
    echo "Old backups cleaned"
else
    echo "Backup failed!" >&2
    exit 1
fi
```

### 12.21 System Info Script

```bash
#!/bin/bash
# System information script

echo "=== System Information ==="
echo "Hostname: $(hostname)"
echo "Uptime: $(uptime -p)"
echo "Kernel: $(uname -r)"
echo ""
echo "=== Disk Usage ==="
df -h | grep -v tmpfs | awk '{print $1, $2, $3, $4, $5, $6}'
echo ""
echo "=== Memory Usage ==="
free -h
echo ""
echo "=== Top 5 Processes ==="
ps aux --sort=-%mem | head -6
```

## Debugging

### 12.22 Debug Options

```bash
#!/bin/bash
# Enable debugging

# Show commands before executing
set -x

# Exit on error
set -e

# Exit on undefined variable
set -u

# Combine options
set -eux

# Your code here
echo "Debug mode active"
```

### 12.23 Debugging Tips

```bash
# Print variables for debugging
echo "Debug: var = $var"

# Use bash -x to trace script
bash -x script.sh

# Use shellcheck for static analysis
shellcheck script.sh
```

## Practice Exercises

```bash
# 1. Create a script that takes a name and greets them
# Save as greet.sh, make executable, run with your name

# 2. Create a script that backs up a directory
# Should accept source and destination as arguments

# 3. Create a menu-driven script
# Options: 1) Date 2) Uptime 3) Users 4) Exit

# 4. Create a script that loops through files
# Print filename and line count for each .txt file
```

## Summary

You've learned bash scripting fundamentals:

- **Variables**: Defining and using
- **Input/Output**: read, echo, arguments
- **Conditionals**: if, case
- **Loops**: for, while, until
- **Functions**: Definition and calling
- **String manipulation**: Substrings, replacement
- **Debugging**: set -x, shellcheck

Scripts are the building blocks of automation. Start with simple scripts and gradually build more complex ones.

In the next chapter, we'll set up a complete development environment on Linux.

---

**Previous Chapter**: [Chapter 11: Git on Linux](11-git-on-linux.md)
**Next Chapter**: [Chapter 13: Development Environment](13-dev-environment.md) →
