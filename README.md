# GitHub SSH Key Setup - tech610-test-ssh

## Overview

This repository demonstrates how to configure SSH authentication between a local machine and GitHub.

SSH keys allow secure authentication when using Git commands such as `git push`, `git pull`, and `git clone` without entering a username and password each time.

---

# Objectives

- Generate an SSH key pair for GitHub authentication.
- Configure the SSH agent.
- Add the public SSH key to GitHub.
- Test GitHub SSH connectivity.
- Create and push a Git repository using SSH.

---

# SSH Key Generation

An Ed25519 SSH key pair was generated:

```bash
ssh-keygen -t ed25519 -a 100 -C "suhaib@tech610"
```

This creates two files:

| File | Purpose |
|---|---|
| `id_ed25519` | Private key stored locally and used for authentication |
| `id_ed25519.pub` | Public key added to GitHub |

The private key must remain secure and should never be uploaded or shared.

---

# Configure SSH Agent

The SSH agent was started to manage the private key:

```bash
eval "$(ssh-agent -s)"
```

The private key was added:

```bash
ssh-add ~/.ssh/id_ed25519
```

To verify the key was loaded:

```bash
ssh-add -l
```

---

# Add SSH Key to GitHub

The public key was displayed using:

```bash
cat ~/.ssh/id_ed25519.pub
```

The public key was added to:

```
GitHub → Settings → SSH and GPG Keys → New SSH Key
```

Configuration:

```
Title:
suhaib@tech610

Key Type:
Authentication Key
```

Only the public key is added to GitHub.

The private key remains stored locally.

---

# Test GitHub SSH Connection

The SSH connection was tested:

```bash
ssh -T git@github.com
```

Successful authentication returned:

```
Hi SMK121! You've successfully authenticated, but GitHub does not provide shell access.
```

This confirmed that SSH authentication was correctly configured.

---

# Repository Setup

A local repository was created:

```
tech610-test-ssh
```

Repository location:

```
Training & Project Work/tech610-test-ssh
```

Git was initialised:

```bash
git init
```

A README file was created:

```bash
echo "# tech610-test-ssh" > README.md
```

---

# Connect Local Repository to GitHub

The GitHub repository was connected using SSH:

```bash
git remote add origin git@github.com:SMK121/tech610-test-ssh.git
```

Verify the remote connection:

```bash
git remote -v
```

Example output:

```
origin git@github.com:SMK121/tech610-test-ssh.git (fetch)
origin git@github.com:SMK121/tech610-test-ssh.git (push)
```

---

# Git Workflow

Changes were staged:

```bash
git add .
```

A commit was created:

```bash
git commit -m "Add README file"
```

The first push connected the local branch to GitHub:

```bash
git push -u origin main
```

Future updates can be pushed using:

```bash
git push
```

---

# Issue Encountered

## Push Rejected Due to Different Histories

The first push failed because the GitHub repository already contained a README file created remotely.

The local repository and GitHub repository had separate commit histories.

The issue was resolved by merging the histories:

```bash
git pull origin main --allow-unrelated-histories
```

After completing the merge:

```bash
git commit -m "Merge remote changes"
git push
```

---

# SSH Authentication Workflow

SSH authentication uses a public/private key pair to securely authenticate between the local machine and GitHub.

```
+--------------------------------+
| Local Machine                  |
|                                |
| ~/.ssh/                        |
|                                |
| id_ed25519                     |
| Private Key                    |
|                                |
| Never shared or uploaded       |
+---------------+----------------+
                |
                |
                | ssh-add ~/.ssh/id_ed25519
                |
                v
+--------------------------------+
| SSH Agent                      |
|                                |
| Stores private key temporarily |
| Handles authentication         |
+---------------+----------------+
                |
                |
                | SSH Authentication Request
                |
                v
+--------------------------------+
| GitHub                         |
|                                |
| SSH Settings                   |
|                                |
| id_ed25519.pub                 |
| Public Key                     |
|                                |
| Added to GitHub                |
+---------------+----------------+
                |
                |
                v
+--------------------------------+
| GitHub Repository              |
|                                |
| tech610-test-ssh               |
|                                |
| git push / git pull using SSH  |
+--------------------------------+
```

The private key stays on the local machine, while only the public key is added to GitHub.

---

# Key Concepts Learned

## SSH Authentication

SSH uses public-key cryptography to securely authenticate users.

The private key remains on the local machine, while the public key is shared with GitHub.

## Public vs Private Keys

| Key | Purpose |
|---|---|
| Private Key (`id_ed25519`) | Stored locally and used for authentication |
| Public Key (`id_ed25519.pub`) | Added to GitHub for verification |

---

## Benefits of Using SSH with GitHub

- Secure authentication.
- No repeated password/token entry.
- Faster Git operations.
- Commonly used in DevOps workflows.

---

# Security Considerations

Sensitive information should never be committed to GitHub.

Examples of files that should not be uploaded:

```
*.pem
*.key
.env
AWS credentials
Private SSH keys
API tokens
Passwords
```

The `.ssh` folder and private SSH keys should never be uploaded to a Git repository.

Example:

```
Local Machine

~/.ssh/
├── id_ed25519
└── id_ed25519.pub


GitHub Repository

tech610-test-ssh
└── README.md
```

Only the public key is shared with GitHub.

A `.gitignore` file should be used to prevent accidental uploads:

```
*.pem
*.key
.env
id_ed25519
id_rsa
```

---

# Final Outcome

Successfully configured SSH authentication with GitHub and pushed changes to the:

```
tech610-test-ssh
```

repository using SSH.
