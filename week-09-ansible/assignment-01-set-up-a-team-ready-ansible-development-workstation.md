# Assignment 01 — Set Up a Team-Ready Ansible Development Workstation

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will prepare an isolated and reusable Ansible development workstation.

You will install Ansible and supporting tools inside a Python virtual environment, configure VS Code, prepare SSH access, configure Git and pre-commit hooks, and document the complete setup.

This workstation will be used as the Ansible controller in upcoming assignments.

---

**FLAG (renamed and restructured):** Your submitted file was `assignment-01-onboarding-workstation-setup-standards-and-ai.md` with a different task list. The current upstream file is `assignment-01-set-up-a-team-ready-ansible-development-workstation.md`. Your work has been mapped onto the new Tasks where the content matches. Partial matches are flagged. Your old file should be removed once this is merged. Please confirm before I delete it.

---

# Task 1 — Create and Initialize the Ansible Workspace

## Goal

Create the assignment workspace, initialize a Git repository, prepare the required directories, and add Git ignore rules for local and sensitive files.

### Evidence

#### Screenshot 1 — Terminal showing the `ansible-onboarding` path, `ls -la` output, and `git status` confirming the Git repository is on the `main` branch

Add your screenshot here.

**FLAG:** Not in your snapshot. Your Screenshot 9 (repository tree) is used under Task 8 instead.

---

# Task 2 — Create the Virtual Environment and Install Ansible Tools

## Goal

Create an isolated Python virtual environment and install Ansible and the required validation tools without modifying the system Python environment.

### Evidence

#### Screenshot 2 — Terminal showing the active `(.venv)` environment, `which ansible`, `ansible --version`, `ansible-lint --version`, `yamllint --version`, and `pre-commit --version`

![Screenshot 1](screenshots/assignment-01/Screenshot%201.png)

![Screenshot 2](screenshots/assignment-01/Screenshot%202.png)

**FLAG:** Screenshot 2 expects `which ansible` and `pre-commit --version`. Please confirm both appear in your images.

---

# Task 3 — Configure VS Code for Ansible Development

## Goal

Configure Visual Studio Code to use the project’s Python virtual environment and provide validation support for Python, YAML, and Ansible files.

### Evidence

#### Screenshot 3 — VS Code Extensions panel showing the Ansible, YAML, and Python extensions installed

![Screenshot 3A](screenshots/assignment-01/Screenshot%203A.png)

![Screenshot 3B](screenshots/assignment-01/Screenshot%203B.png)

---

#### Screenshot 4 — VS Code showing `.vscode/settings.json` and `.editorconfig` open side by side, with the required settings clearly visible

![Screenshot 4](screenshots/assignment-01/Screenshot%204.png)

---

# Task 4 — Create the Baseline Ansible Configuration

## Goal

Create a reusable `ansible.cfg` file containing the default settings that will be used in this workspace and upcoming Ansible assignments.

### Evidence

#### Screenshot 5 — `ansible.cfg` open in VS Code or another editor, showing the complete configuration

![Screenshot 5](screenshots/assignment-01/Screenshot%205.png)

---

#### Screenshot 6 — Terminal showing `ansible --version` with the `ansible.cfg` path and the output of `ansible-config dump --only-changed`

Add your screenshot here.

**FLAG:** Not in your snapshot.

---

# Task 5 — Configure SSH Readiness

## Goal

Prepare SSH key authentication, load the key into the SSH agent, configure reusable SSH client settings, and understand how trusted host fingerprints are stored.

### Evidence

#### Screenshot 7 — Terminal showing `ssh-add -l` with the ED25519 key loaded and the SSH configuration verification output

![Screenshot 6](screenshots/assignment-01/Screenshot%206.png)

**FLAG (partial):** Your Screenshot 6 shows `ssh-add -l`. The SSH configuration verification output that upstream also asks for is not in your snapshot.

---

# Task 6 — Configure Git Identity and Pre-commit Hooks

## Goal

Configure your Git identity and install pre-commit hooks that validate YAML and Ansible files before commits are created.

### Evidence

#### Screenshot 8 — Terminal showing your Git full name, Git email, default branch, successful `pre-commit install` output, and `.git/hooks/pre-commit`

![Screenshot 7](screenshots/assignment-01/Screenshot%207.png)

**FLAG (partial):** Your Screenshot 7 shows `pre-commit install` output. The Git name, email, and default-branch lines are not in the image.

---

# Task 7 — Test the Complete Workstation Setup

## Goal

Verify that Ansible, the linting tools, Git hooks, SSH agent, and Git ignore rules are working correctly.

### Evidence

#### Screenshot 9 — Terminal showing `pre-commit run --all-files` completing successfully

![Screenshot 8](screenshots/assignment-01/Screenshot%208.png)

---

#### Screenshot 10 — Terminal showing `ansible --version` with the project configuration path and `ssh-add -l` with the ED25519 key loaded

Add your screenshot here.

**FLAG:** Not in your snapshot.

---

# Task 8 — Create the README and Onboarding Checklist

## Goal

Document the completed Ansible workstation setup and create a reusable checklist for preparing another workstation in the future.

### Evidence

#### Screenshot 11 — Terminal showing the final `ansible-onboarding` project structure

![Screenshot 9](screenshots/assignment-01/Screenshot%209.png)

**FLAG (partial):** Your Screenshot 9 is a repository tree. Confirm it is the final `ansible-onboarding` structure.

---

#### Screenshot 12 — VS Code Markdown preview showing your full name, project summary, and part of the “New Machine? Do This” checklist

![Screenshot 10](screenshots/assignment-01/Screenshot%2010.png)

**FLAG:** Confirm your full name is visible in this image. The checklist is needed to meet the "10–12 items" requirement.

