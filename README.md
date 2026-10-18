# ⚡ Install Doom Emacs on Windows

<p align="center">
  <img src="https://raw.githubusercontent.com/doomemacs/doomemacs/screenshots/splash.png" alt="Doom Emacs" width="550"/>
</p>

<p align="center">
  <em>A clean, modern, step-by-step guide to installing and configuring <b>Doom Emacs</b> natively on Windows 10 & 11 using <b>Winget</b> and <b>PowerShell</b>.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%20%2F%2011-0078D6?style=for-the-badge&logo=windows&logoColor=white" alt="Windows" />
  <img src="https://img.shields.io/badge/Editor-GNU%20Emacs-7F5AB6?style=for-the-badge&logo=gnuemacs&logoColor=white" alt="Emacs" />
  <img src="https://img.shields.io/badge/Framework-Doom%20Emacs-1e1e2e?style=for-the-badge&logo=doomemacs&logoColor=white" alt="Doom Emacs" />
  <img src="https://img.shields.io/badge/Package%20Manager-winget-4D5BCE?style=for-the-badge" alt="Winget" />
</p>

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Prerequisites](#-prerequisites)
- [Step 1: Install Dependencies](#step-1-install-dependencies)
- [Step 2: Set Environment Variables](#step-2-set-environment-variables)
- [Step 3: Clone Doom Emacs](#step-3-clone-doom-emacs)
- [Step 4: Install Doom Emacs](#step-4-install-doom-emacs)
- [Step 5: Setup Nerd Fonts & Icons](#step-5-setup-nerd-fonts--icons)
- [Verification & Daily Commands](#-verification--daily-commands)
- [Troubleshooting & Windows Tips](#-troubleshooting--windows-tips)

---

## 🔍 Overview

Running Doom Emacs on Windows natively provides a snappy modal editing experience without requiring WSL. This repository outlines the exact commands, environment configurations, and font settings required for a smooth Windows installation.

---

## 📦 Prerequisites

Open **PowerShell** as Administrator (or standard user with Winget available) and ensure Winget is accessible:

```powershell
winget --version
```

---

## Step 1: Install Dependencies

Install Git, Ripgrep, fd-find, and GNU Emacs using `winget`:

```powershell
# Install Git
winget install --id Git.Git -e --source winget

# Install Ripgrep (fast text search engine used by Doom)
winget install -e --id BurntSushi.ripgrep.MSVC

# Install fd (fast file search used by Projectile / Doom)
winget install -e --id sharkdp.fd

# Install GNU Emacs
winget install -e --id GNU.Emacs
```

> [!TIP]
> If you already have Git installed, you can skip `Git.Git`. Make sure `git`, `rg`, and `fd` are reachable in your PATH.

---

## Step 2: Set Environment Variables

Emacs requires a Unix-style `$HOME` directory on Windows to locate your configuration files (`~/.emacs.d` and `~/.doom.d`). Additionally, the Emacs binaries and Doom CLI tools need to be accessible in your `PATH`.

### Option A: Via PowerShell (Automated)

Run the following commands in PowerShell to persist the variables for your current user:

```powershell
# 1. Set HOME to your user profile directory (e.g., C:\Users\<Username>)
[Environment]::SetEnvironmentVariable("HOME", $env:USERPROFILE, "User")

# 2. Add Emacs and Doom binaries to User PATH
$currentPath = [Environment]::GetEnvironmentVariable("Path", "User")
$emacsBin = "C:\Program Files\Emacs\emacs-31.1\bin"
$doomBin  = "$env:USERPROFILE\.emacs.d\bin"

$newPath = "$currentPath;$emacsBin;$doomBin"
[Environment]::SetEnvironmentVariable("Path", $newPath, "User")
```

> [!NOTE]
> Check your Emacs installation directory under `C:\Program Files\Emacs\` to verify the exact version folder (e.g., `emacs-31.1` or `emacs-30.x`).

### Option B: Via Windows GUI (Manual)

1. Press <kbd>Win</kbd> + <kbd>R</kbd>, type `sysdm.cpl`, and press <kbd>Enter</kbd>.
2. Go to the **Advanced** tab and click **Environment Variables**.
3. Under **User variables**:
   - Click **New...** &rarr; Variable name: `HOME`, Variable value: `C:\Users\<YourUsername>`
   - Edit `Path` &rarr; Click **New** and add:
     - `C:\Program Files\Emacs\emacs-31.1\bin`
     - `C:\Users\<YourUsername>\.emacs.d\bin`

---

## Step 3: Clone Doom Emacs

Clone the official Doom Emacs repository into `~/.emacs.d`:

```powershell
git clone --depth 1 https://github.com/doomemacs/doomemacs ~/.emacs.d
```

> [!IMPORTANT]
> If you have existing Emacs configuration in `~/.emacs.d` or `~/.emacs`, make sure to back it up or delete it before cloning.

---

## Step 4: Install Doom Emacs

Restart PowerShell to reload the newly configured environment variables and PATH, then run:

```powershell
doom install
```

Follow the interactive prompts:
- Answer **Yes** (`y`) when asked to generate an `envvar` file.
- Answer **Yes** (`y`) to create sample configuration files in `~/.doom.d/`.

---

## Step 5: Setup Nerd Fonts & Icons

Doom Emacs relies on icon sets and patched nerd fonts for its modern UI.

### 1. Install JetBrains Mono Nerd Font
This repository includes the font file in the [`font/`](./font) directory:

1. Navigate to [`font/JetBrainsMonoNerdFont-Regular.ttf`](./font/JetBrainsMonoNerdFont-Regular.ttf).
2. Right-click the `.ttf` file and select **Install** (or **Install for all users**).

### 2. Install Icon Fonts inside Emacs
Launch Emacs and run the following command to download icons:

```elisp
M-x nerd-icons-install-fonts
```
*(Press <kbd>Alt</kbd> + <kbd>x</kbd>, type `nerd-icons-install-fonts`, and hit <kbd>Enter</kbd>)*. Once downloaded, double-click to install any generated font files if prompted.

### 3. Configure Doom to Use the Font
Open your Doom configuration file at `~/.doom.d/config.el` and add:

```elisp
;; Set default Doom font
(setq doom-font (font-spec :family "JetBrainsMono NF" :size 14 :weight 'normal)
      doom-variable-pitch-font (font-spec :family "JetBrainsMono NF" :size 14))

;; Alternative direct face attribute configuration
(set-face-attribute 'default nil
                    :family "JetBrainsMono NF"
                    :height 120
                    :weight 'normal
                    :width 'normal)
```

Run `doom sync` in your terminal to synchronize changes:

```powershell
doom sync
```

---

## 🚀 Verification & Daily Commands

| Command | Description |
| :--- | :--- |
| `doom doctor` | Diagnoses common system & configuration issues |
| `doom sync` | Synchronizes package and config updates (run after editing `packages.el` or `init.el`) |
| `doom upgrade` | Updates Doom core and all installed packages |
| `doom env` | Refreshes Doom's shell environment cache |

To launch Emacs:

```powershell
runemacs
```
*(Or search for **Emacs** in the Windows Start Menu)*

---

## 🛠️ Troubleshooting & Windows Tips

<details>
<summary><b>1. Enable Git Long Path Support</b></summary>

Windows has a MAX_PATH limit (260 characters) by default. Some Emacs packages exceed this. Enable long paths in Git:

```powershell
git config --global core.longpaths true
```
</details>

<details>
<summary><b>2. Slow Startup or Antivirus Scanning</b></summary>

Windows Defender / Antivirus can significantly slow down Emacs file I/O:
- Add an exclusion in Windows Defender for your Emacs install folder (`C:\Program Files\Emacs`).
- Add an exclusion for your home configuration directory (`C:\Users\<Username>\.emacs.d` and `C:\Users\<Username>\.doom.d`).
</details>

<details>
<summary><b>3. Running doom doctor</b></summary>

Always run `doom doctor` after installation to identify any missing optional CLI dependencies:

```powershell
doom doctor
```
</details>

---

## 🤝 Contributing

Contributions, bug reports, and tips for improving Doom Emacs on Windows are welcome! Feel free to open an issue or submit a pull request.
