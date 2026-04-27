# Chapter 9: Package Management

> _"A package manager is to software what a library is to knowledge—it organizes, indexes, and provides access."_

## Introduction

On Windows, you download installers from websites. On macOS, you use the App Store or drag apps to Applications. On Linux, you use a **package manager**—a system that handles software installation, updates, and removal.

This chapter covers how to find, install, update, and manage software on Linux.

## Package Management Concepts

### 9.1 What Is a Package?

A package is a collection of files that together form a software application:

- **Binary files**: The actual executable programs
- **Configuration files**: Default settings
- **Documentation**: Man pages, readmes
- **Dependencies**: Other packages required to run
- **Metadata**: Version, description, maintainer

### 9.2 Package Formats by Distribution

| Distribution Family | Package Format | Package Manager |
| ------------------- | -------------- | --------------- |
| Debian/Ubuntu       | `.deb`         | apt, dpkg       |
| Fedora/RHEL         | `.rpm`         | dnf, yum        |
| Arch Linux          | `.pkg.tar.zst` | pacman          |
| openSUSE            | `.rpm`         | zypper          |
| Void Linux          | `.xbps`        | xbps            |

### 9.3 Repositories

Repositories (repos) are servers that store packages. Your system knows about default repos, but you can add more:

```bash
# Ubuntu: Add PPA (Personal Package Archive)
sudo add-apt-repository ppa:ubuntu-desktop/ubuntu-make

# Fedora: Add RPM Fusion
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(version).noarch.rpm
```

## Debian/Ubuntu: APT

### 9.4 Updating Package Lists

```bash
# Update package lists
sudo apt update

# Output example:
# Hit:1 http://archive.ubuntu.com/ubuntu jammy InRelease [284 kB]
# Hit:2 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [284 kB]
# ...
# Reading package lists... Done
```

### 9.5 Upgrading Installed Packages

```bash
# Upgrade all packages (smart upgrade)
sudo apt upgrade

# Full upgrade (may install/remove packages)
sudo apt full-upgrade

# Install specific upgrades
sudo apt install --only-upgrade package_name
```

### 9.6 Installing Packages

```bash
# Install single package
sudo apt install nginx

# Install multiple packages
sudo apt install nginx postgresql redis-server

# Reinstall package
sudo apt reinstall nginx

# Install without recommended packages
sudo apt install --no-install-recommends package
```

### 9.7 Removing Packages

```bash
# Remove package (keep config files)
sudo apt remove nginx

# Remove package and config files
sudo apt purge nginx

# Remove unused dependencies
sudo apt autoremove

# Clean up package cache
sudo apt clean
```

### 9.8 Searching for Packages

```bash
# Search by name or description
apt search nginx

# Show package details
apt show nginx

# List installed packages
apt list --installed

# Check if package is installed
dpkg -l nginx
```

### 9.9 Working with `.deb` Files Directly

```bash
# Install .deb file
sudo dpkg -i package.deb

# Fix broken installation
sudo dpkg --configure -a
sudo apt install -f
```

## Fedora/RHEL: DNF

### 9.10 DNF Commands

```bash
# Update package lists
sudo dnf check-update

# Upgrade all packages
sudo dnf upgrade

# Install package
sudo dnf install nginx

# Remove package
sudo dnf remove nginx

# Search
dnf search nginx

# Package info
dnf info nginx

# List installed
dnf list installed

# History (undo operations)
dnf history
dnf history undo 5
```

### 9.11 DNF vs YUM

DNF is the modern replacement for YUM (Yellowdog Updater Modified):

```bash
# YUM (older systems)
sudo yum install nginx

# DNF (modern systems)
sudo dnf install nginx

# They're similar—DNF is just better!
```

## Arch Linux: Pacman

### 9.12 Pacman Commands

```bash
# Synchronize databases
sudo pacman -Sy

# Upgrade all packages
sudo pacman -Su

# Install package
sudo pacman -S nginx

# Remove package
sudo pacman -R nginx

# Remove with dependencies
sudo pacman -Rs nginx

# Search
pacman -Ss nginx

# Query installed
pacman -Q nginx
```

