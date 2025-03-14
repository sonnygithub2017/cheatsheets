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
  - Restore: To the first commit before HEAD (HEAD~1)
    - Undo last commit, keep changes staged: `git reset --soft HEAD~1`
    - Undo last commit, keep changes on disk: `git reset HEAD~1`
    - Undo last commit, discard changes in all places: `git reset --hard HEAD~1`

### Change in Remote Git
  - `git push origin HEAD:refs/for/master` or `git push origin main`
  - Revert: add a new commit that undoes the changes:
    - init -> change1 -> change2 -> change3 -> change1-revert
    - Sync with remote: `git pull origin main`
    - Find commit ID of plan-to-revert: `git log`
    - Add a new commit to revert old-commit: `git revert <commit-id>`
    - If conflict, resolve it manually, then `git add <f1> <f2> <f3>` and `git revert --continue`
    - Push changes: `git push origin main`
