# Chapter 15: Troubleshooting

> _"Every expert was once a beginner who refused to give up when things broke."_

## Introduction

When something goes wrong on Linux, the system usually tells you what's wrong—you just need to know where to look and how to interpret the information. This chapter covers systematic troubleshooting approaches.

## The Troubleshooting Mindset

### 15.1 Systematic Approach

1. **Understand the symptom** — What exactly is happening?
2. **Gather information** — Logs, error messages, system state
3. **Identify possible causes** — What could cause this?
4. **Test hypotheses** — Verify each possibility
5. **Fix the root cause** — Don't just treat symptoms
6. **Document the fix** — For future reference

### 15.2 Key Principle: Read the Error

```
❌ Don't:     Panic and ask for help immediately
✅ Do:        Read the error message carefully
❌ Don't:     Try random commands
✅ Do:        Google the exact error message
❌ Don't:     Restart without understanding
✅ Do:        Check logs first
```

## Common Issues and Solutions

### 15.3 "Permission Denied"

```bash
# Check file permissions
ls -la filename

# Check your user and groups
id

# Fix: Add yourself to the group
sudo usermod -aG groupname username
newgrp groupname

# Or fix permissions (if you own the file)
chmod 644 filename    # For files
chmod 755 script.sh   # For scripts
```

### 15.4 "Command Not Found"

```bash
# Check if command exists
which command
type command
command -v command

# Install if missing
sudo apt install command

# Check PATH
echo $PATH

# Add to PATH (temporarily)
export PATH=$PATH:/custom/path

# Add to PATH (permanently)
echo 'export PATH=$PATH:/custom/path' >> ~/.bashrc
source ~/.bashrc
```

### 15.5 "No Space Left on Device"

```bash
# Check disk usage
df -h

# Find large directories
du -sh /*

# Find large files
find / -type f -size +100M 2>/dev/null | head -20

# Clean up
sudo apt clean
sudo apt autoremove
rm -rf ~/.cache/*
rm -rf /tmp/*
```

### 15.6 "Package Broken/Dependency Issues"

```bash
# Ubuntu/Debian: Fix broken packages
sudo dpkg --configure -a
sudo apt install -f
sudo apt update

# Fedora: Check for issues
sudo dnf check
sudo dnf repoquery --unsatisfied

# Remove old kernels (Ubuntu)
sudo apt autoremove --purge

# Clean package cache
sudo apt clean
```

### 15.7 "Network Not Working"

```bash
# Check interface status
ip addr
ip link set eth0 up

# Check DNS
cat /etc/resolv.conf
ping 8.8.8.8
ping google.com

# Restart network
sudo systemctl restart NetworkManager

# Check firewall
sudo ufw status
sudo iptables -L
```

### 15.8 "Service Won't Start"

```bash
# Check service status
sudo systemctl status service-name

# View detailed logs
journalctl -u service-name -n 50

# Check configuration
sudo nginx -t
sudo apache2ctl configtest

# Check ports
sudo netstat -tulpn | grep :port
ss -tulpn | grep :port
```

## Log Files

### 15.9 Important Log Locations

```bash
# System logs
ls /var/log/

# Authentication logs (who logged in?)
sudo tail -f /var/log/auth.log

# System messages
tail -f /var/log/syslog

# Kernel messages
dmesg | tail

# Application logs
# Usually in /var/log/appname/ or in the app's directory

# User-specific logs
~/.local/share/
~/.config/
```

### 15.10 Reading Logs Effectively

```bash
# View last N lines
tail -n 100 /var/log/syslog

# Follow in real-time
tail -f /var/log/syslog

# Search for errors
grep -i error /var/log/syslog
grep -i fail /var/log/syslog

# Search with context
grep -B 5 -A 5 "error" /var/log/syslog

# Use journalctl (systemd)
journalctl -xe
journalctl -u nginx --since "1 hour ago"
journalctl -p err
```

## Diagnostic Commands

### 15.11 System Information

```bash
# Full system info
uname -a

# Kernel version
cat /proc/version

# Loaded modules
lsmod

# CPU info
lscpu
cat /proc/cpuinfo

# Memory info
free -h
cat /proc/meminfo

# Disk info
lsblk
fdisk -l
```

### 15.12 Process Diagnostics

```bash
# Find process by name
pgrep -a process-name

# Find process by port
lsof -i :8080
ss -tulpn | grep 8080

# Process tree
pstree -p

# Resource usage by process
top
htop

# Process file descriptors
ls -la /proc/<PID>/fd
```

### 15.13 Network Diagnostics

```bash
# Check network interfaces
ip addr
ip link

# Check routes
ip route

# Check DNS resolution
nslookup example.com
dig example.com

# Test connectivity
ping -c 4 example.com
traceroute example.com
mtr example.com

# Check open ports
ss -tulpn
netstat -tulpn
```

