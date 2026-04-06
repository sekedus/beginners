# Git tutorial for beginners

## Table of Contents

- [Create a new repo on github](#create-a-new-repo-on-github)
- [Upload local content to a remote repository](#upload-local-content-to-a-remote-repository)
- [Overwrite a commit after pushed to remote](#overwrite-a-commit-after-pushed-to-remote)
- [Change the commit author for single/last commit](#change-the-commit-author-for-singlelast-commit)
- [Discard commit](#discard-commit)
- [Comparing/diff commits](#comparingdiff-commits)
- [Delete git tag local and remote](#delete-git-tag-local-and-remote)
- [Changing a GitHub repository's visibility](#changing-a-github-repositorys-visibility)
- [Delete inactive deployments](#delete-inactive-deployments)
- [Managing remote Git repositories](#managing-remote-git-repositories)
- [Manage Multiple GitHub (gh) CLI Accounts](#manage-multiple-github-gh-cli-accounts)

<br />

## Create a new repo on github

Related: [gh_repo_create](https://cli.github.com/manual/gh_repo_create#examples)

<details>
<summary>show/hide</summary>
<br />

1. Login to github: `gh auth login`
2. Set global options (name & email):

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your_email@example.com"
   ```
3. Create new repo & clone: `gh repo create`
4. Initialize a git repository: `git init`
5. Set default branch name: `git branch -M main`
6. Add a remote repository:

   ```bash
   git remote add origin https://github.com/<your_username>/<your-repo>.git
   ```
</details>
<br />

## Upload local content to a remote repository

<details>
<summary>show/hide</summary>
<br />

1. Include updates

   ```bash
   git add .
   ```
2. [Commit](https://github.com/git-guides/git-commit) changes

   ```bash
   git commit -m "commit message"
   ```
3. Upload to remote repository

   ```bash
   git push

   # first commit use:
   git push -u origin main
   ```
</details>
<br />

## Overwrite a commit after pushed to remote

Source: https://stackoverflow.com/a/35543613

<details>
<summary>show/hide</summary>
<br />

1. Include updates (optional)

   ```bash
   git add .
   ```
2. Modify old commit

   ```bash
   git commit --amend -m "commit message"
   ```
3. Overwrite history on the Github remote

   ```bash
   git push -f
   ```
</details>
<br />

## Change the commit author for single/last commit

Related: https://stackoverflow.com/a/43231587

<details>
<summary>show/hide</summary>
<br />

1. Modify old commit

   ```bash
   # with new message:
   git commit --amend --author="New Author Name <new.author@example.com>" -m "new commit message"

   # without message:
   git commit --amend --author="New Author Name <new.author@example.com>" --no-edit
   ```
2. Overwrite history on the Github remote

   ```bash
   git push -f

   # or (Force push your changes without overwriting anyone else's commits)

   git push --force-with-lease
   ```
</details>
<br />

## Discard commit

Source: https://stackoverflow.com/a/48732358

<details>
<summary>show/hide</summary>
<br />

- Delete the most recent local commit, **without discard local files changes**:
   ```bash
   git reset --soft HEAD~1
   ```

- Delete commits and **discard/delete all changes in your working tree (local files & untracked files or directories)**:
   ```bash
   git reset --hard origin/<branch>

   # or

   git reset --hard HEAD~5 # reset current branch to 5 commits ago

   # or

   git reset --hard <commit_sha_hash>
   ```
</details>
<br />

## Comparing/diff commits

Source: [Comparing commits](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/viewing-and-comparing-commits/comparing-commits)

<details>
<summary>show/hide</summary>
<br />

Compares two arbitrary commits, using commit SHA-1 hash.

Example:
```
https://github.com/sekedus/tamp/compare/3184ad9db2c6de28794a2ee1bab91fb805f9d826..b431a20697524f9d3a332b916748457934fdc710
```

or use the shortened SHA code
```
https://github.com/sekedus/tamp/compare/3184ad9..b431a20
```
</details>
<br />

## Delete git tag local and remote

Source: https://gist.github.com/mobilemind/7883996

<details>
<summary>show/hide</summary>
<br />

```
# delete local tag '12345'
git tag -d 12345

# delete remote tag '12345' (eg, GitHub version too)
git push origin :refs/tags/12345

# alternative approach
git push --delete origin tagName
```
</details>
<br />

## Changing a GitHub repository's visibility

[GitHub Docs](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/setting-repository-visibility#changing-a-repositorys-visibility)

<br />

## Delete inactive deployments

[community/discussions#85000 (comment)](https://github.com/orgs/community/discussions/85000#discussioncomment-14245406)

<br />

## Managing remote Git repositories

Source:
- https://stackoverflow.com/a/16330439
- [Managing remote repositories](https://docs.github.com/en/get-started/git-basics/managing-remote-repositories)
- [git-remote](https://git-scm.com/docs/git-remote)

<br />
<details>
<summary>show/hide</summary>
<br />

```
# Set a new remote
git remote add <REMOTE_NAME> <GIT_URL>

# Changing a remote URL
git remote set-url <REMOTE_NAME> <GIT_URL>

# Remove remote
git remote remove <REMOTE_NAME>

# List remote
git remote -v
```
</details>
<br />

## Manage Multiple GitHub (gh) CLI Accounts

Related:
- [GitHub Blog](https://stackoverflow.com/a/16330439)
- [GitHub CLI manual](https://cli.github.com/manual/gh_auth)

<br />
<details>
<summary>show/hide</summary>
<br />

1. Add Multiple Accounts
   ```bash
   gh auth login
   ```
   > Repeat the command for your other account(s).  
   > Each login adds a new account without overwriting the previous one.

2. Check Current Accounts
   ```bash
   gh auth status
   ```
   > This shows all accounts currently logged in.  
   > The **active account** is marked, meaning it’s the one used for GitHub operations.

3. Switch Between Accounts
   ```bash
   gh auth switch

   # or

   gh auth switch --user <username>
   ```

4. Remove an Account
   ```bash
   gh auth logout
   ```
</details>
<br />
