# Uninstall fnm (Fast Node Manager)

To completely **uninstall `fnm` (Fast Node Manager)**, follow these steps based on how you installed it:

ㅤ
<details>
  <summary><strong>📚 Table of Contents (click to expand)</strong></summary>

<br>

- [1. Remove the `fnm` binary](#%F0%9F%A7%B9-1-remove-the-fnm-binary)
  - [Via winget](#%F0%9F%94%B8-if-installed-via-winget)
  - [Via Chocolatey](#%F0%9F%94%B8-if-installed-via-chocolatey)
  - [Via Scoop](#%F0%9F%94%B8-if-installed-via-scoop)
  - [Via Cargo (Rust)](#%F0%9F%94%B8-if-installed-via-cargo-rust)
  - [Manual installation](#%F0%9F%94%B8-if-installed-manually)
- [2. Delete `fnm` data directory](#2-delete-fnm-data-directory)
- [3. Clean up shell startup config](#3-clean-up-shell-startup-config)
- [Final Check](#%E2%9C%85-final-check)

</details>

ㅤ
## 🧹 1. Remove the `fnm` binary

### 🔸 If installed via winget:

```sh
winget uninstall Schniz.fnm
```

### 🔸 If installed via Chocolatey:

```sh
choco uninstall fnm
```

## 🔸 If installed via Scoop:

```sh
scoop uninstall fnm
```

### 🔸 If installed via Cargo (Rust):

```sh
cargo uninstall fnm
```

### 🔸 If installed manually:

Delete the binary folder from wherever you placed it (e.g., a directory in your system `PATH`).


ㅤ
## 2. Delete `fnm` data directory

Use PowerShell to removes installed Node.js versions and configuration, run:

```powershell
Remove-Item "$env:APPDATA\fnm" -Recurse -Force
Remove-Item "$env:LOCALAPPDATA\fnm_multishells" -Recurse -Force
```

Or manually delete:

```sh
C:\Users\<YourUsername>\AppData\Roaming\fnm   # or %APPDATA%\fnm
C:\Users\<YourUsername>\AppData\Local\fnm_multishells   # or %LOCALAPPDATA%\fnm_multishells
```

ㅤ
## 3. Clean up shell startup config

Look for and remove any lines like this from your shell profile:

* PowerShell (`$PROFILE`)

  ```powershell
  fnm env --use-on-cd --shell powershell | Out-String | Invoke-Expression
  ```

- Git Bash (`~/.bashrc` or `~/.bash_profile`)
  ```bash
  eval "$(fnm env --use-on-cd --shell bash)"
  ```

* CMD AutoRun (optional):

  ```cmd
  reg delete "HKCU\Software\Microsoft\Command Processor" /v AutoRun /f
  ```

ㅤ
## ✅ Final Check

After all steps:

```sh
fnm --version
# should show: command not found (or not recognized)
```
