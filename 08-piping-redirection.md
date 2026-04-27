# Chapter 8: Piping and Redirection

> _"In Linux, small tools combined well outperform large tools used in isolation. Piping is the glue."_

## Introduction

One of Linux's most powerful concepts is the ability to chain commands together. Instead of doing things one at a time, you can connect programs so the output of one becomes the input of another.

This chapter covers redirection (working with files) and piping (working with streams).

## Standard Streams

### 8.1 Understanding Streams

Every Linux program has three standard streams:

| Stream     | Number | Description               | Default  |
| ---------- | ------ | ------------------------- | -------- |
| **stdin**  | 0      | Standard input (keyboard) | Terminal |
| **stdout** | 1      | Standard output (display) | Terminal |
| **stderr** | 2      | Error messages (display)  | Terminal |

```bash
# Visual representation:
# stdin ← keyboard ← user
# stdout → terminal → user
# stderr → terminal → user
```

## Redirection

### 8.2 Output Redirection (`>` and `>>`)

**Overwrite (`>`)**:

```bash
# Redirect output to file (overwrites)
echo "Hello" > output.txt

# This replaces the entire file content
cat output.txt
# Output: Hello
```

**Append (`>>`)**:

```bash
# Append output to file
echo "Line 1" > file.txt
echo "Line 2" >> file.txt
echo "Line 3" >> file.txt

cat file.txt
# Output:
# Line 1
# Line 2
# Line 3
```

### 8.3 Input Redirection (`<`)

```bash
# Read from file instead of keyboard
# These are equivalent:
# way 1: cat < file.txt
# way 2: cat file.txt

# More complex example:
sort < unsorted.txt > sorted.txt
```

### 8.4 Error Redirection (`2>`)

```bash
# Redirect errors to file
ls /nonexistent 2> errors.txt

# Redirect both output and errors
command > all_output.txt 2>&1

# Modern syntax (bash 4+)
command &> all_output.txt

# Append both
command >> all_output.txt 2>> error_log.txt
```

### 8.5 The `/dev/null` Special File

Discard output you don't want:

```bash
# Suppress normal output, show errors
command > /dev/null

# Suppress error messages
command 2> /dev/null

# Suppress everything
command > /dev/null 2>&1
# or
command &> /dev/null

# Useful example: suppress apt progress
sudo apt update > /dev/null 2>&1
```

## Piping

### 8.6 The Pipe (`|`)

The pipe takes the output of one command and feeds it as input to another:

```bash
# Basic pipe
echo "hello world" | tr '[:lower:]' '[:upper:]'
# Output: HELLO WORLD

# Count lines in output
ls -la | wc -l

# Search in output
ps aux | grep python

# Sort output
ls -l | sort
```

### 8.7 Practical Piping Examples

**Example 1: Find largest files**

```bash
# Find top 5 largest files in current directory
du -ah . | sort -rh | head -5

# Explanation:
# du -ah .           -> List all files with sizes
# sort -rh           -> Sort by size (human-readable, reverse)
# head -5           -> Take first 5 lines
```

**Example 2: Process list**

```bash
# Find all Python processes
ps aux | grep python | grep -v grep

# Explanation:
# ps aux              -> List all processes
# grep python        -> Filter for "python"
# grep -v grep       -> Remove the grep command itself
```

**Example 3: Log analysis**

```bash
# Find errors in log, show last 10
tail -f /var/log/syslog | grep -i error | tail -10

# Explanation:
# tail -f            -> Follow log file
# grep -i error     -> Filter for errors (case-insensitive)
# tail -10          -> Show last 10 matches
```

**Example 4: Word frequency**

```bash
# Count word frequency in file
cat file.txt | tr '[:space:]' '\n' | sort | uniq -c | sort -rn

# Explanation:
# cat file.txt       -> Read file
# tr '[:space:]'    -> Split on whitespace
# '\n'              -> New lines (one word per line)
# sort              -> Alphabetical sort
# uniq -c           -> Count occurrences
# sort -rn          -> Sort by count (reverse, numeric)
```

### 8.8 `tee` — Read from Stdin, Write to Stdout and Files

```bash
# Save output to file AND see it on screen
echo "Hello" | tee output.txt
# Output: Hello
# File contains: Hello

# Append with tee
echo "World" | tee -a output.txt

# Combined with pipes
cat input.txt | tee output.txt | wc -l
```

## Advanced Redirection

### 8.9 Here Strings (`<<<`)

```bash
# Feed string to command
wc -w <<< "one two three"
# Output: 3

# Useful with read
read -r name <<< "John"
echo $name
# Output: John
```

### 8.10 Here Documents (`<<`)

We saw this in Chapter 6 for creating files. It's also useful for interactive commands:

```bash
# Feed multiple lines to command
cat << 'EOF' | bash
echo "Starting setup..."
echo "Configuration complete"
echo "Done"
EOF

# With variable substitution (note: no quotes around EOF)
name="Server"
cat << EOF > config.txt
server_name=$name
port=8080
EOF
```

### 8.11 Command Substitution (`$()` and backticks)

```bash
# Store command output in variable
current_date=$(date +%Y-%m-%d)
echo $current_date
# Output: 2024-04-26

# Use in commands
echo "Today is $(date +%A)"

# Backticks (older syntax)
echo "Uptime: $(uptime -p)"

# Nested commands
ls -la $(which python)
```

### 8.12 Arithmetic Substitution (`$(( ))`)

```bash
# Basic math
echo $(( 5 + 3 ))
# Output: 8

# Variables
x=10
y=5
echo $(( x + y ))
# Output: 15

# Multiplication and division
echo $(( 10 * 5 ))
# Output: 50
echo $(( 10 / 3 ))
# Output: 3 (integer division)

# Increment
x=5
echo $(( x++ ))
# Output: 5 (returns old value)
echo $x
# Output: 6
```