### 9.13 AUR (Arch User Repository)

The AUR has thousands of community packages:

```bash
# Using yay (AUR helper)
yay -S package-name

# Or manually
git clone https://aur.archlinux.org/package-name.git
cd package-name
makepkg -si
```

## Common Tasks Across Distributions

### 9.14 Finding and Installing Development Tools

```bash
# Ubuntu/Debian: Build essentials
sudo apt install build-essential

# Fedora: Development tools
sudo dnf groupinstall "Development Tools"

# Arch: Base development
sudo pacman -S base-devel
```

### 9.15 Installing Programming Languages

```bash
# Python (Ubuntu)
sudo apt install python3 python3-pip python3-venv

# Node.js (Ubuntu)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install nodejs

# Go (Ubuntu)
sudo apt install golang-go

# Rust (Ubuntu)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Java (Ubuntu)
sudo apt install openjdk-17-jdk
```

### 9.16 Installing Databases

```bash
# PostgreSQL
sudo apt install postgresql postgresql-contrib

# MySQL/MariaDB
sudo apt install mariadb-server

# Redis
sudo apt install redis-server

# MongoDB
sudo apt install mongodb
```

### 9.17 Installing Web Servers

```bash
# Nginx
sudo apt install nginx

# Apache
sudo apt install apache2

# Start services
sudo systemctl start nginx
sudo systemctl enable nginx    # Start on boot
```

## Package Cache Management

### 9.18 Understanding Cache

Package managers download files before installing. These accumulate:

```bash
# Ubuntu: Clean cache
sudo apt clean          # Remove downloaded .deb files
sudo apt autoclean     # Remove obsolete packages
sudo apt autoremove    # Remove unused dependencies

# Fedora: Clean cache
sudo dnf clean all

# Arch: Clean cache
sudo pacman -Scc
```

### 9.19 Holding Packages (Prevent Updates)

```bash
# Ubuntu: Hold package at current version
sudo apt-mark hold package-name

# Remove hold
sudo apt-mark unhold package-name

# List held packages
apt-mark showhold
```

## Troubleshooting

### 9.20 Fixing Broken Installations

```bash
# Ubuntu: Fix dependencies
sudo dpkg --configure -a
sudo apt install -f

# Fedora: Check for problems
sudo dnf repoquery --unsatisfied
sudo dnf repoquery --duplicates

# General: Check logs
tail -f /var/log/dpkg.log    # Ubuntu
tail -f /var/log/dnf.log     # Fedora
```

### 9.21 Dependency Issues

```bash
# Find package providing missing file
apt-file update
apt-file search missing-file.so

# Or with dnf
dnf provides "*/missing-file.so"
```

## Practice Exercises

```bash
# 1. Update package lists
sudo apt update

# 2. Search for a package
apt search nginx

# 3. Check package info
apt show nginx

# 4. Install nginx
sudo apt install nginx

# 5. Check installed version
nginx -v

# 6. Remove nginx
sudo apt remove nginx

# 7. Clean up
sudo apt autoremove
sudo apt clean
```

## Summary

You've learned package management fundamentals:

- **APT (Debian/Ubuntu)**: `apt update`, `apt install`, `apt remove`
- **DNF (Fedora)**: `dnf install`, `dnf remove`
- **Pacman (Arch)**: `pacman -S`, `pacman -R`
- **Searching**: `apt search`, `dnf search`, `pacman -Ss`
- **Cleaning**: `apt clean`, `dnf clean`, `pacman -Scc`

Package managers are one of Linux's greatest strengths—software is verified, dependencies are resolved, and updates are centralized.

In the next chapter, we'll explore permissions and security—understanding how Linux controls access to files and system resources.

---

**Previous Chapter**: [Chapter 8: Piping and Redirection](08-piping-redirection.md)
**Next Chapter**: [Chapter 10: Permissions and Security](10-permissions-security.md) →
