# WSL 2 AlmaLinux 9 Development Environment Setup Guide

This guide will walk you through setting up a complete development environment using WSL 2 with AlmaLinux 9 for Rust development in VS Code.

## Prerequisites

- Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11
- Administrator access on your Windows machine
- Stable internet connection

## Step 1: Enable WSL 2

**Modern Windows 11 Method (Recommended):**

1. Open **PowerShell** as Administrator
2. Install WSL with the latest features:
   ```powershell
   wsl --install
   ```
   This command automatically:
   - Enables the required Windows features
   - Downloads and installs the latest Linux kernel
   - Sets WSL 2 as the default version
   - Installs Ubuntu as the default distribution (which we'll replace)

3. Restart your computer when prompted

**Alternative Method (if the above doesn't work):**

1. Open **PowerShell** as Administrator
2. Enable WSL and Virtual Machine Platform:
   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
3. Restart your computer
4. After restart, open PowerShell as Administrator again and set WSL 2 as default:
   ```powershell
   wsl --set-default-version 2
   ```

## Step 2: Install AlmaLinux 9

1. Open the **Microsoft Store**
2. Search for "AlmaLinux 9"
3. Click **Install** and wait for the download to complete
4. Launch AlmaLinux 9 from the Start menu
5. Create a new user account when prompted:
   - Enter a username (lowercase, no spaces)
   - Enter and confirm a password
   - Remember these credentials - you'll need them for `sudo` commands

## Step 3: Initial AlmaLinux Setup and Updates

Once inside your AlmaLinux terminal:

1. **Update the system packages:**
   ```bash
   sudo dnf update -y
   ```
   This may take several minutes depending on your internet connection.

2. **Install essential development tools:**
   ```bash
   sudo dnf groupinstall "Development Tools" -y
   ```

3. **Add EPEL repository:**
   ```bash
   sudo dnf install epel-release -y
   ```

4. **Update package cache after adding EPEL:**
   ```bash
   sudo dnf update -y
   ```

## Step 4: Install Required Dependencies

Install all the dependencies needed for Rust development:

1. **Install core development libraries:**
   ```bash
   sudo dnf install -y \
     openssl-devel \
     pkg-config \
     gcc \
     gcc-c++ \
     make \
     curl \
     wget \
     git \
     keychain
   ```

2. **Verify Git installation:**
   ```bash
   git --version
   ```

## Step 5: Configure Git

Set up your Git identity:

```bash
git config --global user.name "Your Full Name"
git config --global user.email "your.email@company.com"
```

## Step 6: Set Up SSH Keys for GitHub

1. **Generate SSH key:**
   ```bash
   ssh-keygen -t ed25519 -C "your.email@company.com"
   ```
   - Press Enter to accept default location
   - Enter a secure passphrase when prompted

2. **Set up SSH agent persistence:**
   ```bash
   echo 'eval $(keychain --eval --agents ssh id_ed25519)' >> ~/.bashrc
   source ~/.bashrc
   ```

3. **Display your public key:**
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

4. **Add the key to GitHub:**
   - Copy the entire output from the previous command
   - Go to GitHub.com → Settings → SSH and GPG keys
   - Click "New SSH key"
   - Paste the key and give it a descriptive title
   - Click "Add SSH key"

5. **Test the connection:**
   ```bash
   ssh -T git@github.com
   ```

## Step 7: Install Rust

1. **Download and install rustup:**
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

2. **Follow the prompts:**
   - Select option 1 (default installation)
   - Wait for the installation to complete

3. **Reload your shell environment:**
   ```bash
   source ~/.bashrc
   ```

4. **Verify Rust installation:**
   ```bash
   rustc --version
   cargo --version
   ```

5. **Set environment variable for OpenSSL:**
   ```bash
   echo 'export OPENSSL_NO_VENDOR=1' >> ~/.bashrc
   source ~/.bashrc
   ```

## Step 8: Install and Configure VS Code

1. **Install VS Code on Windows** (if not already installed):
   - Download from https://code.visualstudio.com/
   - Install with default options

2. **Install required VS Code extensions:**
   - Open VS Code
   - Install the following extensions:
     - **WSL** (ms-vscode-remote.remote-wsl)
     - **rust-analyzer** (rust-lang.rust-analyzer)
     - **CodeLLDB** (vadimcn.vscode-lldb) - for debugging

## Step 9: Connect VS Code to WSL

1. **Open VS Code**
2. **Connect to WSL:**
   - Press `Ctrl+Shift+P` to open command palette
   - Type "WSL: Connect to WSL"
   - Select your AlmaLinux distribution

3. **Alternative method:**
   - From your AlmaLinux terminal, navigate to your project directory
   - Run: `code .`
   - This will open VS Code connected to WSL

## Step 10: Test Your Setup

1. **Create a test Rust project:**
   ```bash
   mkdir ~/test-project
   cd ~/test-project
   cargo init --name test-project
   ```

2. **Open in VS Code:**
   ```bash
   code .
   ```

3. **Test compilation:**
   - VS Code should automatically detect the Rust project
   - Open `src/main.rs`
   - Make a small change (like adding a comment)
   - Save the file
   - Open a terminal in VS Code (`Ctrl+``)
   - Run: `cargo build`
   - Run: `cargo run`

## Step 11: Clone Your Actual Project

1. **Navigate to your projects directory:**
   ```bash
   mkdir -p ~/projects
   cd ~/projects
   ```

2. **Clone the project repository:**
   ```bash
   git clone git@github.com:your-org/your-project.git
   cd your-project
   ```

3. **Open in VS Code:**
   ```bash
   code .
   ```

4. **Build the project:**
   ```bash
   cargo build
   ```

## Troubleshooting

### Common Issues:

**OpenSSL compilation errors:**
- Make sure `openssl-devel` is installed
- Verify `OPENSSL_NO_VENDOR=1` is set in your environment

**VS Code not connecting to WSL:**
- Restart VS Code
- Make sure WSL extension is installed and enabled
- Try opening from WSL terminal with `code .`

**SSH key issues:**
- Make sure keychain is running: `keychain --list`
- Re-add your key: `keychain --clear && keychain ~/.ssh/id_ed25519`

**Rust analyzer not working:**
- Make sure rust-analyzer extension is installed in WSL (not just Windows)
- Reload VS Code window: `Ctrl+Shift+P` → "Developer: Reload Window"

### Useful Commands:

```bash
# Check WSL version
wsl -l -v

# Restart WSL (run from PowerShell)
wsl --shutdown

# Update Rust toolchain
rustup update

# Check installed packages
dnf list installed | grep -i rust
```

## Daily Workflow

1. Open VS Code
2. Connect to WSL (or use `code .` from WSL terminal)
3. Your SSH keys will be automatically loaded via keychain
4. Start coding!

## Notes

- Your WSL files are stored in Windows at: `\\wsl$\AlmaLinux-9\home\<username>\`
- Always work from within the WSL file system for best performance
- VS Code extensions installed in WSL are separate from Windows extensions
- The setup is portable - you can backup your WSL distribution and restore it on another machine

---

**Setup Complete!** You now have a full Rust development environment ready for building our project. If you encounter any issues, please reach out to the development team for assistance.