# Git tricks

## Commit tricks

* To add and commit a change to a repository
```bash
    git commit -am "message"
```
* However this is only applicable for files tracked by git. If we create a new file, we still need to give `git add .`
* Create aliases for git commands
```bash
    git config --global alias.ac "commit -am"
    git ac "message"
```
* To update the last commit message,
```bash
    git commit -amend -m "updated_message"
```
* To update the last commit with new files,
```bash
    git add .
    git commit -amend "message"
```
* To keep the same commit message,
```bash
    git add .
    git commit -amend --no-edit
```
* To undo previous n commits,
```bash
    git reset HEAD~1
    git reset --soft HEAD~1  # Best practice
```
* To revert a commit yet have it in the history,
```bash
    git revert <commit_id hash> 
```
`A new revert commit will be added here instead of resetting it`

## Push tricks

`NOTE: The above only works if we haven't pushed the code to remote repository`
* In that case, we can push with the force flag. This will override the remote commit with the state of the local code. But we might loose the commits which we dont have in the remote yet.
```bash
    git push origin <branch_name> --force
```
* But this is not safe. To force push only if there is no conflicts, 
```bash
    git push origin <branch_name> --force-with-lease
```
* To revert a commit from a remote repository
```bash
    git revert <commit_id>
```
    * This will undo one commit and bring it to its previous state
    * This wont actually remove the commit from the history. It just goes to a previous state
* To make changes directly to you remote github repository, 
    * Open your repository which you want to work on
    * Click on "."
    * It opens a VSCode version of your repository

## Stash tricks

* `git stash`
    * To save the the stash with a name, 
    ```bash
        git stash save <some_name>
    ```
    * To find the stash
    ```bash
        git stash list
    ```
    * To apply the stash
    ```bash
        git stash apply <index of some_name>
    ```
    * To apply and remove the stash from the stash list
    ```bash
        git stash pop
    ```
* To rename the master branch
```bash
    git branch -M main
```
* To get the git log in a readable manner,
```bash
    git log --graph --oneline --decorate
```
* If we have a commit that is breaking our app and we want to track it we can use `git bisect`
    * `git bisect` allows to start from a commit which has a bug mostly the recent commit
    * We can point bisect to the last known working commit and then perform a binary search to walk you through each commit in between
    * If the commit looks good, type 
    ```bash
        git bisect good <commit_id>
    ```
    * Eventually we find the bad one and We'll know exactly which code to fix
* `git squash` : Squashing your commits
    * If we have more than one commit in our feature branch which is ready to be pushed to the master/main.
    * However all these commits are pointless and it would be better if it was one single commit, we can do that by
    ```bash
        - git checkout <feature_branch>
        - git rebase master --interactive
        
        pick 398f22b Added Courses with Form module
        pick 08f4dbb Added Course Forms
        pick 11a8132 Added service calls to mock service
        pick bddc0ca Form CRUD added
        pick 9885e89 Added Update to Course Form
        pick b1796a5 Added Errors
        pick b4d635f Added Routes
        pick 430cb73 Added Forms, http and  Routers
        pick 848c7b6 Removed alerts

        # Rebase 3ed20af..848c7b6 onto 3ed20af (9 commands)
        #
        # Commands:
        # p, pick <commit> = use commit
    ```
    * We can pick a command using `pick` keyword or squash it using `squash`
    * This will then lead to another file prompting to update the commit message
    * We can use fixup and set up squash when doing rebase
    * We can use fixup and squash with commit 
     ```bash
        git commit --fixup <commit_id>
        git commit --squash <commit_id>
    ```
    * On doing this we tell git in advance that we are going to squash them.
    * Now we can autosquash it using Rebase
* `AUTO SQUASHING`
```bash
    git rebase -i -autosquash    
```
    * This will squash automatically based on the previous commits.

## Hooks trick

* `Git hooks`
    * Whenever we perform a commit, git creates an event.
    * Hook allows too bind these events to some kind of hooks
    * For ex: We can start unit test on commit or lint our code. We can use `husky` a JS package for this
* To go back to the original state from the remote.
```bash
    git fetch origin
    git reset --hard origin/master  
```
* This Overrides local code with remote code. Local code will be removed completely.
* But we might still be left with some untracked files. In that case, we 
```bash
    git clean -df
```
* It removes those untracked files as well.
* If we recently switched out of branch and forgot its name we can use the below command to go back to the previous branch.
```bash
    git checkout -
```
## Add tricks

* If we want to only add segments or specific lines of a file, we can use git add path command
```bash
    git add -p
```
* This command will show the changes in a specific file and we can start adding selected changes and commit them

-- -