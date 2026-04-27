# Chapter 14: Remote Deployment

> _"The cloud runs on Linux. Understanding remote servers is essential for modern developers."_

## Introduction

Most production applications don't run on your local machine—they run on remote servers. This chapter covers:

- Connecting to remote servers
- Deploying applications
- Managing servers
- Automation with configuration management

## Connecting to Remote Servers

### 14.1 SSH Basics

```bash
# Connect to server
ssh username@server-ip

# Connect with key
ssh -i ~/.ssh/my_key username@server-ip

# Connect on different port
ssh -p 2222 username@server-ip

# SSH config for easy connections
cat > ~/.ssh/config << 'EOF'
Host production
    HostName 203.0.113.10
    User deploy
    Port 22
    IdentityFile ~/.ssh/prod_key

Host staging
    HostName 203.0.113.20
    User deploy
    Port 22
    IdentityFile ~/.ssh/staging_key
EOF

# Now connect simply
ssh production
```

### 14.2 SSH Key Authentication

```bash
# Generate SSH key (if you don't have one)
ssh-keygen -t ed25519 -C "your_email@example.com"

# Copy key to server
ssh-copy-id username@server-ip

# Or manually:
cat ~/.ssh/id_ed25519.pub | ssh username@server-ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# Secure SSH directory and files
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

### 14.3 SSH Security Hardening

```bash
# Edit SSH config (on server)
sudo nano /etc/ssh/sshd_config

# Recommended changes:
# Port 2222                    # Change default port
# PermitRootLogin no           # Disable root login
# PasswordAuthentication no    # Disable password auth
# PubkeyAuthentication yes     # Enable key auth
# MaxAuthTries 3               # Limit attempts

# Restart SSH
sudo systemctl restart sshd
```

## File Transfer

### 14.4 SCP (Secure Copy)

```bash
# Copy file to server
scp localfile.txt username@server:/home/username/

# Copy file from server
scp username@server:/var/log/app.log ./

# Copy directory recursively
scp -r myfolder/ username@server:/destination/

# With custom port
scp -P 2222 file.txt username@server:/path/
```

### 14.5 SFTP (SSH File Transfer)

```bash
# Start SFTP session
sftp username@server-ip

# SFTP commands:
# ls              - List files
# cd directory    - Change directory
# get file.txt    - Download file
# put file.txt    - Upload file
# get -r folder/  - Download directory
# quit            - Exit
```

### 14.6 Rsync (Efficient Sync)

```bash
# Sync directory to server
rsync -avz --delete ./local-folder/ username@server:/remote/folder/

# Exclude patterns
rsync -avz --exclude '.git' --exclude 'node_modules' ./ username@server:/path/

# Compress during transfer
rsync -avz --compress ./ username@server:/path/

# Dry run (preview)
rsync -avn --delete ./ username@server:/path/
```

## Server Basics

### 14.7 Systemd Services

```bash
# Create systemd service for your app
sudo nano /etc/systemd/system/myapp.service

# Service file:
# [Unit]
# Description=My Application
# After=network.target
#
# [Service]
# Type=simple
# User=www-data
# WorkingDirectory=/opt/myapp
# ExecStart=/opt/myapp/start.sh
# Restart=always
#
# [Install]
# WantedBy=multi-user.target

# Enable and start
sudo systemctl daemon-reload
sudo systemctl enable myapp
sudo systemctl start myapp

# Check status
sudo systemctl status myapp

# View logs
journalctl -u myapp -f
```

### 14.8 Nginx as Reverse Proxy

```bash
# Install Nginx
sudo apt install nginx

# Create site config
sudo nano /etc/nginx/sites-available/myapp

# Configuration:
# server {
#     listen 80;
#     server_name example.com;
#
#     location / {
#         proxy_pass http://localhost:3000;
#         proxy_http_version 1.1;
#         proxy_set_header Upgrade $http_upgrade;
#         proxy_set_header Connection 'upgrade';
#         proxy_set_header Host $host;
#         proxy_cache_bypass $http_upgrade;
#     }
# }

# Enable site
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/

