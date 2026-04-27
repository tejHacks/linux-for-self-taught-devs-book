# Chapter 10: Permissions and Security

> _"Security is not a product, but a process—and it starts with understanding permissions."_

## Introduction

Linux is a multi-user operating system. Multiple people can use the same machine, each with their own files and varying levels of access. Understanding permissions is crucial for:

- Keeping your files private
- Running services securely
- Troubleshooting "permission denied" errors
- Administering Linux servers

This chapter covers the Linux permission model from basics to advanced.

## The Permission Model

### 10.1 Users and Groups

Linux identifies users and groups by numbers:

```bash
# View user database
cat /etc/passwd

# Output example:
# root:x:0:0:root:/root:/bin/bash
# daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
# ...
# username:x:1000:1000:Username,,,:/home/username:/bin/bash

# Format: username:password:UID:GID:GECOS:home:shell
```

**UID (User ID)**:

- `0`: root (superuser)
- `1-999`: System users (services)
- `1000+`: Regular users

```bash
# View groups
cat /etc/group

# Your identity
id
# Output: uid=1000(username) gid=1000(username) groups=1000(username)
```

### 10.2 File Permissions Explained

When you run `ls -la`, you see something like:

```
-rw-r--r--  1 username group  1234 Apr 26 10:00 myfile.txt
drwxr-xr-x  1 username group  4096 Apr 26 10:00 mydirectory/
lrwxrwxrwx  1 username group    12 Apr 26 10:00 mylink -> target
```

**Breaking it down**:

```
[1] [2]   [3]  [4]    [5]     [6]        [7]      [8]
- rw- r-- r--  1 username group  1234 Apr 26 10:00 myfile.txt
| |  |  |  |   |    |       |      |        |        |
| |  |  |  |   |    |       |      |        |        +-- timestamp
| |  |  |  |   |    |       |      |        +-- filename
| |  |  |  |   |    |       |      +-- size (bytes)
| |  |  |  |   |    |       +-- group name
| |  |  |  |   |    +-- user name
| |  |  |  |   +-- number of hard links
| |  |  |  +-- other permissions (rwx)
| |  |  +-- group permissions (rwx)
| |  +-- owner permissions (rwx)
+-- file type (-=file, d=directory, l=symlink)
```

### 10.3 Permission Bits

| Symbol | Meaning       | Binary | Octal |
| ------ | ------------- | ------ | ----- |
| `r`    | Read          | 100    | 4     |
| `w`    | Write         | 010    | 2     |
| `x`    | Execute       | 001    | 1     |
| `-`    | No permission | 000    | 0     |

**Permission combinations**:

```
rwx = 7 (4+2+1) = Read + Write + Execute
rw- = 6 (4+2+0) = Read + Write
r-x = 5 (4+0+1) = Read + Execute
r-- = 4 (4+0+0) = Read only
-wx = 3 (0+2+1) = Write + Execute
-w- = 2 (0+2+0) = Write only
--x = 1 (0+0+1) = Execute only
--- = 0 (0+0+0) = No permissions
```

### 10.4 Numeric Permission Notation

```bash
# Common permission sets:
# 644 = rw-r--r-- (owner rw, others r)
# 755 = rwxr-xr-x (owner rwx, others rx)
# 600 = rw------- (owner rw, others none)
# 700 = rwx------ (owner only)
# 777 = rwxrwxrwx (everyone full access)
```

## Changing Permissions

### 10.5 `chmod` — Change Mode

```bash
# Add execute permission to file
chmod +x script.sh

# Remove write permission from others
chmod -w myfile.txt

# Set specific permissions (numeric)
chmod 755 script.sh

# Set owner only
chmod 600 secret.txt

# Recursive (directories)
chmod -R 755 myfolder/

# Change only files (not directories)
find /path -type f -exec chmod 644 {} \;

# Change only directories
find /path -type d -exec chmod 755 {} \;
```

**Permission change symbols**:

```bash
# u = user (owner)
# g = group
# o = others
# a = all

chmod u+x file.sh      # Owner: add execute
chmod g-w file.sh      # Group: remove write
chmod o=r file.txt    # Others: set to read only
chmod a+rx file.sh    # All: add read and execute
```

### 10.6 `chown` — Change Owner

```bash
# Change file owner
sudo chown username file.txt

# Change owner and group
sudo chown username:group file.txt

# Change group only
sudo chown :group file.txt

# Recursive
sudo chown -R username:group directory/
```

### 10.7 `chgrp` — Change Group

```bash
# Change group ownership
chgrp groupname file.txt

# Recursive
chgrp -R groupname directory/
```

## Special Permissions

### 10.8 Setuid (`4000`)

When set on an executable, runs as the file owner (not the running user):

```bash
# Setuid example (password command)
ls -l /usr/bin/passwd
# -rwsr-xr-x 1 root root ... /usr/bin/passwd
#         ^ s instead of x means setuid

# Set setuid
chmod u+s executable
chmod 4755 executable
```

> ⚠️ **Security note**: Setuid is dangerous. Only use when absolutely necessary.

