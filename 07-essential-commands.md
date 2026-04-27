# Chapter 7: Essential Commands

> _"The commands in this chapter won't all be used every day, but when you need them, they'll save you hours."_

## Introduction

Chapters 4 and 6 covered fundamental commands (`ls`, `cd`, `cp`, `mv`, `rm`). This chapter expands your toolkit with commands you'll need for real-world development work.

## System Information

### 7.1 `uname` — System Information

```bash
# Basic system info
uname
# Output: Linux

# All information
uname -a
# Output: Linux hostname 5.15.0-91-generic #101-Ubuntu SMP ...

# Kernel release
uname -r
# Output: 5.15.0-91-generic

# Kernel version
uname -v
# Output: #101-Ubuntu SMP PREEMPT Tue Nov ...

# Machine type
uname -m
# Output: x86_64

# Operating system
uname -o
# Output: GNU/Linux
```

### 7.2 `hostname` — Computer Name

```bash
# Show hostname
hostname

# Show domain
hostname -d

# Show IP address
hostname -i

# Temporarily change hostname (requires root)
sudo hostname newname
```

### 7.3 `uptime` — System Uptime

```bash
# Simple uptime
uptime

# Output:  10:30:00 up 42 days,  1:22,  2 users,  load average: 0.52, 0.58, 0.59

# Detailed with pretty format
uptime -p
# Output: up 6 weeks, 1 day, 22 minutes

# Since epoch
uptime -s
# Output: 2024-03-15 08:08:00
```

### 7.4 `date` — Date and Time

```bash
# Current date and time
date

# Format output
date "+%Y-%m-%d"           # 2024-04-26
date "+%H:%M:%S"           # 10:30:45
date "+%A %B %d"           # Friday April 26

# Unix timestamp
date +%s
# Output: 1714132245

# Convert timestamp to date
date -d @1714132245

# Set system time (requires root)
sudo date -s "2024-04-26 10:30:00"
```

## Process Management

### 7.5 `ps` — Process Snapshot

```bash
# Your processes
ps

# All processes
ps aux

# Format output
ps -eo pid,ppid,cmd,%mem,%cpu

# Find specific process
ps aux | grep python

# Tree view
ps -efH

# Explanation of common flags:
# a     - All users' processes
# u     - User-oriented format
# x     - Processes without terminal
# e     - Show environment
# f     - Forest view (tree)
```

### 7.6 `top` — Real-time Process Monitor

```bash
# Interactive process viewer
top

# Common keys in top:
# q             - Quit
# P             - Sort by CPU
# M             - Sort by Memory
# k             - Kill process
# r             - Renice process
# 1             - Toggle CPU cores
# f             - Field manager

# Useful shortcuts:
top -u username    # Only user's processes
top -p 1234        # Monitor specific PID
```

### 7.7 `htop` — Enhanced Process Viewer (Recommended)

```bash
# Install
sudo apt install htop

# Run
htop

# Features:
# - Color output
# - Scroll horizontally
# - Tree view (F5)
# - Kill processes easily (F9)
# - Search processes (F3)
```

### 7.8 `kill` — Terminate Processes

```bash
# Kill by PID
kill 12345

# Force kill
kill -9 12345

# Graceful termination (SIGTERM)
kill -15 12345

# Kill all processes by name
pkill -f "python script.py"

# Kill all processes for user
pkill -u username

# Common signals:
# 1   - SIGHUP   - Hangup (reload config)
# 2   - SIGINT   - Interrupt (Ctrl+C)
# 9   - SIGKILL  - Force kill (cannot be caught)
# 15  - SIGTERM  - Graceful termination
# 19  - SIGSTOP  - Stop (cannot be caught)
```

### 7.9 `killall` — Kill by Name

```bash
# Kill all processes with name
killall firefox

# Force kill
killall -9 chrome

# Case-insensitive
killall -i chrome
```

## Resource Monitoring

### 7.10 `free` — Memory Usage

```bash
# Basic memory info
free

# Human-readable
free -h

# In megabytes
free -m

# Show total
free -t

# Continuous monitoring
free -s 5 -c 10    # Update every 5 seconds, 10 times
```

**Example output**:

```
              total        used        free      shared  buff/cache   available
Mem:           16Gi       4.5Gi       8.5Gi       200Mi       3.0Gi        11Gi
Swap:         2.0Gi          0B       2.0Gi
```

### 7.11 `df` — Disk Filesystem Usage

```bash
# All filesystems
df

# Human-readable
df -h

# Show filesystem type
df -T

# Exclude certain types
df -h -x tmpfs -x devtmpfs

# Inode information
df -i
```

### 7.12 `du` — Disk Usage

```bash
# Current directory size
du

# Human-readable
du -h

# Total only
du -sh

# All subdirectories
du -h --max-depth=1

# Sort by size
du -sh * | sort -h

# Exclude certain directories
du -h --exclude='*.log'
```

### 7.13 `top` for CPU — `mpstat` and `iostat`

```bash
# Install sysstat for these tools
sudo apt install sysstat

# CPU statistics
mpstat

# Per-CPU statistics
mpstat -P ALL

# I/O statistics
iostat

# Detailed I/O
iostat -x
```

## Network Commands

### 7.14 `ip` — Network Configuration (Modern)

```bash
# Show IP addresses
ip addr

# Show only IPv4 addresses
ip -4 addr

# Show routes
ip route

# Show neighbors (ARP table)
ip neigh

# Add IP address (requires root)
sudo ip addr add 192.168.1.100/24 dev eth0

# Bring interface up/down
sudo ip link set eth0 up
sudo ip link set eth0 down
```

