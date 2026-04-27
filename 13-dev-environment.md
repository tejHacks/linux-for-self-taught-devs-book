# Chapter 13: Development Environment

> _"Your development environment is your workshop. A well-organized workspace leads to better work."_

## Introduction

A proper development environment includes editors, language runtimes, databases, containers, and productivity tools. This chapter walks through setting up a complete Linux development workstation.

## Code Editors

### 13.1 Visual Studio Code

```bash
# Ubuntu/Debian
sudo apt install code

# Or use Snap
sudo snap install --classic code

# Run
code

# Install extensions from command line
code --install-extension ms-python.python
code --install-extension dbaeumer.vscode-eslint
```

### 13.2 JetBrains Toolbox (All JetBrains IDEs)

```bash
# Download from https://www.jetbrains.com/toolbox/
# Or use snap (some IDEs available)
sudo snap install intellij-idea-ultimate --classic
sudo snap install pycharm-professional --classic
```

### 13.3 Vim/Neovim (For Terminal Lovers)

```bash
# Install Neovim (modern Vim)
sudo apt install neovim

# Install Vim plugins (using vim-plug)
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
  https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

# Basic .vimrc
cat > ~/.vimrc << 'EOF'
syntax on
set number
set tabstop=4
set shiftwidth=4
set expandtab
set autoindent
colorscheme desert
EOF
```

## Programming Languages

### 13.4 Python

```bash
# Ubuntu: Install Python
sudo apt install python3 python3-pip python3-venv

# Verify
python3 --version
pip3 --version

# Create virtual environment
python3 -m venv myenv
source myenv/bin/activate

# Install packages
pip install requests flask django

# Common data science stack
pip install numpy pandas jupyterlab
```

### 13.5 Node.js

```bash
# Install Node.js 20.x (LTS)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install nodejs

# Verify
node --version
npm --version

# Install global packages
npm install -g yarn pm2

# Create new project
npm init -y
npm install express
```

### 13.6 Java

```bash
# Install OpenJDK 17
sudo apt install openjdk-17-jdk

# Verify
java --version
javac --version

# Set JAVA_HOME
echo 'export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64' >> ~/.bashrc
source ~/.bashrc
```

### 13.7 Go

```bash
# Install Go
sudo apt install golang-go

# Verify
go version

# Set GOPATH
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$PATH:$GOPATH/bin' >> ~/.bashrc
source ~/.bashrc

# Create and run program
mkdir -p $HOME/go/src/hello
cat > $HOME/go/src/hello/main.go << 'EOF'
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
EOF
go run $HOME/go/src/hello/main.go
```

### 13.8 Rust

```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Source cargo
source $HOME/.cargo/env

# Verify
rustc --version
cargo --version

# Create project
cargo new hello_rust
cd hello_rust
cargo run
```

## Databases

### 13.9 PostgreSQL

```bash
# Install
sudo apt install postgresql postgresql-contrib

# Start service
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Access PostgreSQL console
sudo -u postgres psql

# Create user and database
# In psql:
# CREATE USER devuser WITH PASSWORD 'password';
# CREATE DATABASE myapp OWNER devuser;
# \q
```

### 13.10 MySQL/MariaDB

```bash
# Install MariaDB (MySQL replacement)
sudo apt install mariadb-server

# Start service
sudo systemctl start mariadb
sudo systemctl enable mariadb

# Secure installation
sudo mysql_secure_installation

# Access
sudo mysql -u root
```

### 13.11 Redis

```bash
# Install
sudo apt install redis-server

# Start service
sudo systemctl start redis
sudo systemctl enable redis

# Test connection
redis-cli ping
# Should return: PONG
```

### 13.12 MongoDB

```bash
# Install MongoDB
sudo apt install mongodb

# Or use Docker
docker run -d -p 27017:27017 mongo

# Start service
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

## Containers

### 13.13 Docker

```bash
# Install Docker
sudo apt install docker.io

# Start service
sudo systemctl start docker
sudo systemctl enable docker

# Add user to docker group (no sudo needed)
sudo usermod -aG docker $USER
newgrp docker

# Verify
docker --version
docker run hello-world

