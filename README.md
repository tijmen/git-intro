# Introduction to Git
 - Last updated: 12 March 2024
 - Author: Tijmen de Haan

## Distributed Version Control

In contrast to CVS or SVN, each Git repository is created equal. Repositories all contain the entire history, and there are no client/server designations built into Git. You may choose to use Git in any topology you'd like, though a traditional central server is still very common. These days we often choose this central server to be a Git hosting platform like GitHub. 

An advantage of distributed version control is that everyone's copy serves as a complete backup. As an example, in 2011 kernel.org went down due to a security breach. The distributed nature of Git allowed development of the Linux kernel to continue relatively unaffected.

## Setting up Your Git

1. Introduce yourself to Git:
   ```
   git config --global user.name "Your Name"
   git config --global user.email your@email.com
   ```

2. Enable color output:
   ```
   git config --global color.ui auto
   ```

3. Choose your default text editor:
   ```
   git config --global core.editor nano
   ```

4. Set default push behavior:
   ```
   git config --global push.default current
   ```

## Creating a Repository
1. Create a new repository:
   ```
   git init
   ```

2. Create a bare repository:
   ```
   git init --bare
   ```

## Four Areas
1. Working tree: files and folders on your filesystem except the .git folder
2. Staging area: files added with `git add` are staged for commit
3. Repository: each commit is a snapshot of the entire tree
4. Remotes: other repositories (often this will be GitHub)

## Branches

1. Create a new branch:
   ```
   git branch feature
   ```

2. List branches:
   ```
   git branch
   git branch -a
   ```

3. Switch branches:
   ```
   git checkout main
   git checkout feature
   ```

4. Merge changes and delete branch:
   ```
   git checkout main
   git merge feature
   git branch -d feature
   ```

5. Discard branch changes:
   ```
   git checkout main
   git branch -D feature
   ```

6. Cherry-pick a specific commit:
   ```
   git cherry-pick a43b72f
   ```

## Remotes

These days it's very common to use GitHub to manage remote repositories.

1. Add a remote repository:
   ```
   git remote add origin https://github.com/username/repo.git
   ```

2. List remote repositories:
   ```
   git remote -v
   ```

3. Fetch changes from a remote repository:
   ```
   git fetch origin
   ```

4. Pull changes from a remote repository:
   ```
   git pull origin main
   ```

5. Push changes to a remote repository:
   ```
   git push origin main
   ```

## Commit Messages

Provide descriptive commit messages. If you ever need to use a shared account (such as the southpoletelescope user that runs SPT), sign commits by hand e.g. "TdH: Added a git tutorial."

**Good commit message example:**
```
Add parallel processing feature with multiprocessing.Pool. Checks available number of CPUs N and chooses min(N,8) threads. Unit tests passed. 
```

**Bad commit message example:**
```
Fixed bugs
```

## Browsing Your Repository

1. View global history:
   ```
   git log
   ```

2. View history for a specific file:
   ```
   git log -- path/to/file
   ```

3. Compare versions:
   ```
   git diff 51ad6f..23eba2 -- path/to/file
   ```

4. Compare recent commits:
   ```
   git diff HEAD^^..HEAD
   ```

5. Compare working tree and staging area:
   ```
   git diff
   ```

## Stash

1. Store untracked changes:
   ```
   git stash
   ```

2. Re-apply stashed changes:
   ```
   git stash pop
   ```

3. If you're not sure what's in the stash, try:
    ```
    git stash list
    ```

## Pull with --rebase

When working in a configuration where there is one central server that all users pull from and push to, it can happen that you go to push only to find out that there were some trivial changes made in some other part of the repository. In this case, `git pull` will create a merge, which will be a separate commit that clutters up the history. However, in these cases it is better to `git pull --rebase` to reapply your recent commit on top of the current state of the remote. 

## Undo Changes

1. Reset working tree to the current repository state:
   ```
   git reset --hard HEAD
   ```

2. Reset local repository to an earlier commit:
   ```
   git reset --hard 154bf2
   ```

3. Undo the latest commit:
   ```
   git reset --hard HEAD^
   ```

4. Interactive rebasing (avoid rewriting pushed history):
   ```
   git rebase -i
   ```

5. Use `git revert` to create a new commit that undoes the changes made in a previous commit without rewriting history:
   ```
   git revert a43b72f
   ```

## Resolving Merge Conflicts

When merging branches or pulling changes from a remote repository, you may encounter merge conflicts if the same lines of code have been modified in different ways. To resolve merge conflicts:

1. Open the conflicting files and look for the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).

2. Manually edit the files to resolve the conflicts by choosing the desired changes and removing the conflict markers.

3. Stage the resolved files using `git add`.

4. Commit the changes to complete the merge:
   ```
   git commit -m "Resolve merge conflicts"
   ```

## Git Hosting Platforms

Git hosting platforms provide a cloud-based remote. The most popular ones are:

1. GitHub
   - By far the most popular at the moment. 
   - Provides a large community and a vast ecosystem of integrations and tools.

2. GitLab
   - Offers a complete DevOps platform with built-in CI/CD, issue tracking, and project management tools.
   - Provides both cloud-hosted and self-hosted options.

