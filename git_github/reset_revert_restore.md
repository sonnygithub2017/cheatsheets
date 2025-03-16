reference: https://www.youtube.com/watch?v=S7XpTAnSDL4&t=1159s

### A file tracked in 4 places:
  - Disk (working directory)
  - Staging area
  - Local git repository
  - Remote git repository

### Change in Disk
  - `vi <file>`
  - Check:
    - `git diff <file>` - diff between disk and local git
    - `git status` - Changes not staged for commit
  - Restore change on disk: `git restore <file>`

### Change in Staging
  - `git add <file>`
  - Check: `git status` - Changes to be committed
  - Restore:
    - Unstage: `git restore --staged <file>`
    - Discard changes on staging and disk: `git checkout HEAD <file>`
    - NOTE: HEAD is the last commit. This restores the file to the last commit.

### Change in Local Git
  - `git commit -m "message"`
  - Check:  `git log` - commit history:
  - Restore: To the first commit after HEAD (HEAD~1)
    - Undo last commit, keep changes staged: `git reset --soft HEAD~1`
    - Undo last commit, keep changes on disk: `git reset HEAD~1`
    - Undo last commit, discard changes in all places (disk, staging and commit): `git reset --hard HEAD~1`
    - NOTE: Can also use **commit-id**. e.g., `git reset <commit-id>` to reset to a specific commit.

### Change in Remote Git
  - `git push origin HEAD:refs/for/master` or `git push origin main`
  - Revert: add a new commit that undoes the changes, remote can only forward, not backward
    - Example: change1 -> change2 -> change3 -> change1-revert
  - Steps:
    - Sync with remote: `git pull origin main`
    - Find commit-id (full or short) of plan-to-revert: `git log`
    - Add a new commit to revert old-commit: `git revert <commit-id>`
      - If conflict, resolve it manually, then `git add <f1> <f2> <f3>` and `git revert --continue`
      - This will create a new commit that undoes the changes of the old commit
    - Push changes: `git push origin main`(github) or `git push origin HEAD:refs/for/master`(gerrit)

### git stash
  - remove changes from the working directory or staging and Save them in a stack
  - scenarios: you are in the middle of a change and need to work on some urgent work. you can
    - commit your changes (preferred)
    - or stash your changes
  - `git stash` - delete changes from working directory or staging and save to stack
  - switch to another branch or work on another task
  - `git stash pop` - apply changes back to the working directory to resume original task
  - `git stash list` - list saved changes
  - `git stash clear` - remove all changes from the stack