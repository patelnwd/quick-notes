# WSL Ubuntu – Dev Environment Setup & Efficient Setup Guide (Windows)

This document provides a **production‑ready, repeatable checklist** to configure WSL (Ubuntu) efficiently for modern development (AWS, serverless, web, backend).

---

## 1. Verify WSL & Ubuntu Installation

### From Windows (PowerShell)
```powershell
wsl -l -v
```
Expected:
- VERSION = 2
- Ubuntu distro present

If not WSL2:
```powershell
wsl --set-default-version 2
```

---

## 2. First‑Time Ubuntu Setup

### Update System (MANDATORY)
```bash
sudo apt update && sudo apt upgrade -y
```

Install core utilities:
```bash
sudo apt install -y \
  build-essential \
  curl \
  wget \
  unzip \
  zip \
  ca-certificates \
  gnupg \
  software-properties-common
```

---

## 3. Shell Choice (Bash / ZSH / Fish)

### Default (Recommended initially): Bash
No action needed.

### Optional: Install ZSH
```bash
sudo apt install -y zsh
chsh -s $(which zsh)
```
Restart terminal.

---

## 4. ZSH Shell Installation & Configuration (Optional but Recommended)

This section installs **ZSH** with **Oh My Zsh** and the **Powerlevel10k** theme — the most popular and productive setup.

### 4.1 Install ZSH
```bash
sudo apt install -y zsh
```

Verify:
```bash
zsh --version
```

### 4.2 Set ZSH as Default Shell
```bash
chsh -s $(which zsh)
```
Close **all WSL terminals** and reopen.

Verify:
```bash
echo $SHELL
```
Expected:
```
/usr/bin/zsh
```

---

### 4.3 Install Oh My Zsh
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

### 4.4 Install Powerlevel10k Theme
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \
  ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Edit config:
```bash
nano ~/.zshrc
```

Set theme:
```bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Reload:
```bash
source ~/.zshrc
```

---

### 4.5 Install Recommended ZSH Plugins

Edit plugins list:
```bash
nano ~/.zshrc
```

```bash
plugins=(git z sudo docker kubectl node npm python history-substring-search zsh-autosuggestions zsh-syntax-highlighting)
```

Install plugins:
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

git clone https://github.com/zsh-users/zsh-history-substring-search \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/history-substring-search
```

Reload:
```bash
source ~/.zshrc
```

---

### 4.6 Configure Powerlevel10k
```bash
p10k configure
```

Recommended choices:
- Unicode: Yes
- Nerd Font: Yes
- Style: Lean or Classic
- Instant Prompt: Yes

---

### 4.7 Nerd Font Installation (Windows Side – REQUIRED)

1. Download **MesloLGS NF**
   https://github.com/romkatv/powerlevel10k#fonts
2. Install all font files (Right click → Install)
3. Windows Terminal → Settings → Ubuntu → Font face
4. Select **MesloLGS NF**

---

### 4.8 Rollback to Bash (If Needed)
```bash
chsh -s /bin/bash
sudo apt remove --purge zsh -y
```

---

## 5. Git & SSH (Critical)

### Configure Git
```bash
git config --global user.name "Your Name"
git config --global user.email "you@email.com"
```

### Generate SSH Key
```bash
ssh-keygen -t ed25519 -C "you@email.com"
```

Start SSH agent:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

## 5. Node.js (via NVM – Recommended)

```bash
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc   # or ~/.zshrc

nvm install --lts
nvm use --lts
```

Verify:
```bash
node -v
npm -v
```

---

## 6. Python Environment

```bash
sudo apt install -y python3 python3-pip python3-venv
```

Upgrade pip:
```bash
python3 -m pip install --upgrade pip
```

---

## 7. AWS Toolchain (Serverless Ready)

### AWS CLI
```bash
sudo apt install -y awscli
```

### AWS SAM CLI
```bash
pip install aws-sam-cli
```

Verify:
```bash
aws --version
sam --version
```

---

## 8. Docker (Optional – Required for Native Lambda Builds)

```bash
sudo apt install -y docker.io
author sudo usermod -aG docker $USER
```
Restart WSL.

Verify:
```bash
docker --version
```

---

## 9. File System Best Practices (IMPORTANT)

✅ Use Linux FS:
```
/home/<user>/projects
```

❌ Avoid working in:
```
/mnt/c/Users/...
```

Reason: **10–20x performance improvement**.

---

## 10. WSL Performance Tuning

### Create Windows config
```powershell
notepad $env:USERPROFILE\.wslconfig
```

Recommended:
```ini
[wsl2]
memory=8GB
processors=4
swap=2GB
```

Restart WSL:
```powershell
wsl --shutdown
```

---

## 11. VS Code Integration

### Install VS Code Extensions (Windows side)
- Remote – WSL
- ESLint
- Prettier
- Docker
- YAML

Launch VS Code from WSL:
```bash
code .
```

---

## 12. Networking & Utilities

Clipboard support:
```bash
sudo apt install -y xclip
```

Check ports:
```bash
sudo apt install -y net-tools
```

---

## 13. Aliases & Productivity

Edit:
```bash
nano ~/.bashrc   # or ~/.zshrc
```

Add:
```bash
alias ll='ls -lah'
alias gs='git status'
alias gp='git pull'
alias ..='cd ..'
```

Reload:
```bash
source ~/.bashrc
```

---

## 14. Security Hardening (Recommended)

```bash
sudo apt install -y ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

---

## 15. Validation Checklist

Run:
```bash
whoami
lsb_release -a
git --version
node -v
python3 --version
aws --version
sam --version
```

If all pass → **WSL is production‑ready** 🚀

---

## 16. Recommended Next Steps

- Setup AWS profiles (`aws configure --profile dev`)
- Create SAM project skeleton
- Configure CI/CD
- Setup Cognito + API Gateway auth

---

### Status: READY FOR DEVELOPMENT