### 7.15 `ping` — Test Connectivity

```bash
# Ping host
ping google.com

# Ping with count
ping -c 4 google.com

# Interval (faster)
ping -i 0.2 google.com

# Flood ping (requires root)
sudo ping -f google.com
```

### 7.16 `curl` — HTTP Client

```bash
# GET request
curl https://api.github.com

# Save to file
curl -O https://example.com/file.zip

# Custom headers
curl -H "Authorization: Bearer TOKEN" https://api.example.com

# POST data
curl -X POST -d "data=value" https://api.example.com

# Follow redirects
curl -L https://example.com

# Verbose (show request/response)
curl -v https://example.com
```

### 7.17 `wget` — Download Files

```bash
# Download file
wget https://example.com/file.zip

# Download with filename
wget -O output.zip https://example.com/file.zip

# Continue download
wget -c https://example.com/large-file.zip

# Background download
wget -b https://example.com/file.zip

# Multiple URLs from file
wget -i urls.txt
```

### 7.18 `ssh` — Secure Shell

```bash
# Connect to server
ssh username@server.example.com

# Connect on different port
ssh -p 2222 username@server.example.com

# Connect with key
ssh -i ~/.ssh/my_key username@server.example.com

# Copy file (SCP)
scp file.txt username@server:/path/

# Copy from server
scp username@server:/path/file.txt ./

# Secure copy (SFTP)
sftp username@server.example.com
```

## Searching and Finding

### 7.19 `find` — Search for Files

```bash
# Find by name
find /home -name "*.txt"

# Case-insensitive
find /home -iname "*.TXT"

# Find by type (f=file, d=directory)
find /var -type f -name "*.log"

# Find by size
find /tmp -size +100M
find /tmp -size -1M

# Find by time
find /home -mtime -7        # Modified in last 7 days
find /home -atime +30       # Accessed more than 30 days ago

# Find by permissions
find /usr -perm 755

# Execute command on results
find /home -name "*.tmp" -delete
find /home -name "*.log" -exec tail -n 10 {} \;
```

### 7.20 `locate` — Fast File Search

```bash
# Install (requires updated database)
sudo apt install mlocate

# Update database (run as root)
sudo updatedb

# Find file
locate myfile.txt

# Search in database (fast!)
locate -i "readme"
```

### 7.21 `which` and `whereis` — Find Commands

```bash
# Find command location
which python
# Output: /usr/bin/python

# Find all instances
which -a python

# Find binary, source, and man page
whereis python
# Output: python: /usr/bin/python /usr/share/man/man1/python.1.gz
```

## Archive and Compression

### 7.22 `tar` — Tape Archive

```bash
# Create tar archive
tar -cvf archive.tar directory/

# Extract tar archive
tar -xvf archive.tar

# Create tar.gz
tar -czvf archive.tar.gz directory/

# Extract tar.gz
tar -xzvf archive.tar.gz

# List contents without extracting
tar -tzvf archive.tar.gz
```

### 7.23 `zip` and `unzip`

```bash
# Create zip archive
zip -r archive.zip directory/

# Extract zip
unzip archive.zip

# Extract to specific directory
unzip archive.zip -d /destination/

# List contents
unzip -l archive.zip
```

### 7.24 `gzip`, `bzip2`, `xz` — Compression

```bash
# Compress file
gzip file.txt          # Creates file.txt.gz
bzip2 file.txt         # Creates file.txt.bz2
xz file.txt            # Creates file.txt.xz

# Decompress
gunzip file.txt.gz
bunzip2 file.txt.bz2
unxz file.txt.xz

# Compress and keep original
gzip -k file.txt

# Decompress to stdout
gunzip -c file.txt.gz > output.txt
```

## User Management

### 7.25 `whoami` and `id`

```bash
# Current user
whoami

# User and group IDs
id

# Output:
# uid=1000(username) gid=1000(username) groups=1000(username),4(adm),24(cdrom),27(sudo)
```

### 7.26 `sudo` — Execute as Root

```bash
# Run command as root
sudo apt update

# Edit file as root
sudo nano /etc/nginx/nginx.conf

# Become root user
sudo -i

# Run as specific user
sudo -u username command
```

## Practice Exercises

```bash
# 1. Check system info
uname -a
hostname
uptime

# 2. Find your top processes by memory
ps aux --sort=-%mem | head -10

# 3. Check disk usage
df -h
du -sh /var/* | sort -h | tail -5

# 4. Test network connectivity
ping -c 3 google.com

# 5. Download a small file
curl -o test.html https://example.com

# 6. Create and compress a directory
tar -czvf backup.tar.gz Documents/

# 7. Find all .log files older than 7 days
find /var -name "*.log" -mtime +7
```

## Summary

You've expanded your command toolkit with:

- **System info**: `uname`, `hostname`, `uptime`, `date`
- **Process management**: `ps`, `top`, `htop`, `kill`, `pkill`
- **Resource monitoring**: `free`, `df`, `du`
- **Networking**: `ip`, `ping`, `curl`, `wget`, `ssh`
- **Searching**: `find`, `locate`, `which`
- **Archives**: `tar`, `zip`, `gzip`

These commands will handle 90% of the system administration tasks you'll encounter as a developer.

In the next chapter, we'll explore one of Linux's most powerful features: piping and redirection—connecting commands to create complex data flows.

---

**Previous Chapter**: [Chapter 6: Working with Files](06-working-with-files.md)
**Next Chapter**: [Chapter 8: Piping and Redirection](08-piping-redirection.md) →
