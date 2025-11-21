# VS Code/VSCodium Git with Multiple Git Accounts

This guide shows how to configure **SSH keys**, manage **multiple accounts** (GitHub, GitLab, Gitea, etc.), and use Git inside VS Code or VSCodium.

ㅤ
## Table of Contents
- [Prerequisites](#prerequisites)
- [Generate SSH keys](#1-generate-ssh-keys)
- [Add public keys](#2-add-public-keys)
- [Configure SSH](#3-configure-ssh)
- [Auto-load SSH keys](#4-auto-load-ssh-keys)
    - [Linux / macOS](#linux--macos)
    - [Windows (PowerShell / VS Code)](#windows-powershell--vs-code)
    - [Windows (Git Bash)](#windows-git-bash)
- [Test SSH connection](#5-test-ssh-connection)
- [Clone repositories in VS Code](#6-clone-repositories-in-vs-code)
- [Per-repo commit identity](#7-per-repo-commit-identity)
- [Enhance with GitLens in VS Code](#8-enhance-with-gitlens-in-vs-code)
- [Changing existing repos](#9-changing-existing-repos)
- [You’re ready!](#%F0%9F%8E%89-youre-ready)

ㅤ
## Prerequisites

Before you begin, make sure you have:

- **VS Code** or **VSCodium** installed  
  [Download VS Code](https://code.visualstudio.com/) | [Download VSCodium](https://vscodium.com/)

- **Git** installed and available in your system PATH
  - **Windows:** install [Git for Windows](https://gitforwindows.org/)
  - **Linux/macOS:** use your package manager or [download Git](https://git-scm.com/downloads)

- **OpenSSH** installed
  - **Linux/macOS:** usually pre-installed
  - **Windows:** use the built-in [OpenSSH](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-overview) (included in Windows 10/11).  
    ⚠️ Don’t use Git Bash’s bundled OpenSSH unless you explicitly choose the session-based option.

- Basic familiarity with:
  - Running commands in **Terminal** (Linux/macOS) or **PowerShell/Git Bash** (Windows)
  - Copy-pasting files/paths

ㅤ
## 1. Generate SSH keys

Each account gets its own key, run these one by one (replace the email with yours):

```bash
# GitHub Personal
ssh-keygen -t ed25519 -C "personal@email.com" -f ~/.ssh/id_github_personal

# GitHub Work
ssh-keygen -t ed25519 -C "work@email.com" -f ~/.ssh/id_github_work

# GitLab
ssh-keygen -t ed25519 -C "gitlab@email.com" -f ~/.ssh/id_gitlab

# Gitea
ssh-keygen -t ed25519 -C "gitea@email.com" -f ~/.ssh/id_gitea

# Bitbucket
ssh-keygen -t ed25519 -C "bitbucket@email.com" -f ~/.ssh/id_bitbucket

# sourcehut
ssh-keygen -t ed25519 -C "sourcehut@email.com" -f ~/.ssh/id_sourcehut
```

* Press <kbd>Enter</kbd> for default path.
* Enter a **passphrase** (adds extra security), or press <kbd>Enter</kbd> for no passphrase.

ㅤ
## 2. Add public keys

Show your public key file (`.pub`) and copy it:

```bash
# GitHub Personal
cat ~/.ssh/id_github_personal.pub

# GitHub Work
cat ~/.ssh/id_github_work.pub

# GitLab
cat ~/.ssh/id_gitlab.pub

# Gitea
cat ~/.ssh/id_gitea.pub

# Bitbucket
cat ~/.ssh/id_bitbucket.pub

# sourcehut
cat ~/.ssh/id_sourcehut.pub
```

Each output starts with `ssh-ed25519 ...`, copy each one separately.


Add to platforms:

* [**GitHub**](https://github.com/settings/keys) → `Settings` → `SSH and GPG keys` → `SSH keys` → `New SSH key`

* [**GitLab**](https://gitlab.com/-/user_settings/ssh_keys) → `Preferences` → `SSH Keys` → `Add new key`

* [**Gitea**](https://gitea.com/user/settings/keys) → `Settings` → `SSH / GPG Keys` → `Manage SSH Keys` → `Add Key`

* [**Bitbucket**](https://bitbucket.org/account/settings/ssh-keys/) → `Personal settings` → `SSH keys` → `Add key`

* [**sourcehut**](https://meta.sr.ht/keys) → `meta` → `keys` → `SSH Keys` → `Add key`

* [**Gitee**](https://gitee.com/profile/sshkeys) → `Settings` → `SSH keys` → `Add key` → `Submit`

ㅤ
## 3. Configure SSH

Edit (or create) `~/.ssh/config` file:

```bash
nano ~/.ssh/config
```

Add entries for each identity:

```ssh
# `UseKeychain` is for macOS only (stores passphrase in Keychain)

# GitHub Personal (alias = github.com-personal → real host = github.com)
Host github.com-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_github_personal
  AddKeysToAgent yes
  IdentitiesOnly yes

# GitHub Work (alias = github.com-work → real host = github.com)
Host github.com-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_github_work
  AddKeysToAgent yes
  IdentitiesOnly yes

# GitLab
Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_gitlab
  AddKeysToAgent yes
  IdentitiesOnly yes

# Gitea
Host gitea.com
  HostName gitea.com
  User git
  IdentityFile ~/.ssh/id_gitea
  AddKeysToAgent yes
  IdentitiesOnly yes

# Bitbucket
Host bitbucket.org
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_bitbucket
  AddKeysToAgent yes
  IdentitiesOnly yes

# sourcehut
Host git.sr.ht
  HostName git.sr.ht
  User git
  IdentityFile ~/.ssh/id_sourcehut
  AddKeysToAgent yes
  IdentitiesOnly yes
```

Save and Exit Nano

- Press <kbd>Ctrl</kbd> + <kbd>O</kbd> to write the changes
- Hit <kbd>Enter</kbd> to confirm
- Press <kbd>Ctrl</kbd> + <kbd>X</kbd> to exit

ㅤ
> [!NOTE]  
> `AddKeysToAgent yes` tells SSH to add the key to the agent the first time it’s used.
>
> On macOS, use `UseKeychain yes` to stores your passphrase in the system keychain, so you don’t need to re‑enter it after reboot.

ㅤ

The `Host` alias (like `github.com-personal` or `github.com-work`) is what you use in Git URLs.  
It maps to the real domain (`github.com`) but forces SSH to pick the right key for that identity.

Example:
- `git@github.com-personal:username/repo.git` → uses **`id_github_personal`**
- `git@github.com-work:org/repo.git` → uses **`id_github_work`**

ㅤ
## 4. Auto-load SSH keys

### Linux / macOS

On Linux, most desktop environments already start `ssh-agent`.

If not, add this to `~/.bashrc` or `~/.zshrc`:

```bash
eval "$(ssh-agent -s)" >/dev/null
```

You can confirm which keys are loaded with:

```bash
ssh-add -l
```

This lists all keys currently loaded in the agent.

ㅤ
### Windows (PowerShell / VS Code)

Enable the built-in `ssh-agent` service:

- Press Start, type `PowerShell`
- Right-click → **Run as administrator**
- Enable & start the service, run:

  ```powershell
  Set-Service -Name ssh-agent -StartupType Automatic
  Start-Service ssh-agent
  Get-Service ssh-agent
  ```

Windows `ssh-agent` will remember keys across reboots.

If you use passphrases, they’ll be cached until logout.

ㅤ
### Windows (Git Bash)

Git Bash includes its own OpenSSH (separate from Windows). You have two options:

#### Option 1 — Use Windows’ OpenSSH (recommended)

1. Ensure Windows `ssh-agent` is enabled (see above).

2. Make Git Bash use Windows’ OpenSSH instead of its bundled one.  
   Add this to your `~/.bashrc` or `~/.bash_profile`:

   ```bash
   export PATH="/c/Windows/System32/OpenSSH:$PATH"
   ```

3. Restart Git Bash and check:

   ```bash
   which ssh
   ```

   It should return: `/c/Windows/System32/OpenSSH/ssh`.

Now Git Bash, PowerShell, and VS Code all share the same persistent agent and keys.

#### Option 2 — Use Git Bash’s own agent (session-based)

1. Start an agent each session:

   ```bash
   eval "$(ssh-agent -s)" >/dev/null
   ```

2. Verify with:

   ```bash
   ssh-add -l
   ```

> [!NOTE]  
> Keys will work for that session only. Once you close Git Bash, **you’ll need to repeat the steps**.

ㅤ
## 5. Test SSH connection

Try connecting:

```bash
# GitHub Personal
ssh -T git@github.com-personal

# GitHub Work
ssh -T git@github.com-work

# GitLab
ssh -T git@gitlab.com

# Gitea
ssh -T git@gitea.com

# Bitbucket
ssh -T git@bitbucket.org

# sourcehut
ssh -T git@git.sr.ht
```

If it works, you’ll see a welcome/success message (or a note that shell access is disabled — that’s normal).

<!-- ### GitHub Personal/Work

```bash
ssh -T git@github.com-personal
ssh -T git@github.com-work

# Expected output: Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

### GitLab

```bash
ssh -T git@gitlab.com

# Expected output: Welcome to GitLab, @<username>!
```

### Gitea

```bash
ssh -T git@gitea.com

# Expected output: Hi there, <username>! You've successfully authenticated with the key named DESKTOP-I0EGD3M, but Gitea does not provide shell access.
```

### Bitbucket

```bash
ssh -T git@bitbucket.org

# Expected output: You can use git to connect to Bitbucket. Shell access is disabled
```

### sourcehut

```bash
ssh -T git@git.sr.ht

# Expected output: Hi <username>! You've successfully authenticated, but I do not provide an interactive shell. Bye!
``` -->

ㅤ
> [!NOTE]  
> When you connect to a new SSH host (GitHub, GitLab, Gitea, etc.) for the first time, you’ll see:
>
> ```
> The authenticity of host '<Host> (IP)' can't be established.
> ECDSA key fingerprint is SHA256:...
> Are you sure you want to continue connecting (yes/no/[fingerprint])?
> ```
>
> Type `yes`.
>
> This adds the host fingerprint to `~/.ssh/known_hosts` so you won’t be asked again.

ㅤ
## 6. Clone repositories in VS Code

1. In VS Code:

   * `Ctrl+Shift+P` → `Git: Clone` → paste SSH URL → pick folder.

2. Copy the repo’s SSH URL:

   ```bash
   # GitHub Personal → maps to github.com with "personal" key
   git clone git@github.com-personal:username/repo.git

   # GitHub Work → maps to github.com with "work" key
   git clone git@github.com-work:org/repo.git

   # GitLab
   git clone git@gitlab.com:group/repo.git

   # Gitea
   git clone git@gitea.com:user/repo.git

   # Bitbucket
   git clone git@bitbucket.org:workspace/repo.git

   # sourcehut
   git clone git@git.sr.ht:~username/repo
   ```

> [!IMPORTANT]  
> Make sure the remote is `git@...`, not `https://`.
>
> **sourcehut** requires the tilde (`~username`) in the URL.

ㅤ
## 7. Per-repo commit identity

Each repo should use the correct identity:

```bash
git config user.name "Your Name"
git config user.email "your@email.com"
```

> [!TIP]  
> * **SSH keys** decide which account you push to.
> * **user.name/user.email** decide who shows up in the commit history.
> * Always set both correctly for each repo.

ㅤ
## 8. Enhance with GitLens in VS Code

Install [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) for:

* Blame annotations
* History per line/file
* Branch comparison
* Repo explorer

Works with **all Git servers**.

ㅤ
## 9. Changing existing repos

To switch an HTTPS or plain `github.com` repo to your alias, example:

```bash
# Switch HTTPS → SSH
git remote set-url origin git@github.com-work:org/repo.git

# Verify
git remote -v
```

ㅤ

## 🎉 You’re ready!

With this setup:

* One machine → use multiple different SSH keys per account/host.
* No account conflicts.
* Auto-load SSH keys on startup.
* Have different commit identities per repo.
* VS Code / VSCodium handles push/pull seamlessly.

Daily use in VS Code:

* **Source Control Panel** → stage, commit, push, pull.
* **GitLens** → view history, blame, and branch comparisons.
