# Linux for Self-Taught Devs

<p align="center">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License">
  <img src="https://img.shields.io/badge/Contributions-Welcome-brightgreen.svg" alt="Contributions">
  <img src="https://img.shields.io/badge/Status-Complete-brightgreen.svg" alt="Status">
</p>

> A comprehensive guide to Linux for developers who are self-taught and want to build professional-grade skills.

**Start here**: [00-how-to-use-this-book.md](00-how-to-use-this-book.md)

## 📖 About the Book

**Linux for Self-Taught Devs** is a 16-chapter book designed to take you from Linux novice to competent developer-environment manager. Whether you're coming from Windows, macOS, or no prior OS experience, this book meets you where you are.

### What's Inside

| Chapter | Title                            | Description                                            |
| ------- | -------------------------------- | ------------------------------------------------------ |
| 0       | How to Use This Book             | Guide to getting the most from this book               |
| 1       | Why Linux for Developers         | The case for Linux in modern development               |
| 2       | Linux vs Other Operating Systems | Detailed comparison with Windows and macOS             |
| 3       | Choosing Your Setup              | Picking the right distribution and installation method |
| 4       | Terminal Basics                  | Your first commands in the shell                       |
| 5       | The Filesystem                   | Understanding Linux's directory structure              |
| 6       | Working with Files               | Creating, editing, and managing files                  |
| 7       | Essential Commands               | The commands every developer must know                 |
| 8       | Piping and Redirection           | Data flow and command chaining                         |
| 9       | Package Management               | Installing and managing software                       |
| 10      | Permissions and Security         | Users, groups, and file permissions                    |
| 11      | Git on Linux                     | Version control setup and workflow                     |
| 12      | Bash Scripting                   | Automating your workflow                               |
| 13      | Development Environment          | IDEs, tools, and productivity                          |
| 14      | Remote Deployment                | Working with servers                                   |
| 15      | Troubleshooting                  | Debugging common Linux issues                          |

### Target Audience

- **Self-taught developers** looking to expand into Linux
- **Career changers** transitioning from other fields to software development
- **Students** learning operating systems concepts
- **Anyone** wanting to understand the backbone of modern computing

### Learning Path

```
Week 0     → Chapter 0 (How to Use)
Week 1-2   → Chapters 1-4 (Foundation)
Week 3-4   → Chapters 5-8 (Core Skills)
Week 5-6   → Chapters 9-12 (Professional Dev)
Week 7-8   → Chapters 13-15 (Real-World App)
```

---

## 🚀 Quick Start

### Reading the Book

Simply open the markdown files in your preferred editor:

```bash
# Clone the repository
git clone https://github.com/yourusername/Linux-for-Self-Taught-Devs.git
cd Linux-for-Self-Taught-Devs

# Start reading
cat 01-why-linux.md
```

### Recommended Tools

- **VS Code** with Markdown All in One extension
- **Obsidian** for note-taking alongside reading
- **GitBook** or **MkDocs** for local HTML generation

---

## 📂 Project Structure

```
Linux-for-Self-Taught-Devs/
├── README.md              # This file
├── LICENSE                # MIT License
├── 01-why-linux.md        # Chapter 1
├── 02-linux-vs-others.md # Chapter 2
├── 03-choosing-setup.md   # Chapter 3
├── 04-terminal-basics.md # Chapter 4
├── 05-filesystem.md      # Chapter 5
├── 06-working-with-files.md
├── 07-essential-commands.md
├── 08-piping-redirection.md
├── 09-package-management.md
├── 10-permissions-security.md
├── 11-git-on-linux.md
├── 12-bash-scripting.md
├── 13-dev-environment.md
├── 14-remote-deployment.md
├── 15-troubleshooting.md
└── assets/                # Images and diagrams
```

---

## 🔧 Generating PDF

### Option 1: Using Pandoc (Recommended)

```bash
# Install pandoc
# macOS
brew install pandoc

# Ubuntu/Debian
sudo apt install pandoc

# Windows (with Chocolatey)
choco install pandoc

# Generate PDF
pandoc *.md -o Linux-for-Self-Taught-Devs.pdf --pdf-engine=xelatex \
  --toc \
  --toc-depth=2 \
  --highlight-style=tango \
  -V mainfont="Georgia" \
  -V sansfont="Arial" \
  -V monofont="Courier New" \
  -V geometry:margin=1in
```

### Option 2: Using Markdown Here (VS Code Extension)

1. Install "Markdown Here" extension in VS Code
2. Open all chapters in VS Code
3. Press `Ctrl+Shift+P` → "Markdown Here: Preview"
4. Print to PDF from browser

### Option 3: Using GitBook

```bash
# Install gitbook CLI
npm install -g gitbook-cli

# Initialize
gitbook init

# Build PDF
gitbook pdf . Linux-for-Self-Taught-Devs.pdf
```

### Option 4: Online Services

- **GitHub**: Upload to private repo → GitHub Actions → Release as PDF
- **StackEdit**: Import all files → Export to PDF
- **Dillinger**: Paste content → Download HTML → Print to PDF

---

## 🤝 Contributing

We welcome contributions! This is a community-driven book.

### Ways to Contribute

1. **Write Content** — Add new chapters or expand existing ones
2. **Edit** — Fix typos, improve clarity, update outdated info
3. **Translate** — Help reach non-English speakers
4. **Review** — Provide feedback on existing content
5. **Report Issues** — Help identify problems

### Contribution Guidelines

#### Before You Start

1. **Check existing issues** to avoid duplication
2. **Fork the repository**
3. **Create a feature branch**: `git checkout -b feature/your-feature`

#### Writing Standards

- **Tone**: Conversational but professional
- **Code blocks**: Use triple backticks with language identifiers
- **Headings**: Use `#` for chapter titles, `##` for sections
- **Links**: Use relative links for internal references

````markdown
# Good Example

## Installing Node.js

Use your package manager:

```bash
# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```
````

See [Chapter 4: Terminal Basics](04-terminal-basics.md) for more on using the terminal.

```

#### Pull Request Process

1. Update Table of Contents in README.md
2. Ensure all links work
3. Check code examples are accurate
4. Submit PR with clear description
5. Wait for review (typically 48 hours)

### Code of Conduct

- Be respectful and inclusive
- Provide constructive feedback
- Welcome newcomers
- Focus on the content, not the person

---

## 📋 TODO / Help Wanted

- [ ] Complete Chapters 5-15
- [ ] Add code examples for all chapters
- [ ] Create diagrams for filesystem and networking chapters
- [ ] Add exercises at end of each chapter
- [ ] Translate to Spanish (initial translation started)
- [ ] Create audiobook version
- [ ] Add video companion series

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

```

MIT License

Copyright (c) 2024 Linux for Self-Taught Devs

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

```

---

## 🙏 Acknowledgments

- The Linux community for decades of documentation
- Open source projects that make Linux great
- All contributors who have helped shape this book

---

## 📬 Contact

- **Issues**: Open a GitHub issue for bugs/suggestions
- **Discussions**: Use GitHub Discussions for questions
- **Email**: maintainer@email.com (optional)

---

<p align="center">
  Made with ❤️ for the self-taught developer community
</p>
```
