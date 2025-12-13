# fnm-on-windows

To use [fnm](https://github.com/Schniz/fnm) (**Fast Node Manager**) on **Windows**, here’s a step-by-step guide:

ㅤ
<details>
  <summary><strong>📑 Table of Contents (click to expand)</strong></summary>

<br>

- [1. Install `fnm` on Windows](#1-install-fnm-on-windows)
  - [Using Winget](#using-winget)
  - [Using Chocolatey](#using-chocolatey)
  - [Using Scoop](#using-scoop)
  - [Using Cargo (if you have Rust)](#using-cargo-if-you-have-rust)
  - [Manual download (from GitHub)](#manual-download-from-github)
  - [Verify installation](#verify-installation)

- [2. Set Up Shell Integration](#2-set-up-shell-integration)
  - [PowerShell](#powershell)
  - [CMD](#cmd)
  - [Git Bash (Git for Windows)](#git-bash-git-for-windows)

- [3. Use `fnm`](#3-use-fnm)
  - [Install Node.js versions](#install-nodejs-versions)
  - [List installed versions](#list-installed-versions)
  - [Use a specific version (temporarily)](#use-a-specific-version-temporarily)
  - [Set default version](#set-default-version)
  - [Check if it’s working](#check-if-its-working)

- [FAQ](#faq)
  - [Find Where `fnm` Stores Its Data](#find-where-fnm-stores-its-data)
  - [Use different version of Node.js for a specific project](#use-different-version-of-nodejs-for-a-specific-project)

- [Uninstall](#uninstall)

</details>


ㅤ
## 1. Install `fnm` on Windows

### Using Winget

```powershell
winget install Schniz.fnm
```

---

### Using Chocolatey

```powershell
choco install fnm -y
```

> If Chocolatey isn’t installed yet, you can install it from https://chocolatey.org/install

---

### Using Scoop

```powershell
scoop install fnm
```

> If Scoop isn’t installed yet, you can install it from https://scoop.sh

---

### Using Cargo (if you have Rust)

```powershell
cargo install fnm
```

---

### Manual download (from GitHub)

1. Download the latest release for Windows ([fnm-windows.zip](https://github.com/Schniz/fnm/releases/latest/download/fnm-windows.zip))

2. Extract the file and place the `fnm.exe` in a folder, e.g.:

   ```
   C:\Program Files\fnm\
   ```

3. Add the binary folder to your [system `PATH` environment variable](https://stackoverflow.com/a/79361333).

---

### Verify installation

Open PowerShell or CMD, run:

```sh
fnm --version
```

You should see the installed version number.


ㅤ
## 2. Set Up Shell Integration

### PowerShell

1. Enable script execution, open PowerShell as **Administrator** and run:

   ```powershell
   Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
   ```

   > Learn more about Set-ExecutionPolicy [here](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy).

2. Create or edit your PowerShell profile, run:

   ```powershell
   notepad $PROFILE
   ```

   Add the following line to the end of the file:

   ```powershell
   fnm env --use-on-cd --shell powershell | Out-String | Invoke-Expression
   ```

3. Close and reopen PowerShell to apply the changes.

> [!NOTE]  
> To find the location of your PowerShell `$PROFILE`, run this command:
> ```powershell
> $PROFILE
> ```

---

### CMD

1. Create a `.cmd` profile script, open CMD and run:

   ```cmd
   echo @echo off> "%AppData%\fnm\fnm_autorun.cmd"
   echo :: for /F will launch a new instance of cmd so we create a guard to prevent an infnite loop>> "%AppData%\fnm\fnm_autorun.cmd"
   echo if not defined FNM_AUTORUN_GUARD (>> "%AppData%\fnm\fnm_autorun.cmd"
   echo     set "FNM_AUTORUN_GUARD=AutorunGuard">> "%AppData%\fnm\fnm_autorun.cmd"
   echo     FOR /f "tokens=*" %%z IN ('fnm env --use-on-cd') DO CALL %%z>> "%AppData%\fnm\fnm_autorun.cmd"
   echo )>> "%AppData%\fnm\fnm_autorun.cmd"

   ```

2. Hook it into CMD **AutoRun** using this command:

   ```cmd
   reg add "HKCU\Software\Microsoft\Command Processor" /v AutoRun /t REG_SZ /d "%AppData%\fnm\fnm_autorun.cmd" /f

   ```

   This ensures the script runs every time CMD starts.

3. Close and reopen CMD to apply the changes.

> [!WARNING]  
> Make sure `fnm_autorun.cmd` is accessible to all users and doesn’t require elevated permissions to execute.

---

### Git Bash ([Git for Windows](https://gitforwindows.org/))

1. Create or edit a `.bashrc` file, open Git Bash and run:

   ```bash
   nano ~/.bashrc
   ```

2. Add the **fnm line** at the bottom, scroll to the end and insert:

   ```bash
   eval "$(fnm env --use-on-cd --shell bash)"
   ```

3. Save and Exit Nano

   - Press <kbd>Ctrl</kbd> + <kbd>O</kbd> to write the changes
   - Hit <kbd>Enter</kbd> to confirm
   - Press <kbd>Ctrl</kbd> + <kbd>X</kbd> to exit

4. Close and reopen Git Bash to apply the changes.


ㅤ
## 3. Use `fnm`

### Install Node.js versions

```powershell
fnm install 16
fnm install 20
```

### List installed versions

```powershell
fnm list
```

### Use a specific version (temporarily)

```powershell
fnm use 16

# Use system-installed Node.js, outside of FNM’s control.
fnm use system
```

### Set default version

```powershell
fnm default 20
```

### Check if it’s working

```sh
node -v
# or
fnm current
```

If you see the correct Node.js version, it's working.

ㅤ
## FAQ

### Find Where `fnm` Stores Its Data

1. In PowerShell, run:

   ```powershell
   fnm env
   ```

2. Look for the `FNM_DIR` line in the output:

   ```powershell
   $env:FNM_DIR = "C:\Users\<YourUsername>\AppData\Roaming\fnm"   # or %APPDATA%\fnm
   ```

   This path is where fnm stores:
   * Installed Node.js versions
   * Aliases

3. Find Node.js installation managed by fnm

   ```sh
   where node   # PowerShell or CMD

   # or

   which node   # Git Bash
   ```

---

### Use different version of Node.js for a specific project

1. Navigate to your project directory

   ```sh
   cd path/to/your/project
   ```

2. Install Node.js version locally

   For example, use Node.js `16`:

   ```sh
   fnm install 16
   fnm use 16
   ```

3. Create a `.node-version`, `.nvmrc`, or add Node version in `package.json`

   In your project’s root folder:

   ```sh
   echo 16 > .node-version
   ```
   Or use `.nvmrc` if you prefer:
   ```sh
   echo 16 > .nvmrc
   ```
   Or add the Node version to your project's `package.json` (recommended for many toolchains):
   ```json
   {
     "engines": {
       "node": "16"
     }
   }
   ```


ㅤ
## Uninstall

To completely uninstall fnm (Fast Node Manager), follow [these steps](UNINSTALL.md).
