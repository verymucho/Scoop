<h1 align="center">Scoop: Optimized Special Edition</h1>

<!--<img src="scoop.png" alt="Long live Scoop!"/>-->
<p align="center">
        <a href="https://github.com/verymucho/Scoop#what-does-scoop-do">Features</a>
        |
        <a href="https://github.com/verymucho/Scoop#installation">Installation</a>
        |
        <a href="https://github.com/ScoopInstaller/Scoop/wiki">Documentation</a>
</p>

---

<p align="center">
    <a href="https://github.com/ScoopInstaller/Scoop">
        <img src="https://img.shields.io/github/languages/code-size/ScoopInstaller/Scoop.svg" alt="Code Size" />
    </a>
    <a href="https://github.com/ScoopInstaller/Scoop">
        <img src="https://img.shields.io/github/repo-size/ScoopInstaller/Scoop.svg" alt="Repository size" />
    </a>
    <a href="https://github.com/ScoopInstaller/Scoop/actions/workflows/ci.yml">
        <img src="https://github.com/ScoopInstaller/Scoop/actions/workflows/ci.yml/badge.svg" alt="Scoop Core CI Tests" />
    </a>
    <a href="https://discord.gg/s9yRQHt">
        <img src="https://img.shields.io/badge/chat-on%20discord-7289DA.svg" alt="Discord Chat" />
    </a>
    <a href="./LICENSE">
        <img src="https://img.shields.io/badge/license-UNLICENSE%20or%20MIT-blue" alt="License" />
    </a>
</p>

A modified and optimized special edition of Scoop. It addresses common pain points through third-party solutions, enabling you to use Scoop with ease.

## What does Scoop do?

Scoop installs apps from the command line with a minimal amount of friction. It:

- Eliminates [User Account Control](https://learn.microsoft.com/windows/security/application-security/application-control/user-account-control/) (UAC) prompt notifications.
- Hides the graphical user interface (GUI) of wizard-style installers.
- Prevents polluting the `PATH` environment variable. Normally, this variable gets cluttered as different apps are installed on the device.
- Avoids unexpected side effects from installing and uninstalling apps.
- Resolves and installs dependencies automatically.
- Performs all the necessary steps to get an app to a working state.

## Features

1. Adds a hook that automatically detects and replaces download links with domestic mirrors—eliminating the need to switch Buckets and sparing you from using "trash cans." Based on ISP detection, the acceleration strategy is highly precise.
    - Supports custom GitHub Proxy mirror addresses; simply run `scoop config GH_PROXY ghfast.top` to configure.
    - If you encounter download errors or checksum mismatches, you can disable this feature by running `scoop config URL_REPLACE false`.
    - If you find any software packages that were not successfully mirrored, please feel free to open an issue.

2. When running `scoop search`, it prioritizes the use of [scoop-search](https://github.com/shilangyu/scoop-search) to perform the search, resulting in significantly faster speeds.
    - Priority Order: `scoop-search` > `PowerShell Core` > `Windows PowerShell`

3. When running `scoop update`, it avoids using `git pull` to synchronize Buckets, thereby eliminating the need to manually resolve Git commit conflicts.

4. ~~When running `scoop update`, it prioritizes the use of [hok](https://github.com/chawyehsu/hok) to synchronize Buckets via multi-threaded Rust Git2 operations.~~ (Temporarily disabled)
    - Priority Order: `PowerShell Core + Git` (Multi-threaded) > `Windows PowerShell + Git` (Single-threaded)

5. Supports the automatic creation of desktop shortcuts.
    - Enable: `scoop config DESKTOP_SHORTCUT true` (Default setting in the installation script)
    - Disable: `scoop config DESKTOP_SHORTCUT false` or `scoop config rm DESKTOP_SHORTCUT`

6. Supports the automatic creation of Control Panel uninstall entries, allowing you to uninstall or reset applications directly via the Control Panel.
    - Priority Order: First shortcut name > Application name. Uses `scoop_` + Application Name as the registry key and the Bucket name as the publisher.
    - Enable: `scoop config UNINSTALL_SHORTCUT true` (Default setting in the installation script)
    - Disable: `scoop config UNINSTALL_SHORTCUT false` or `scoop config rm UNINSTALL_SHORTCUT`

7. Repository synchronized to [GitCode](https://gitcode.com/xrgzs/scoop)，facilitating rule updates for users in China.
    - Switch to the GitHub version: `scoop config scoop_repo 'https://github.com/verymucho/scoop'`

8. The installation script automatically configures `7zip`, `git`, `aria2`, `scoop-search`, and `gsudo`, and applies relevant optimizations.

9. The installation script supports installation with administrator privileges and automatically repairs Scoop file ACLs to grant access to the current user.

## Installation

### Default Installation

The installation script is compatible with PowerShell 2.0 and higher, and supports Windows 7 SP1 and higher.

```powershell
irm www.surge.box.ca/files/scoop | iex
# Alternative：Invoke-RestMethod https://raw.githubusercontent.com/verymucho/Scoop/refs/heads/main/bin/install.ps1 | Invoke-Expression
```

Win7 SP1 (PowerShell 2.0) and higher：

```powershell
(New-Object System.Net.WebClient).DownloadString('https://www.surge.box.ca/files/scoop') | iex
# Alternative：(New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/verymucho/Scoop/refs/heads/main/bin/install.ps1') | Invoke-Expression
```

For systems that do not have PowerShell 5.1 installed, we will automatically install PowerShell 7.2 and force Scoop to execute using PowerShell 7.2.

对于 Windows PEFor Windows PE environments, `C:\Windows\System32\Robocopy.exe` must be present to install Scoop.

### Install Additional Software

Multiple items can be specified, separated by spaces.

```powershell
iex "& { $(irm www.surge.box.ca/files/scoop) } -Append xrok"
```

### Minimal Installation

Installs only the core program, `git`, and `aria2`, while adding the `main` and `sdoog` buckets.

```powershell
iex "& { $(irm www.surge.box.ca/files/scoop) } -Slim"
```

### Set Installation Path

Install to the D drive.

```powershell
iex "& { $(irm www.surge.box.ca/files/scoop) } -ScoopDir 'D:\Scoop' -ScoopGlobalDir 'D:\ScoopGlobal'"
```

### Specify GitHub Acceleration

Use a custom GitHub acceleration service.

```powershell
iex "& { $(irm www.surge.box.ca/files/scoop) } -GitHubProxy 'https://ghfast.top'"
```

### Switch to This Version

If you have already installed Scoop, you can switch to this specialized version.

```powershell
scoop config scoop_repo 'https://github.com/verymucho/Scoop'
scoop config scoop_branch 'main'

scoop update
```

### Force Update

If your Scoop installation fails to update, you can execute the following commands to force an update:

```powershell
Remove-Item -Path "~\scoop\apps\scoop\current\.git\" -Recurse -Force
scoop update
```

Or：

```powershell
Push-Location "~\scoop\apps\scoop\current\"
git fetch origin main
git reset --hard origin/main
Pop-Location
```
