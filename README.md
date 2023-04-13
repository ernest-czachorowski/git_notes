## Useful commands for GitHub.

These are my GitHub notes which I took while taking the GitHub course on Udemy. I believe they are useful, so I am publishing them. If you find any errors, please let me know. Thanks.  

## Stagging area  

Check files in stagging area:  
`git ls-files`  

Remove files from stagging area:  
`git rm <filename>`  
`git reset <filename>`  
`git restore --staged <filename>`  

## Restoring and cleaning data  

Restore file to working directory (tracked files):  
`git checkout <filename>`  
`git restore <finename>`  

Remove files from working directory (untracked files):  
1. `git clean -dn`  
2. `git clean -df`  

Undo one commit (local repository):  
`git reset --soft HEAD~1`  

Undo one commit (local repository + stagging area):  
`git reset HEAD~1`  

Undo one commit (local repository + stagging area + working directory):  
`git reset --hard HEAD~1`  

Override last commit in local repository:  
`git commit --amend`  

## Branches   

Create new branch:  
`git branch <branch_name>`  

Create new branch and switch to that branch:  
`git checkout -b <branch_name>`  
`git switch -c <branch_name>`  

Remove branch (if merged):  
`git branch -d <branch_name>`  

Remove branch:  
`git branch -D <branch_name>`  

Change branch:  
`git checkout <branch_name>`  
`git switch <branch_name>`  

## Working with DETACH HEAD  
Note: If branch with DETACH HEAD will be switched during work it will be removed/hidden. It still can be found in reflog.  

### DETACH HEAD - Method 1  

1. Switch commit to one with DETACH HEAD:  
`git checkout <commit_id>`  
2. Just do your work and commit changes.  
3. Change branch to master and check output. There will be <hidden_branch_id> printed.  
4. Create new branch from hidden branch:  
`git branch <new_branch_name> <hidden_branch_id>`  
5. Switch branch to master:  
`git switch master`  
6. Merge changes:  
`git merge <new_branch_name>`  
7. Remove unused branch  
`git branch -D <new_branch_name>`  

### DETACH HEAD - Method 2  

1. Switch commit to one with DETACH HEAD:  
`git checkout <commit_id>`  
2. Create new branch:  
`git branch <new_branch_name>`  
3. Just do your work and commit changes.  
4. Switch branch to master:  
`git switch master`  
5. Merge changes:  
`git merge <new_branch_name>`  
6. Remove unused branch:  
`git branch -D <new_branch_name>`  

## Working with "stash"  
Changes should be commited (if finished) or added to stash (if experimental) before every branch switch.  
NOTE: Stash will not stash new files.**  

List stash:  
`git stash list`  

Add changes to stash:  
`git stash`  

Add changes to stash with some message:  
`git stash push -m "<message>"`  

Remove selected changes from stash:  
`git stash drop <number>`  

Remove all changes from stash:  
`git stash clear`  

Move selected changes from stash to working area (changes will be removed from stash):  
`git stash pop <number>`  

Copy latest changes from stash to working area:  
`git stash apply`  

Copy selected changes from stash to working area:  
`git stash apply <number>`  

## Reflog - restoring removed commits and branches  
If commit or branch was removed in the period of last 30 days it still can be restored from reflog which exists only in local branch.  

Show reflog history:  
`git reflog`  

Restore removed commit (it will point HEAD on removed commit):  
`git reset --hard <reflog_hash_id>`  

Restore removed branch:  
1. `git checkout <reflog_hash_id>`  
2. `git switch -c <branch_name>`  

## Merging:  

In all below cases you must first swith to branch where you want you changes to be merged to.  

### Fast-forward merge - if the master branch has not been modified since the creation of another branch. No commit is required, git will perform that automatically.  

Fast-forward merge (merging all commits separatley):  
`git merge <branch_name>`  

Fast-forward merge (merging all commits as one):  
`git merge --squash <branch_name>`  

Force git to not perform fast-forward merge (changed must be commited):  
1. `git merge --no-ff <branchname>`  
2. `git add <files>`  
3. `git commit -m "<commit_message>"`  

### Recursive merge - if the master branch has been modified since the creation of another branch. It creates a new commit in master, where then git is moving changes.  

Recursive merge (merging all commits separatley):  
1. `git merge <branch_name>`  
2. `git add <files>`  
3. `git commit -m "<commit_message>"`  

Recursive merge (merging all commits as one):  
1. `git merge --squash <branch_name>`  
2. `git add <files>`  
3. `git commit -m "<commit_message>"`  