3. Bitbucket
   - Integrates well with Atlassian's suite of tools like Jira and Confluence.
   - Offers free private repositories for small teams.

These platforms have built-in issue trackers, wikis, discussion sections, access control, etc.

Pull requests are a *particularly* useful feature, allowing you to propose changes to a repository and assign reviewers to that change. When the reviewers approve, they can merge your changes into the main branch.

## Git Workflows

Git workflows provide guidelines for structuring your branching and merging strategies. Two common workflows are:

1. Gitflow Workflow
   - Consists of two main branches: `main` (previously `master`) and `develop`.
   - Feature branches are created from `develop` and merged back into `develop` when completed.
   - Release branches are created from `develop`, tested, and merged into `main` for production releases.
   - Hotfix branches are created from `main` for critical bug fixes and merged back into both `main` and `develop`.

2. Feature Branch Workflow
   - Each new feature or bug fix is developed in a separate branch.
   - Feature branches are created from the main branch (e.g., `main`) and merged back via pull requests.
   - This workflow is simpler and more flexible than Gitflow, making it suitable for smaller teams or projects.

Choose a workflow that aligns with your needs.

## Git Best Practices

1. Write clear and concise commit messages that describe the changes made in each commit.

2. As much as possible, keep commits focused on a single logical change to maintain a clean and readable history.

3. Regularly pull changes from remote repositories to stay up to date and avoid merge conflicts. Remember to use `git pull --rebase` if appropriate.

4. Use descriptive branch names that reflect the purpose of the branch (e.g., `feature/multiprocessing`, `bugfix/indexing-error`).

5. Avoid pushing unfinished or untested code to the main branch.

6. Perform code reviews and use pull requests to ensure code quality and collaboration.

7. Use tags to mark important milestones or releases in your project.

## Advanced Git Features

1. `git bisect`: Helps you find the commit that introduced a bug by performing a binary search through the commit history.
   ```
   git bisect start
   git bisect bad 23eba2
   git bisect good 51ad6f
   ```

2. `git reflog`: Allows you to view and recover lost commits or branch references.
   - `git reflog` shows the history of all actions performed in the repository, including commits, branch switches, and resets.
   - A "lost commit" refers to a commit that is no longer reachable from any branch or tag, often due to a reset or branch deletion. Similarly, a "lost branch reference" refers to a branch that has been deleted or overwritten.
   - To recover a lost commit or branch, find the desired commit in the reflog output and use `git checkout` to switch to it:
     ```
     git reflog
     git checkout 154bf2
     ```

3. `git blame`: Shows the author and commit information for each line of a file.
   ```
   git blame path/to/file
   ```

## Visualizations

GUI tools like GitKraken, GitHub Desktop, gitg, gitk, or SourceTree are useful for visualizing repository history and changes.

## Git Hooks

Git hooks are scripts that Git executes before or after events such as: commit, push, and receive. They allow you to customize Git's internal behavior and trigger customizable actions at key points in your work flow. 

Here are some examples of git hooks you could consider:
 - **pre-commit hook:** code linter to check for code quality and style
 - **pre-commit hook:** commit message check for style or required information
 - **pre-commit hook:** security check to look for API keys or other sensitive information
 - **pre-push hook:** run unit tests to make sure the code is functional
 - **post-commit hook:** generate a documentation or version number
 - **pre-rebase hook:** make sure the latest changes from the remote have been pulled in
 - etc.

## Tagging

Tags are used to capture a point in history that is used for a e.g. a version release.

1. Create a tag:
   ```
   git tag -a v1.0 -m "Version 1.0"
   ```

2. Push a tag to remote:
   ```
   git push origin v1.0
   ```

3. List all tags:
   ```
   git tag
   ```

## Aliases

Aliases are used to create shorter commands that map to longer commands.

1. Create an alias:
   ```
   git config --global alias.co checkout
   git config --global alias.br branch
   git config --global alias.ci commit
   git config --global alias.st status
   ```

2. Use an alias:
   ```
   git co main
   ```

3. Create aliases for complex commands or sequences of commands using shell scripts or Git's `!` syntax:
   ```
   git config --global alias.release '!./release.sh'
   ```

## Submodules

Submodules allow you to keep a Git repository as a subdirectory of another Git repository. This lets you clone another repository into your project and keep your commits separate.

1. Add a submodule:
   ```
   git submodule add https://github.com/username/repo.git
   ```

2. Update submodules:
   ```
   git submodule update --remote repo
   ```

There is also `git subtree` which is similar but integrates the external module more tightly with the main repo. `git submodule` requires the developer to be aware of the submodule and manually keep it up to date, while `git subtree` is less visible to the developer. 

## Recommended Resources

- Git Cheat Sheet: [https://training.github.com/downloads/github-git-cheat-sheet.pdf](https://training.github.com/downloads/github-git-cheat-sheet.pdf)
- Atlassian Git Tutorial: [https://www.atlassian.com/git/tutorials](https://www.atlassian.com/git/tutorials)
- (humor) Git man page generator: https://git-man-page-generator.lokaltog.net/

## Conclusion

I hope I have covered the basics of using Git for version control. Feel free to refer to this document as a cheat sheet and let me know at <tijmen.dehaan@gmail.com> if you have any suggestions for improvement.
