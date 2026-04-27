# Chapter 5: The Filesystem

> _"Understanding the Linux filesystem is like understanding the anatomy of a city—once you know the layout, navigation becomes intuitive."_

## Introduction

In Chapter 4, you learned basic terminal commands like `ls`, `cd`, and `mkdir`. But we skipped an important topic: the underlying structure that makes it all work. Understanding the Linux filesystem hierarchy will help you:

- Locate configuration files quickly
- Understand where software installs
- Troubleshoot permission issues
- Navigate any Linux system with confidence

## The Filesystem Hierarchy

### 5.1 What Is a Filesystem?

A filesystem is how an operating system organizes and stores files on a disk. Linux uses a hierarchical directory structure—a tree of folders (directories) starting from the root.

```
/
├── bin/        (essential command binaries)
├── boot/       (boot files)
├── dev/        (device files)
├── etc/        (configuration files)
├── home/       (user home directories)
├── lib/        (shared libraries)
├── media/      (removable media)
├── mnt/        (mount points)
├── opt/        (optional software)
├── proc/       (process information)
├── root/       (root user home)
├── run/        (runtime data)
├── sbin/       (system binaries)
├── srv/        (service data)
├── sys/        (system information)
├── tmp/        (temporary files)
├── usr/        (user programs)
└── var/        (variable data)
```

### 5.2 The Root Directory (`/`)

Everything in Linux starts from the root directory. It's denoted by a forward slash `/`, not the backslash you might be used to from Windows.

```bash
# View root directory contents
ls -la /

# Output example:
drwxr-xr-x   1 root root  4096 Apr 26 10:00 .
drwxr-xr-x   1 root root  4096 Apr 26 10:00 ..
drwxr-xr-x  17 root root  4096 Apr 26 10:00 bin
drwxr-xr-x   5 root root  4096 Apr 26 10:00 boot
drwxr-xr-x  1 root root  4096 Apr 26 10:00 dev
drwxr-xr-x  95 root root  4096 Apr 26 10:00 etc
drwxr-xr-x   3 root root  4096 Apr 26 10:00 home
drwxr-xr-x   1 root root  4096 Apr 26 10:00 lib
drwxr-xr-x   1 root root root  4096 Apr 26 10:00 media
drwxr-xr-x   1 root root root  4096 Apr 26 10:00 mnt
drwxr-xr-x   3 root root  4096 Apr 26 10:00 opt
drwxr-xr-x   1 root root  4096 Apr 26 10:00 proc
drwxr-xr-x   1 root root root  4096 Apr 26 10:00 root
drwxr-xr-x  35 root root  4096 Apr 26 10:00 run
drwxr-xr-x   2 root root  4096 Apr 26 10:00 sbin
drwxr-xr-x   4 root root  4096 Apr 26 10:00 srv
drwxr-xr-x   1 root root root  4096 Apr 26 10:00 sys
drwxr-xr-x   4 root root  4096 Apr 26 10:00 tmp
drwxr-xr-x  11 root root  4096 Apr 26 10:00 usr
drwxr-xr-x  24 root root  4096 Apr 26 10:00 var
```

## Key Directories Explained

### 5.3 `/home` — Your Personal Space

This is where regular user accounts live. Each user gets a subdirectory:

```bash
# Your home directory
echo $HOME
# Output: /home/username

# List contents
ls -la ~/

# Typical structure:
# ~/Documents/
# ~/Downloads/
# ~/Pictures/
# ~/Music/
# ~/Videos/
# ~/.config/        (application settings)
# ~/.local/         (local application data)
```

> **Note**: The `~` is a shortcut for your home directory.

### 5.4 `/etc` — Configuration Central

The `etc` directory contains system-wide configuration files. This is where you'll spend lots of time tweaking your system.

```bash
# Important config files
ls /etc/

# Network configuration
cat /etc/hostname        # Your computer's name
cat /etc/hosts          # IP address mappings

# User database
cat /etc/passwd         # User accounts (viewable by all)
cat /etc/shadow         # Encrypted passwords (root only)

# Package sources
ls /etc/apt/sources.list.d/    # Additional apt sources
cat /etc/apt/sources.list     # Main apt sources
```

**Common `/etc` files you'll encounter**:

| File                   | Purpose                  |
| ---------------------- | ------------------------ |
| `/etc/passwd`          | User account information |
| `/etc/shadow`          | Encrypted passwords      |
| `/etc/group`           | Group memberships        |
| `/etc/fstab`           | Filesystem mount points  |
| `/etc/hosts`           | Static hostname lookups  |
| `/etc/resolv.conf`     | DNS servers              |
| `/etc/ssh/sshd_config` | SSH server configuration |

### 5.5 `/var` — Variable Data

Files that change frequently—logs, caches, databases—live here:

```bash
# Important /var subdirectories
ls /var/

# System logs (troubleshooting gold)
ls /var/log/
# - /var/log/syslog      # System messages
# - /var/log/auth.log   # Authentication attempts
# - /var/log/kern.log   # Kernel messages

# Package cache
ls /var/cache/

# Application data
ls /var/lib/           # Application state
```

