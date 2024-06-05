**Git tutorial for beginners**

<details>
<summary>Create a new repo on github</summary>
ㅤ

[related](https://cli.github.com/manual/gh_repo_create#examples)

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

ㅤ
</details>

<details>
<summary>Upload local content to a remote repository</summary>
ㅤ

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

ㅤ
</details>

<details>
<summary>Overwrite a commit after pushed to remote</summary>
ㅤ

[source](https://cli.github.com/manual/gh_repo_create#examples)

1. Include updates

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

ㅤ
</details>

<details>
<summary>Change the commit author for single/last commit</summary>
ㅤ

[related](https://stackoverflow.com/a/43231587)

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

ㅤ
</details>

<details>
<summary>Discard commit</summary>
ㅤ

[source](https://stackoverflow.com/a/48732358)

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

ㅤ
</details>

<details>
<summary>Comparing/diff commits</summary>
ㅤ

[source](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/viewing-and-comparing-commits/comparing-commits)

Compares two arbitrary commits, using commit SHA-1 hash.

Example:
```
https://github.com/sekedus/tamp/compare/3184ad9db2c6de28794a2ee1bab91fb805f9d826..b431a20697524f9d3a332b916748457934fdc710
```

or use the shortened SHA code
```
https://github.com/sekedus/tamp/compare/3184ad9..b431a20
```

ㅤ
</details>

<details>
<summary>Delete git tag local and remote</summary>
ㅤ

[source](https://gist.github.com/mobilemind/7883996)

```
# delete local tag '12345'
git tag -d 12345

# delete remote tag '12345' (eg, GitHub version too)
git push origin :refs/tags/12345

# alternative approach
git push --delete origin tagName
```

ㅤ
</details>

<details>
<summary>Changing a GitHub repository's visibility</summary>
ㅤ

[GitHub Docs](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/setting-repository-visibility#changing-a-repositorys-visibility)

ㅤ
</details>

<details>
<summary>Delete inactive deployments</summary>
ㅤ

[community/discussions#85000 (comment)](https://github.com/orgs/community/discussions/85000#discussioncomment-14245406)

ㅤ
</details>

<details>
<summary>Managing remote Git repositories</summary>
ㅤ

[source](https://stackoverflow.com/a/16330439), [source2](https://docs.github.com/en/get-started/git-basics/managing-remote-repositories), [source3](https://git-scm.com/docs/git-remote)

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

ㅤ
</details>

<details>
<summary>Manage Multiple GitHub (gh) CLI Accounts</summary>
ㅤ

[GitHub Blog](https://stackoverflow.com/a/16330439), [GitHub CLI manual](https://cli.github.com/manual/gh_auth)

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

ㅤ
</details>