---

# Task 9 — Share Your Progress on WhatsApp

## Goal

Publish a WhatsApp Status showing a screenshot of your active Python virtual environment and Ansible version, a short caption about what you completed and learned, and your personal DMI progress link.

Get your progress link from **DMI Leaderboard → Your name → Share your progress**.

### Evidence

#### Screenshot 13 — Published WhatsApp Status showing your Ansible workstation screenshot, caption, and personal DMI progress link

Add your screenshot here.

> **Privacy Note:** Do not display private keys, credentials, private messages, phone numbers, or other sensitive information.

**FLAG:** New upstream task. Not in your snapshot.

---

# Assignment Questions

Answer the following in your own words:

**1. What is one feature that makes your workstation setup team-friendly?**

Team-friendly feature: requirements.txt pins every tool version, and .vscode/settings.json points the editor at the project's .venv. A new teammate who runs pip install -r requirements.txt gets the same Ansible and lint results as everyone else.

---

**2. What is one pitfall you avoided while completing the setup?**

Pitfall avoided: The staged copy of .pre-commit-config.yaml still had trailing whitespace, even though the working copy was already fixed. That would have failed the yamllint and ansible-lint hooks on commit. I caught it because the linter failed, and I staged the corrected file so the commit matched the working copy.

*Earlier note on corporate proxy or CA certificates (kept as written):* None were needed in this environment. Package installs, the pre-commit hooks, and the push to GitHub all worked without proxy or certificate changes. If you're on a corporate network, you may need to configure those yourself.

---

**3. Why should Ansible be installed inside a Python virtual environment?**

Add your answer here.

---

**4. Why must SSH private keys and `.venv/` remain outside version control?**

Add your answer here.

---

# Required Files

Confirm that the following files are included in your assignment workspace:

- [ ] `README.md`
- [ ] `requirements.txt`
- [ ] `.gitignore`
- [ ] `.editorconfig`
- [ ] `.vscode/settings.json`
- [ ] `ansible.cfg`
- [ ] `.pre-commit-config.yaml`
- [ ] `inventories/` **(FLAG: confirm; not in your old checklist)**
- [ ] `roles/` **(FLAG: confirm; not in your old checklist)**

---

# Submission Instructions

- Add all required screenshots in your submission.
- Full Name must be visible in required screenshots.
- All screenshots must be readable.
- Answer all assignment questions clearly in your own words.
- Do not expose SSH private-key contents, passwords, access tokens, API keys, credentials, private certificates, or other sensitive information.

---

# Completion Checklist

- [ ] Task 1: `ansible-onboarding` workspace created **(FLAG: Screenshot 1 missing)**
- [ ] Task 1: Git initialized on the `main` branch **(FLAG: confirm)**
- [ ] Task 1: `.gitignore` created **(FLAG: confirm)**
- [x] Task 2: Python virtual environment created
- [x] Task 2: Virtual environment activated
- [x] Task 2: Ansible installed inside `.venv`
- [x] Task 2: `ansible-lint`, `yamllint`, and `pre-commit` installed
- [x] Task 2: `requirements.txt` created
- [x] Task 3: Required VS Code extensions installed
- [ ] Task 3: VS Code uses the Python interpreter from `.venv` **(FLAG: confirm)**
- [x] Task 3: `.vscode/settings.json` created
- [x] Task 3: `.editorconfig` created
- [x] Task 4: `ansible.cfg` created
- [ ] Task 4: Ansible loads `ansible.cfg` from the project directory **(FLAG: Screenshot 6 missing)**
- [x] Task 5: ED25519 SSH key exists
- [x] Task 5: SSH private key has not been exposed
- [x] Task 5: SSH key loaded into the SSH agent
- [ ] Task 5: `~/.ssh/config` contains the required settings **(FLAG: confirm)**
- [ ] Task 5: `~/.ssh/known_hosts` exists **(FLAG: confirm)**
- [x] Task 6: Git identity configured correctly **(FLAG: confirm; not in Screenshot 8)**
- [x] Task 6: Pre-commit hooks installed
- [x] Task 7: `pre-commit run --all-files` completes successfully
- [ ] Task 8: `README.md` contains your full name and workstation details **(FLAG: confirm)**
- [ ] Task 8: “New Machine? Do This” checklist contains 10–12 items **(FLAG: confirm)**
- [ ] Task 9: WhatsApp Status published with the workstation screenshot, caption, and DMI progress link **(FLAG: new task — not yet done)**
- [ ] All 13 required screenshots are included **(FLAG: 10 mapped; Screenshots 1, 6, 10, 13 missing)**
- [ ] Assignment questions are answered **(FLAG: Questions 3 and 4 unanswered)**
- [x] No sensitive information is exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra and The CloudAdvisory, focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- DMI Official Website: [https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme](https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme)
- University: [https://university.pravinmishra.com?utm_source=github&utm_medium=readme](https://university.pravinmishra.com?utm_source=github&utm_medium=readme)
- Discord Community: [https://discord.pravinmishra.com?utm_source=github&utm_medium=readme](https://discord.pravinmishra.com?utm_source=github&utm_medium=readme)
- Blog: [https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme](https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme)
- YouTube Playlist: [https://www.youtube.com/playlist?list=PLFeSNDtI4Cho](https://www.youtube.com/playlist?list=PLFeSNDtI4Cho)
- Pravin Mishra LinkedIn: [https://www.linkedin.com/in/pravin-mishra-aws-trainer/](https://www.linkedin.com/in/pravin-mishra-aws-trainer/)
- CloudAdvisory LinkedIn: [https://www.linkedin.com/company/thecloudadvisory/](https://www.linkedin.com/company/thecloudadvisory/)

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