### 15.14 Performance Diagnostics

```bash
# CPU usage
top
htop
mpstat 1

# Memory usage
free -h
vmstat 1

# I/O usage
iostat -x 1

# Disk I/O
iotop

# Network I/O
nethogs
iftop
```

## Debugging Scripts

### 15.15 Adding Debug Output

```bash
#!/bin/bash
set -x  # Enable debug mode

# Your script here
echo "Starting..."
variable="test"
echo "Variable: $variable"
```

### 15.16 Error Handling in Scripts

```bash
#!/bin/bash

# Exit on error
set -e

# Exit on undefined variable
set -u

# Function with error handling
safe_command() {
    if ! command; then
        echo "Command failed!" >&2
        exit 1
    fi
}

# Trap errors
trap 'echo "Error on line $LINENO"' ERR
```

## Emergency Procedures

### 15.17 Recovery Mode

```bash
# Boot into recovery mode:
# 1. Hold Shift during boot (Ubuntu)
# 2. Or select "Advanced options" from GRUB menu
# 3. Select recovery mode

# From recovery mode you can:
# - Drop to root shell
# - Run fsck on filesystems
# - Reset password
# - Reinstall GRUB
```

### 15.18 Password Recovery

```bash
# 1. Boot into recovery mode
# 2. Select "Root" option
# 3. Remount filesystem as read-write
mount -o remount,rw /

# 4. Reset password
passwd username

# 5. Reboot
reboot
```

### 15.19 Disk Failure

```bash
# Check SMART status
sudo smartctl -a /dev/sda

# Test disk
sudo smartctl -t short /dev/sda

# Check for bad blocks
sudo badblocks -sv /dev/sda
```

## Getting Help

### 15.20 Where to Find Help

| Resource         | Use For                |
| ---------------- | ---------------------- |
| `man command`    | Official documentation |
| `command --help` | Quick help             |
| `info command`   | Detailed info          |
| Google           | Error messages         |
| Stack Overflow   | Common problems        |
| Linux forums     | Specific distros       |
| Reddit r/linux   | Community advice       |

### 15.21 Asking Good Questions

```bash
# ❌ Bad question:
# "Linux not working help"

# ✅ Good question:
# "Nginx returns 502 Bad Gateway on Ubuntu 22.04.
#  I've checked:
#  - nginx status: active
#  - php-fpm status: active
#  - error.log shows: connect() failed"
```

## Prevention

### 15.22 Regular Maintenance

```bash
# Weekly:
sudo apt update && sudo apt upgrade -y
df -h  # Check disk space

# Monthly:
sudo apt autoremove
du -sh ~/.cache/*

# Quarterly:
sudo journalctl --vacuum-time=30days
sudo find / -name "*.log" -mtime +30 -delete
```

### 15.23 Backups

```bash
# Always back up:
# - /home (your files)
# - /etc (configuration)
# - Database data
# - SSH keys

# Simple backup script
#!/bin/bash
tar -czf backup_$(date +%Y%m%d).tar.gz \
    /home /etc /var/lib/postgresql
```

## Practice Exercises

```bash
# 1. Check system logs for errors
sudo journalctl -p err -n 20

# 2. Find processes using most memory
ps aux --sort=-%mem | head -10

# 3. Check disk usage and find large files
df -h
sudo du -sh /var/* 2>/dev/null | sort -rh | head -5

# 4. Test network connectivity
ping -c 3 google.com
nslookup example.com

# 5. Check service status
sudo systemctl status nginx
journalctl -u nginx -n 10
```

## Summary

You've learned troubleshooting skills:

- **Systematic approach**: Understand → Gather → Identify → Test → Fix → Document
- **Common issues**: Permission, space, network, services
- **Log files**: /var/log, journalctl
- **Diagnostic tools**: top, htop, ss, dmesg
- **Prevention**: Regular maintenance, backups

The most important skill is staying calm and reading error messages carefully. Most problems have been solved by someone before—Google is your friend.

---

**Congratulations!** You've completed _Linux for Self-Taught Devs_.

## Where to Go Next

- **Practice**: Set up a Linux VM and use it as your daily driver
- **Deepen**: Pick a topic (Docker, Kubernetes, Ansible) and specialize
- **Contribute**: Open source projects always need help
- **Certify**: Consider Linux Foundation certifications

## Final Thoughts

Linux is a journey, not a destination. Every problem you solve makes you better. Don't be afraid to break things—that's how you learn.

**Keep learning. Keep building.**

---

**Previous Chapter**: [Chapter 14: Remote Deployment](14-remote-deployment.md)