## Other merge related commands  

Abort current merge:  
`git merge --abort`  

Show what is merged:  
`git log --merge`  

Show changes between current and merge:  
`git diff`  

## Rebasing  

Rebase is creating and adding new commits infront of HEAD. Those commits have different id's so the history of changes is re-write by this action.  

To perform rebase I need to be switched to <working_branch> (where my changes are) and then perform rebase of <master_branch>. Git will automatically create copy of changes and add commits from <master_branch> in to my <working_branch> in correct places (copies id's will be different than original id's). Then I will be able to merge those changes to <master_branch>. Since id's were changed during copying from <master_branch> to <working_branch>, some history of <master_branch> will be lost.  

Rebase:  
1. `git switch <working_branch>`  
2. `git rebase <master_branch>`  
3. `git switch <master_branch>`  
4. `git merge <working_branch>`  

## Cherry pick:  
Cherry pick is used to copy some changes from <old_commit> which are in <working_branch> to <master_branch> and apply them. Cherry pick will create new commit with another new id.   

Cherry pick:  
`git cherry-pick <commit_id>`  

## Tags:  

Tags are used to mark commits. They allow fast access and easy selection of tagged commits.  

List tags:  
`git tag`  

Lightway tag (an reference to commit):  
`git tag <tag_name> <commit_id>`  

Annotaited tag (new object containing data like email of person who added that tag):  
`git tag -a <tag_name> -m "<tag_message>"`  

Show tagged commit:  
`git show <tag_name>`  

Checkout tagged commit:  
`git checkout <tag_name>`  

Removing tag:  
`git tag -d <tag_name>`  

## Repositories and branches:  

There are four different branches in Git that are related to repositories.  

1. Local branch - This is only a local branch, and it is not connected to a remote branch in any way. It is made with the "git init" command. To push changes to a remote branch, a remote tracking branch must be set up first with "git remote add <origin> <repo_url>", then changes can be pushed to GitHub with "git push <origin> <master_branch>". Basically, this one always requires specifying <origin> because it doesn't have upstream configured.  

2. Local tracking branch - This branch is a reference to a remote tracking branch and has an upstream configured. Changes can be pushed to the remote branch with "git push".  

3. Remote tracking branch - This branch is not for edition (it is always in a DETACHED HEAD state). It is just a local snapshot of the remote branch. It can be updated with "git fetch". When the "git push" command is executed in local tracking branch, changes are first copied here.  

4. Remote branch - This is a branch that exists only on the server.  

If branches are created manually, they must have same names. Otherwise git won't be able to setup whole route.  

List local branches:  
`git branch -a`  

List local branches with more details:  
`git branch -vv`  

List remote branches:   
`git ls-remote`  

Show remote servers:  
`git remote`  

Show remote servers configurations:   
`git remote show <orgin>`  

Connect local and remote branches:  
`git remote add <origin> https://<url>`  

Pushing changes to remote branch (upstream set):  
`git push`  

Pushing changes to remote branch (no upstream):  
`git push <origin> <master_branch>`  

Pushing changes to origin repository and setting upstream (-u will create local tracking branch):   
`git push -u <origin> <master_branch>`  

Updating all remote tracking branches:  
`git fetch`  

Updating selected remote tracking branch:   
`git fetch <origin> <master_branch>`  

Updating local branch (same as git fetch and git merge):  
`git pull <origin> <master_branch>`  

Updating local tracking branch (same as git fetch and git merge):   
`git pull`  

Create local tracking branch as reference to remote tracking branch (name of <local_tracking_branch> and <remote_tracking_branchname> must be same):  
`git branch --track <local_tracking_branch> <origin>/<remote_tracking_branchname>`  

Create new remote branch and remote tracking branch from local branch:  
`git push <origin> <new_branch_name>`  

Remove local tracking branch and remote tracking branch:  
`git branch --delete --remotes <branch_name>`  

Remove local tracking branch, remote tracking branch and remote branch:  
`git push origin --delete <branch_name>`  

Reverting commits in safe way:  
1. `git revert <commit_id>`  
2. `git push`  

Reseting head of remote branch (it will affect all users and it is not safe):  
1. `git reset --hard HEAD~1`  
2. `git push --force <origin> <branch_name>`  

Removing git history:  
1. `git reset --soft <initial_commit_id>`  
2. `git commit --amend -m "Initial commit"`  
3. `git push --force`  
