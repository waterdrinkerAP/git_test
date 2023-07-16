# Git

Git is a distributed version control system. It saves a history of our files both locally
and remotely.
Git works with folders called repositories. Every time we make changes to the files in
our repositories, we need to stage those changes and then commit them.
Git can log the changes between different commits and move in time between commits.
Add meaningful messages to all commits.

## Topics

- [Git Initial Setup](#git-initial-setup)
- [Git Basics](#git-basics)
- [Git Commit Message Rules](#git-commit-message-rules)
- [Git Branching and Merging](#git-branching-and-merging)


# Git Initial Setup

To set up Git for use with GitHub we first need to configure the following settings:

```shell
git config --global user.name "Your name"
```

```shell
git config --global user.email "Your@email.com"
```

```shell
git config --global init.defaultBranch main
```

```shell
git config --global color.ui auto
```

We can check what we have done with:

```shell
git config --get user.name
```

```shell
git config --get user.email
```

```shell
git config --list --show-origin
```

To easily push code to GitHub we need to generate an `SSH` key:

```shell
ssh-keygen -t ed25519 -C <your email>
```

In our GitHub account's `Settings > SSH and GPG keys` add the `New SSH Key`:

```shell
cat ~/.ssh/id_ed25519.pub
```

To test if we have successfully linked Git to our GitHub:

```shell
ssh -T git@github.com
```

If the following is shown, just type `yes`. We can check the [fingerprint](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints) if we like.

```
The authenticity of host 'github.com (IP ADDRESS)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)?
```

To set VS Code as our editor for commit messages type:

```shell
git config --global core.editor "code --wait"
```

For additional information check:

```shell
git help
```

```shell
man git
```

Or: https://git-scm.com/doc

To set up tab-completion in PowerShell see:
https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-PowerShell


# Git Basics

It is better to create our repositories on the website and then clone them to our 
local machine with:

```shell
git clone <SSH URL>
```

If we want to initialize an empty local repository - `cd` into the desire folder and type:

```shell
git init
```

This creates the hidden `.git` folder in the current directory.

If we have not changed it in the configuration, the default branch is called `master`.
We can rename it to `main` with:

```shell
git branch -M main
```

GitHub uses `main` for their default branches.

To connect a local repository to a remote one use:

```shell
git remote add origin <url_address>
```

When we  `cd` into the repository folder we can check it's origin by typing:

```shell
git remote -v
```

Once we put files in that repository and start making changes to them, we can check
the status of the repo with:

```shell
git status
```

`git status -s` gives shorter less detailed output that follows the form:

```shell
 M README  # M stands for modified file
MM Makefile  # Left M is status of staging area, right is working tree
A  lib/git.rb # A stands for added to staging area
?? LICENSE.txt # ?? is a new file that is not tracked
```

Mark new and recently changed files for a commit by adding them to the staging area:

```shell
git add <file_name>
```

We don't need to add all the changes made to a file to the staging area.
We can go over the chunks of changes and chose what to add with:

```shell
git add -p <file_name>
```

Remove file from the staging area with:

```shell
git restore --staged <file_name>
```

Revert changes back to the previous commit with:

```shell
git restore <file_name>
```

When adding or restoring, we can replace the file name with `.`, to apply the 
command to all files in the folder.

See exactly what was changed with one of:

```shell
git diff  # This shows only unstaged changes
git diff --staged 
```

Finalize changes by committing all files in the staging area, and adding a message
for the new commit. Use imperative mood in messages and don't end them with a
punctuation mark. First line of the message should be no longer than 50 characters.
Check [Git Commit Message Rules](#git-commit-message-rules) for more information.

```shell
git commit -m "One line message here"
```

To set VS Code as the editor for commit messages type:

```shell
git config --global core.editor "code --wait"
```

Skip the staging are and add or remove all files known to git by using:

```shell
git commit -a
```

To make changes to last commit use:

```shell
git commit --amend
```

The current staging area is used to replace last commit. If no changes were made,
only the commit message will be changed.

See a list of last 2 commits and the changes they introduced with:

```shell
git log -p -2
```

To get a single line summary of each commit use one of:

```shell
git log --oneline
# Or
git log --pretty=oneline
```

For more information on `--pretty` and its values check the [book](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History).

When removing or renaming files it is faster to use git commands:

```shell
git rm  # Remove
git mv  # Move/Rename
```

This will skip having to add the file to the staging area. However if the files are
already in the staging area, add `-f` to force the change.

To go back in time one commit, use:

```shell
git reset main^
```

For moving back more than one commit, type:

```shell
git reset main~<number_of_steps>
```

To upload local changes to a remote repository use:

```shell
git push
```

If the repository was created locally you need to set the upstream first with:

```shell
git push -u origin main
```

Check for changes on the remote server with:

```shell
git fetch
```

Pull files from and merge with the remote repository using:

```shell
git pull
```

To get a list of remotes use:

```shell
git remote
```

To get more information for a particular remote use:

```shell
git remote show <name>
```

To rename or remove a remote connection use one of:

```shell
git remote rename <name>
git remote remove <name>
```

To add a remote use:

```shell
git remote add <name> <URL>
```


# Git Commit Message Rules

1. Separate subject from body with a blank line
2. Limit the subject line to 50 characters
3. Capitalize the subject line
4. Do not end the subject line with a period
5. Use the imperative mood in the subject line
6. Wrap the body at 72 characters
7. Use the body to explain what and why vs. how

Original article:
https://cbea.ms/git-commit/

# Git Branching and Merging

Every commit in Git contains data about itself (size, author, committer, message) and two
pointers. A pointer to a snapshot of the content that was staged at the time of the commit
and a pointer to the previous commit. Branches are just named pointers to a commit.
When we first create a repository there is only one branch called `main` that points to
the latest commit.  We can create a new branch by using:

```shell
git branch <name_of_branch>
```

```
　　　　　　　　　　　　　　　　　　HEAD
                                ↓
                  new_branch  main
                           ↘   ↙
commit 1 ←-- commit 2 ←-- commit 3
  ↓            ↓            ↓
snapshot A ← snapshot B ← snapshot C
```

Another pointer called `HEAD` points to the currently active branch. To move the head use:

```shell
git checkout <destination> # use on branches and commits
git switch <destination> # use only on branches
```

List available branches, marking current one, with:

```shell
git branch
```

Create a branch and move `HEAD` at the same time with one of these commands:

```shell
git checkout -b <name_of_branch>
git switch -c <name_of_branch>
```

Move `HEAD` to the previously selected branch with one of:

```shell
git checkout -
git switch -
```

See information about all branches and how they diverge with:

```shell
git log --graph --all
```


`HEAD` can point to a branch or to a specific commit. When `HEAD` is not pointing to a
branch we are in a state of "detached HEAD". When detaching the head we need to use the
`checkout` command, the `switch` command works only with branches. To make `HEAD`
point to a specific commit we need the first 7 characters of a commit's SHA. For example:

```shell
git checkout 9b8eb90
```

To push a new branch to a remote Git repository first `checkout` the branch and then use:

```shell
git push -u origin <name_of_branch>
```

To merge two branches first `checkout` the branch you want to merge into and then call the
`merge` command:

```shell
git checkout main
git merge new_branch
```

If the commit at `new_branch` is a direct descendent of the one `main` is pointing at,
then `main` will simply be fast-forwarded to point to the new commit.

Before merge:

```
                       main     new_branch
                        ↓          ↓
commit 1 ← commit 2 ← commit 3 ← commit 4
```
After merge:

```
                        new_branch   main
                                 ↘   ↙
commit 1 ← commit 2 ← commit 3 ← commit 4
```

If the two branches have diverged at a previous point, Git will still try to merge them.

```
                       main   
                        ↓
commit 1 ← commit 2 ← commit 4
                    ↖  
                      commit 3
                        ↑
                     new_branch
```

Merges of this type create a new "merge commit" with two parents and move the pointer
of the checked out branch to this new commit.

```
                                  main   
                                   ↓
commit 1 ← commit 2 ← commit 4 ← commit 5
                    ↖          ↙
                      commit 3
                        ↑
                     new_branch
```

However if the two branches introduce different changes to the same files, there might
be conflicts. After Git detects the conflicts, all we have to do is open the files in any
editor and chose how to deal with them.

Changes will look like this:

```
<<<<<<< HEAD
Changes from the currently checked out branch.
=======
Changes from the branch we are trying to merge into the current one.
>>>>>>> new_branch
```

We can chose to keep changes from the current branch, from the new branch or to keep both.
We need to make our edits and remove the special characters `<<<<<<<`, `=======` and `>>>>>>>`.
After we handle the conflicts, we need to `add` the files to the staging area and `commit`.

It is a good idea to check the `status` of the repository often. When a `merge` has failed
due to conflicts and we call `status`, Git tells us that we are in the middle of a merge.
If we don't want to deal with the conflicts, we can undo the merge with:

```shell
git merge --abort
```

We can see a history of how our branches have diverged and merged with:

```shell
git log --oneline --graph --all
```

A sample output from that command would be:

```
*   ff216e6 (HEAD -> main, origin/main, origin/HEAD) Merge branch 'office'
|\
| * 2f164a5 (origin/office, office) Edit hello_world.txt from Office
* |   f8a02cf Merge branch 'test'
|\ \
| * | a952772 (test) Edit hello_world.txt to test branching
| |/
* / 856350d Add a question to hello_world.txt
|/
* b77c90c Edit hello_world.txt from Linux
* 9b8eb90 Edit README.md
* 79b53ee Edit README.md and hello_world.txt
```

Each `*` on the graph represents a commit.
Branch names and remote origins are shown in parentheses.

To see the last commit on each branch run:

```shell
git branch -v
```

To see branches that are already merged into the current one type:

```shell
git branch --merged
```

To delete a branch that has been merged and is no longer needed first make sure it's not
checked out, then do:

```shell
git branch -d <name_of_branch>
```

Git prevents deletion of branches that are not fully merged in. To force the deletion use:

```shell
git branch -D <name_of_branch>
```

It is not a good idea to rename branches that were pushed to remote.
To rename a local branch and push it use:

```shell
git branch --move <old_branch_name> <new_branch_name>
git push --set-upstream origin <new_branch_name>
```

To delete the old branch from remote use:

```shell
git push origin --delete <old_branch_name>
```

Local and remote branches can differ in many ways. Git keeps a reference to both local and
remote branches. To see a list of all branches use:

```shell
git branch --all
```

To checkout a branch that exists only on remote but not locally use one of the following:

```shell
git checkout -b <name_of_local_branch> origin/<name_of_remote_branch>
git checkout --track origin/<name_of_remote_branch>
git checkout <name_of_remote_branch> # must be identical with the remote branch name
```

The first option allows us to have a different name for our local version of the branch.

To see a list of branches with their remotes and their last commits type:

```shell
git branch -vv
```

From the output of this command you can also see what local branches don't have a remote
counterpart and by how many commits local and remote branches differ.
Information about remote branches is stored locally and can be out of date. To update use:

```shell
git fetch --all
```

What the `git pull` command actually does is, it fetches all changes from the server and
then tries to merge the remote branch into the local branch of the same name that you have
currently checked out.

Apart from merging we can also rebase branches into other branches. Rebasing allows for
the commit history to look as a straight line. To rebase a branch use  one of the following:

```shell
git rebase main # if you are currently on new_branch
git rebase main new_branch # from any branch
```

This creates a copy of the changes introduced by the `new_branch` and puts them in front
of the last commit of the `main` branch:

```
                       main     new_branch
                        ↓          ↓
commit 1 ← commit 2 ← commit 4 ← commit 3`
                    ↖  
                      commit 3
```

We can then continue the merge with the following two steps:

```shell
git checkout main
git merge new_branch
```

If successful the result will look like this:

```
                                  main
                                   ↓
commit 1 ← commit 2 ← commit 4 ← commit 3`
                    ↖              ↑
                      commit 3  new_branch
```

It is best to use rebasing only on local repositories. Rebasing repositories that other people
are working on can create a lot of confusion and may even make the repository unusable.

For strategies on how to use branching and merging see :
https://youtu.be/Uszj_k0DGsg?t=488