# Common commands
docker ps              # Running containers
docker ps -a           # All containers
docker images          # Local images
docker pull nginx      # Download image
docker run -d -p 8080:80 nginx  # Run nginx
```

### 13.14 Docker Compose

```bash
# Install Docker Compose
sudo apt install docker-compose

# Verify
docker-compose --version

# Example docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - db_data:/var/lib/postgresql/data
volumes:
  db_data:
EOF

# Run
docker-compose up -d
```

### 13.15 Kubernetes (Minikube for local)

```bash
# Install kubectl
sudo apt install kubectl

# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start cluster
minikube start
minikube status
```

## Version Control

### 13.16 Git Configuration

```bash
# Already covered in Chapter 11, but here's a pro setup:

git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Useful aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.last 'log -1 HEAD --format="%s"'
git config --global alias.graph 'log --oneline --graph --all'

# Better diff
git config --global diff.colorMoved default

# Credential helper (cache for 1 hour)
git config --global credential.helper cache --timeout=3600
```

## Terminal Enhancements

### 13.17 Oh My Zsh (Better Shell)

```bash
# Install Zsh
sudo apt install zsh

# Install Oh My Zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Useful plugins
# Edit ~/.zshrc
plugins=(git docker python kubectl)

# Themes
ZSH_THEME="robbyrussell"
```

### 13.18 Starship (Fast Prompt)

```bash
# Install starship
curl -sS https://starship.rs/install.sh | sh

# Add to .bashrc or .zshrc
eval "$(starship init bash)"
```

### 13.19 Terminal Multiplexers

```bash
# Install tmux
sudo apt install tmux

# Basic usage
tmux new -s mysession    # Create session
Ctrl+b d                 # Detach
tmux ls                  # List sessions
tmux attach -t mysession # Attach

# .tmux.conf basic config
cat > ~/.tmux.conf << 'EOF'
# Set prefix to Ctrl+a
set -g prefix C-a
unbind C-b

# Enable mouse
set -g mouse on

# Start window numbering at 1
set -g base-index 1

# Split panes with | and -
bind | split-window -h
bind - split-window -v
EOF
```

## API Testing Tools

### 13.20 HTTP Clients

```bash
# cURL (already installed)
curl -X GET https://api.example.com/data

# HTTPie (user-friendly)
sudo apt install httpie

# Usage
http GET api.example.com/data
http POST api.example.com/data name="John"
```

### 13.21 Postman (GUI)

```bash
# Install via Snap
sudo snap install postman

# Or download from https://www.postman.com/
```

## Project Structure

### 13.22 Typical Project Layout

```
myproject/
├── .git/                 # Git repository
├── .gitignore           # Git ignore rules
├── README.md            # Project readme
├── LICENSE              # License file
├── requirements.txt     # Python dependencies
├── package.json         # Node dependencies
├── docker-compose.yml   # Docker compose config
├── .env                 # Environment variables
├── src/                 # Source code
│   ├── __init__.py
│   └── main.py
├── tests/               # Test files
│   └── test_main.py
├── docs/                # Documentation
├── scripts/             # Utility scripts
└── .github/             # GitHub workflows
```

## Practice Exercises

```bash
# 1. Install VS Code and open from terminal
code

# 2. Set up Python virtual environment
python3 -m venv myproject
source myproject/bin/activate
pip install requests

# 3. Run a container
docker run -d --name mynginx -p 8080:80 nginx

# 4. Set up tmux with config
cat > ~/.tmux.conf << 'EOF'
set -g mouse on
set -g base-index 1
EOF

# 5. Configure Git aliases
git config --global alias.graph 'log --oneline --graph --all'
```

## Summary

You've set up a complete development environment:

- **Editors**: VS Code, Neovim
- **Languages**: Python, Node.js, Java, Go, Rust
- **Databases**: PostgreSQL, MySQL, Redis, MongoDB
- **Containers**: Docker, Docker Compose
- **Terminal**: Zsh, Starship, tmux

A well-configured environment makes you more productive. Take time to customize it to your needs.

In the next chapter, we'll cover remote deployment—working with servers.

---

**Previous Chapter**: [Chapter 12: Bash Scripting](12-bash-scripting.md)
**Next Chapter**: [Chapter 14: Remote Deployment](14-remote-deployment.md) →