# Test config
sudo nginx -t

# Reload
sudo systemctl reload nginx
```

### 14.9 SSL/TLS with Let's Encrypt

```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Get certificate
sudo certbot --nginx -d example.com -d www.example.com

# Auto-renew (Certbot sets this up automatically)
sudo certbot renew --dry-run

# View certificates
sudo certbot certificates
```

## Deployment Strategies

### 14.10 Simple Deployment (Git Pull)

```bash
# On server:
# 1. Install git
sudo apt install git

# 2. Clone or pull your repo
cd /var/www/myapp
git pull origin main

# 3. Install dependencies
npm install  # or pip install -r requirements.txt

# 4. Restart service
sudo systemctl restart myapp
```

### 14.11 Docker Deployment

```bash
# Build and deploy with Docker
# On server:

# Pull latest image
docker pull myregistry/myapp:latest

# Stop old container
docker stop myapp

# Remove old container
docker rm myapp

# Run new container
docker run -d --name myapp -p 8080:8080 myregistry/myapp:latest

# Or use docker-compose
docker-compose -f production.yml up -d
```

### 14.12 CI/CD with GitHub Actions

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Deploy to server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /var/www/myapp
            git pull
            docker-compose up -d --build
```

## Cloud Providers

### 14.13 Common Cloud Commands

**AWS EC2**:

```bash
# Install AWS CLI
sudo apt install awscli

# Configure
aws configure

# Connect to instance (use key)
ssh -i key.pem ec2-user@public-dns
```

**DigitalOcean**:

```bash
# Install doctl
sudo snap install doctl

# Authenticate
doctl auth init

# Create droplet
doctl compute droplet create my-droplet --region nyc1 --image ubuntu-20-04-x64 --size s-1vcpu-1gb
```

### 14.14 Server Monitoring

```bash
# Install htop (if not already)
sudo apt install htop

# Monitor resources
htop

# Monitor specific service
watch -n 1 'systemctl status myapp'

# Check disk I/O
iostat -x 1 5

# Network connections
ss -tunapl
```

## Automation

### 14.15 Ansible Basics

```bash
# Install Ansible
sudo apt install ansible

# Inventory file (hosts)
cat > hosts << 'EOF'
[webservers]
server1 ansible_host=203.0.113.10
server2 ansible_host=203.0.113.20

[webservers:vars]
ansible_user=deploy
ansible_ssh_private_key_file=~/.ssh/key
EOF

# Ping all hosts
ansible all -i hosts -m ping

# Run playbook
ansible-playbook -i hosts deploy.yml
```

### 14.16 Example Playbook

```yaml
# deploy.yml
---
- hosts: webservers
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Docker
      apt:
        name: docker.io

    - name: Pull application
      docker_image:
        name: myapp
        source: pull

    - name: Run container
      docker_container:
        name: myapp
        image: myapp:latest
        ports:
          - "8080:8080"
        restart_policy: always
```

## Practice Exercises

```bash
# 1. Set up SSH key authentication to a remote server
ssh-keygen -t ed25519
ssh-copy-id username@server

# 2. Configure SSH config for easy connections
nano ~/.ssh/config

# 3. Transfer files using scp
scp -r ./myproject/ username@server:/path/

# 4. Set up a systemd service
sudo nano /etc/systemd/system/myapp.service

# 5. Configure Nginx reverse proxy
sudo nano /etc/nginx/sites-available/myapp
```

## Summary

You've learned remote deployment:

- **SSH**: Key-based authentication, security
- **File transfer**: SCP, SFTP, Rsync
- **Services**: systemd, Nginx
- **Deployment**: Git pull, Docker, CI/CD
- **Cloud**: Basic provider commands
- **Automation**: Ansible basics

These skills are essential for deploying and managing production applications.

In the final chapter, we'll cover troubleshooting—how to diagnose and fix common Linux issues.

---

**Previous Chapter**: [Chapter 13: Development Environment](13-dev-environment.md)
**Next Chapter**: [Chapter 15: Troubleshooting](15-troubleshooting.md) →