## Filters and Stream Editors

### 8.13 `sed` — Stream Editor

**Find and replace**:

```bash
# Replace first occurrence in each line
sed 's/old/new/' file.txt

# Replace all occurrences
sed 's/old/new/g' file.txt

# Replace on specific line
sed '3s/old/new/' file.txt

# In-place edit
sed -i 's/old/new/g' file.txt

# Examples:
echo "hello world" | sed 's/world/Linux/'
# Output: hello Linux

# Multiple replacements
echo "cat dog cat" | sed -e 's/cat/bird/g' -e 's/dog/cat/g'
# Output: bird cat bird
```

**Other sed operations**:

```bash
# Print specific lines
sed -n '5p' file.txt        # Print line 5
sed -n '5,10p' file.txt   # Print lines 5-10

# Delete lines
sed '5d' file.txt         # Delete line 5
sed '/pattern/d' file.txt # Delete matching lines

# Print lines with pattern
sed -n '/pattern/p' file.txt
```

### 8.14 `awk` — Pattern Scanning and Processing

**Basic usage**:

```bash
# Print first column
echo "one two three" | awk '{print $1}'
# Output: one

# Print multiple columns
echo "John 25 Developer" | awk '{print $1, $3}'
# Output: John Developer

# Field separator
echo "1,2,3" | awk -F',' '{print $2}'
# Output: 2

# Using in files
awk '{print $1, $3}' data.txt
```

**Built-in variables**:

| Variable      | Description            |
| ------------- | ---------------------- |
| `$0`          | Entire line            |
| `$1`, `$2`... | Field 1, 2...          |
| `NF`          | Number of fields       |
| `NR`          | Current record number  |
| `FS`          | Field separator        |
| `OFS`         | Output field separator |

**Practical examples**:

```bash
# Print lines longer than 80 chars
awk 'length > 80' file.txt

# Sum column
awk '{sum+=$1} END {print sum}' numbers.txt

# Print with formatting
awk '{printf "%-10s %s\n", $1, $2}' data.txt

# Conditional processing
awk '$3 > 50 {print $1, $3}' data.txt
```

### 8.15 `cut` — Select Columns

```bash
# Cut by field delimiter
echo "one:two:three" | cut -d':' -f2
# Output: two

# Cut by character position
echo "Hello" | cut -c1-3
# Output: Hel

# Multiple fields
echo "a,b,c,d" | cut -d',' -f1,3
# Output: a,c
```

### 8.16 `tr` — Character Translation

```bash
# Convert to uppercase
echo "hello" | tr '[:lower:]' '[:upper:]'
# Output: HELLO

# Delete characters
echo "hello123" | tr -d '0-9'
# Output: hello

# Squeeze repeated characters
echo "hellooo" | tr -s 'o'
# Output: helllo

# Translate set
echo "yes" | tr 'ys' 'YS'
# Output: YeS
```

## Combining Everything

### 8.17 Complex Pipeline Example

Let's build a pipeline step by step. Goal: Find the top 5 most frequent errors in a log file.

```bash
# Step 1: Get log content
cat /var/log/syslog

# Step 2: Filter for errors
cat /var/log/syslog | grep -i error

# Step 3: Extract just the error message
cat /var/log/syslog | grep -i error | awk '{print $5}'

# Step 4: Count occurrences
cat /var/log/syslog | grep -i error | awk '{print $5}' | sort | uniq -c

# Step 5: Sort by frequency
cat /var/log/syslog | grep -i error | awk '{print $5}' | sort | uniq -c | sort -rn

# Step 6: Take top 5
cat /var/log/syslog | grep -i error | awk '{print $5}' | sort | uniq -c | sort -rn | head -5
```

**Refined version**:

```bash
grep -i error /var/log/syslog | awk '{print $5}' | sort | uniq -c | sort -rn | head -5
```

### 8.18 One-Liner Library

Here are useful one-liners to keep in your toolkit:

```bash
# Kill all processes of a type
ps aux | grep pattern | awk '{print $2}' | xargs kill

# Find and replace in multiple files
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# Monitor command output
watch -n 1 'ps aux | grep python'

# Download and extract in one command
curl -sL url | tar -xz -C /destination/

# Quick HTTP server in current directory
python3 -m http.server 8000

# Find duplicate files
find . -type f -exec md5sum {} \; | sort | uniq -D -w32
```

## Practice Exercises

```bash
# 1. Redirect output to file
echo "My first redirection" > myfile.txt

# 2. Append more content
echo "Another line" >> myfile.txt

# 3. Use pipe to count lines
ls -la ~ | wc -l

# 4. Find processes, filter, count
ps aux | grep bash | wc -l

# 5. Create a pipeline
cat /etc/passwd | grep /bin/bash | cut -d: -f1 | sort

# 6. Use tee
echo "Saving and displaying" | tee output.txt

# 7. Use command substitution
echo "Current user: $(whoami)"
echo "Today: $(date +%Y-%m-%d)"
```

## Summary

You've learned to connect commands into powerful pipelines:

- **Redirection**: `>`, `>>`, `<`, `2>`
- **Piping**: `|` to chain commands
- **tee**: Save and display output
- **sed**: Stream editor for find/replace
- **awk**: Pattern scanning and processing
- **tr**: Character translation
- **cut**: Column extraction

These tools let you build complex data processing workflows from simple components. This is the essence of the Unix philosophy: small tools, combined well.

In the next chapter, we'll explore package management—how to install, update, and remove software on Linux.

---

**Previous Chapter**: [Chapter 7: Essential Commands](07-essential-commands.md)
**Next Chapter**: [Chapter 9: Package Management](09-package-management.md) →