### 10.9 Setgid (`2000`)

On executables: runs with group permissions. On directories: new files inherit group:

```bash
# Setgid on directory
chmod g+s shared-folder/
ls -ld shared-folder/
# drwxr-sr-x ... shared-folder/

# Setgid numeric
chmod 2775 shared-folder/
```

### 10.10 Sticky Bit (`1000`)

On directories, prevents users from deleting others' files:

```bash
# /tmp has sticky bit
ls -ld /tmp
# drwxrwxrwt ... /tmp
#                 ^ t means sticky

# Set sticky bit
chmod +t mydirectory/
chmod 1777 shared/
```

## Access Control Lists (ACL)

### 10.11 Extended Permissions

ACLs provide finer-grained control than standard permissions:

```bash
# Check if file has ACL
getfacl file.txt

# Add ACL for specific user
setfacl -m u:username:rw file.txt

# Add ACL for specific group
setfacl -m g:groupname:r file.txt

# Remove ACL
setfacl -x u:username file.txt

# Remove all ACLs
setfacl -b file.txt

# Default ACL for directory
setfacl -d -m u:username:rw directory/
```

## Process Permissions

### 10.12 Understanding Process Ownership

Every running process has:

- **Real UID (RUID)**: Who started the process
- **Effective UID (EUID)**: Current permissions
- **Saved UID (SUID)**: Can be switched to

```bash
# View process ownership
ps -ef | grep nginx

# Output:
# root    1234     1  0 10:00 ?        00:00:00 nginx: master process /usr/sbin/nginx
# www-data 2345 1234  0 10:00 ?        00:00:00 nginx: worker process ...

# Notice: nginx runs as root for master, www-data for workers
```

### 10.13 Running as Different User

```bash
# Run command as another user
sudo -u username command

# Run as root
sudo command

# Become another user (requires root)
su - username

# Switch to root
sudo -i
# or
sudo su -
```

## Security Best Practices

### 10.14 Principle of Least Privilege

Only grant the minimum permissions needed:

```bash
# Bad: World-writable
chmod 777 sensitive/

# Good: Owner only
chmod 700 sensitive/

# Good for web files: Owner rw, others r
chmod 644 index.html
chmod 755 cgi-bin/
```

### 10.15 SSH Key Permissions

```bash
# SSH directory
chmod 700 ~/.ssh

# Private key (most important!)
chmod 600 ~/.ssh/id_rsa

# Public key
chmod 644 ~/.ssh/id_rsa.pub

# Known hosts
chmod 644 ~/.ssh/known_hosts
```

### 10.16 sudo Configuration

```bash
# View sudoers file (be careful!)
sudo visudo

# Example entry:
# username ALL=(ALL:ALL) ALL
# %admin ALL=(ALL) ALL
# %sudo ALL=(ALL:ALL) ALL
```

> ⚠️ **Warning**: Never edit `/etc/sudoers` directly—use `visudo` which validates syntax.

## Troubleshooting Permissions

### 10.17 Common Issues

**"Permission denied"**:

```bash
# Check current permissions
ls -la problematic-file

# Check ownership
ls -la problematic-file | awk '{print $3, $4}'

# Check your groups
groups

# Fix: Add yourself to the group
sudo usermod -aG groupname username
newgrp groupname  # Apply immediately
```

**"Operation not permitted"**:

```bash
# You're trying to do something restricted
# Check if you need root
sudo command
```

### 10.18 Debugging Steps

```bash
# 1. Who are you?
whoami
id

# 2. What are the file permissions?
ls -la file

# 3. Who owns it?
stat file

# 4. What groups are you in?
groups

# 5. Can you use sudo?
sudo -l
```

## Practice Exercises

```bash
# 1. Create a file and check its permissions
touch myfile.txt
ls -la myfile.txt

# 2. Change permissions
chmod 755 myfile.txt
ls -la myfile.txt

# 3. Try different permission sets
chmod 644 myfile.txt
chmod 600 myfile.txt
chmod 700 myfile.txt

# 4. Create a directory with restricted access
mkdir secure_dir
chmod 700 secure_dir
ls -ld secure_dir

# 5. Change owner
sudo chown root:root myfile.txt
ls -la myfile.txt

# 6. Check ACLs (if installed)
getfacl myfile.txt
```

## Summary

You've learned the Linux permission system:

- **File permissions**: Owner, group, others (rwx)
- **Numeric notation**: 755, 644, 600
- **chmod**: Change permissions
- **chown**: Change owner
- **Special permissions**: setuid, setgid, sticky bit
- **ACLs**: Fine-grained access control
- **Security**: Least privilege, proper SSH keys

Understanding permissions is essential for system administration and security. Many "mysterious" Linux issues are actually permission problems.

In the next chapter, we'll cover Git on Linux—version control setup and workflow.

---

**Previous Chapter**: [Chapter 9: Package Management](09-package-management.md)
**Next Chapter**: [Chapter 11: Git on Linux](11-git-on-linux.md) →