**Practical examples**:

```bash
# View recent system errors
tail -f /var/log/syslog

# View failed login attempts
sudo tail /var/log/auth.log

# Check disk usage by log files
du -sh /var/log/*
```

### 5.6 `/usr` — User Programs

The largest directory tree—contains most installed software:

```bash
# /usr structure
ls /usr/
# bin/    - User commands
# sbin/   - System commands
# lib/    - Libraries
# include - Header files (for C/C++ development)
# share/  - Architecture-independent data
# local/  - Locally installed software

# Where programs install
which python3          # Find where python is installed
# Output: /usr/bin/python3

# Program documentation
ls /usr/share/doc/
```

### 5.7 `/tmp` — Temporary Files

A world-writable directory for temporary files. Contents are often deleted on reboot:

```bash
# Check tmp
ls -la /tmp/

# Create temporary file
touch /tmp/my-temp-file.txt

# Many programs use this for working files
# Note: Anyone can read/write here!
```

### 5.8 `/proc` and `/sys` — Virtual Filesystems

These are special directories that don't exist on disk—they're interfaces to kernel data:

```bash
# Process information
ls /proc/              # One folder per running process

# Your current process
echo $$                # Show current shell PID
cat /proc/self/status  # Information about this process

# System information
cat /proc/cpuinfo      # CPU details
cat /proc/meminfo      # Memory details
cat /proc/uptime       # System uptime

# Kernel parameters (tunable!)
cat /proc/sys/net/ipv4/ip_forward    # Is IP forwarding on?
```

> **Warning**: `/proc` and `/sys` are virtual—files here are generated on-the-fly by the kernel.

## Path Types

### 5.9 Absolute vs Relative Paths

**Absolute paths** start from root:

```bash
# These are absolute paths
/home/user/Documents/project.txt
/etc/nginx/nginx.conf
/var/log/syslog
```

**Relative paths** start from your current directory:

```bash
# If you're in /home/user/
Documents/project.txt        # Same as /home/user/Documents/project.txt

# Go up one level
../Documents/project.txt    # From /home/user/Documents/

# Go up two levels
../../etc/passwd
```

### 5.10 Special Directory References

| Symbol | Meaning            | Example         |
| ------ | ------------------ | --------------- |
| `.`    | Current directory  | `./script.sh`   |
| `..`   | Parent directory   | `../config.txt` |
| `~`    | Home directory     | `~/Downloads`   |
| `-`    | Previous directory | `cd -`          |

## Disk Usage

### 5.11 Checking Space

```bash
# Overall disk usage
df -h

# Output:
# Filesystem      Size  Used Avail Use% Mounted on
# /dev/sda1      100G   45G   55G  45% /
# tmpfs          2.0G     0  2.0G   0% /dev/shm
# /dev/sda2     500G  200G  300G  40% /home

# Directory sizes
du -sh /var/log

# Top 10 largest directories
du -sh /var/* | sort -rh | head -10
```

### 5.12 Finding Files

```bash
# Find a file by name
find /home -name "myfile.txt"

# Find by pattern
find /etc -name "*.conf"

# Find by size
find /var -size +100M

# Find recently modified files
find /home -mtime -7        # Modified in last 7 days
```

## Symbolic Links

### 5.13 Understanding Links

Linux has two types of links:

**Hard links** — multiple names for the same file:

```bash
# Create hard link
ln original.txt hardlink.txt

# Both point to same data on disk
# Cannot cross filesystem boundaries
# Cannot link to directories
```

**Symbolic (soft) links** — like Windows shortcuts:

```bash
# Create symbolic link
ln -s original.txt symlink.txt

# View link
ls -la symlink.txt
# lrwxrwxrwx  1 user user  12 Apr 26 10:00 symlink.txt -> original.txt

# Remove original, link breaks!
rm original.txt
cat symlink.txt    # Error: No such file or directory
```

## Practice Exercises

```bash
# 1. Explore the root filesystem
ls -la /

# 2. Check your home directory structure
tree -L 2 ~/

# 3. Find all .conf files in /etc
find /etc -name "*.conf" | head -20

# 4. Check disk usage
df -h

# 5. Find the largest directories in /var
du -sh /var/* 2>/dev/null | sort -rh | head -5

# 6. Create a symbolic link
ln -s /var/log syslog-link
ls -la syslog-link
```

## Summary

You've learned the Linux filesystem hierarchy:

- **Root (`/`)**: The foundation of everything
- **`/home`**: Your personal files
- **`/etc`**: System configuration
- **`/var`**: Logs and variable data
- **`/usr`**: Installed software
- **`/proc` and `/sys`**: Kernel interfaces

Understanding this structure will make you more effective at troubleshooting, configuration, and general system administration.

In the next chapter, we'll dive deeper into working with files—creating, editing, copying, and managing them effectively.

---

**Previous Chapter**: [Chapter 4: Terminal Basics](04-terminal-basics.md)
**Next Chapter**: [Chapter 6: Working with Files](06-working-with-files.md) →
