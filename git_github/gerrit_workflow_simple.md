### Workflow for git with Gerrit

**Entities**:
  1. Remote-git: Gerrit
  2. Local-git: Local repository
  3. Staging: `git add`
  4. Disk: File system

**Stages**:
  1. **Initial**:
     - Only the remote repository has the master branch.
  2. **Clone to local**: `git clone <gerrit-url>`
     - Example: `git clone https://fetch-gerrit.dyn.nutanix.com/nutest-py3-tests`
     - Local machine gits Git source code control (.git folder and multiple config files).
     - Both disk and local repository have the master branch.
  3. **Create New Branch**: to avoid messing up the master branch: `git checkout -b wrk`
     - The local repository now has master and wrk branches.
     - Switches to `wrk`.
  4. **Modify files**: on `wrk`: `vi <file>` or use VSCode
     - Changes are made on disk, but not yet in local-git.
     - To see changes between disk and local-git: `git diff <file>`
  5. **Add and commit**: the file:
     - Stage changes: `git add <file>`
     - Commit to the local repository: `git commit -m "message"`
     - Ensure your commit message includes a Change-Id (Gerrit typically generates this automatically if you have the commit-msg hook installed)
     - if you have changes after commit, you can use `git commit --amend` to keep as one commit
     - The local repository now has a new commit based on original master-init.
  6. **Rebase changes** on top of the latest updated master:
     - Get new base from remote master, and wrk's change on top of master-updates
     - `git pull --rebase` or `git pull --rebase origin master`
     - If there is a conflict:
       - Resolve it manually using `vi <conflict-file>` or VSCode.
       - Mark the conflict as resolved: `git add <resolved-file>`
       - Continue rebase: `git rebase --continue`
     - Note: `git rebase` creates new commit IDs (hashes) based on new updates.
       - Temporarily removes original commit from `wrk`.
       - Applies the latest master commits to `wrk`.
       - Replays original commit (same contents and change-id) on top of master commit with new commit IDs.
  7. **Push for review**: `git push origin HEAD:refs/for/master`
     - This will generate code-review page on Gerrit: e.g., `https://gerrit.eng.nutanix.com/c/nutest-py3-tests/+/984435/`
  8. If need additional changes after review.
    - make the changes, and `git add <file>`
    - `git commit --amend` to keep the **same change-id** but new commit-id.
    - `git push origin HEAD:refs/for/master` to push the changes to Gerrit
    - This will same review page as before as the change-id is same.
  9. Gerrit: once approved, merge the change into the master branch.
  10. Remove `wrk` locally (optional):
      - Switch to master: `git checkout master`
      - Delete `wrk`: `git branch -D wrk`
  11. Update local master: `git pull origin master`

**Table summary**:
| Stage                | Command                                | Disk                 | Staging    | Local-git                           | Remote-git                         |
| -------------------- | -------------------------------------- | -------------------- | ---------- | ----------------------------------- | ---------------------------------- |
| Initial              |                                        | x                    | x          | x                                   | master init                        |
| Clone to local       | `git clone <gerrit-url>`               | master init          | x          | master init                         | master init                        |
| Create branch        | `git checkout -b wrk`                  | wrk init             | x          | *wrk init<br>master init            | master init                        |
| Add/edit file        | (change) `git diff`                    | wrk change           | x          | *wrk init                           | master init                        |
| Add to stage         | `git add <file>`                       | wrk change           | wrk change | wrk init                            | master init                        |
| Commit               | `git commit -m "message"`              | wrk change           | x          | wrk change                          | master init                        |
| Rebase remote master | `git pull --rebase`                    | wrk change           | x          | *wrk update-change                  | master update                      |
| Push to master       | `git push origin HEAD:refs/for/master` | wrk change           | x          | *wrk update-change                  | wrk update-change<br>master update |
| Gerrit:Review change | Gerrit: Review change                  | wrk change           | x          | *wrk-update-change                  | wrk-update-change<br>master update |
| Gerrit:Merge change  | Gerrit: merge change                   | wrk change           | x          | *wrk-update-change                  | master-update-change               |
| switch to master     | `git checkout master`                  | master-update        | x          | *master-update<br>wrk-update-change | master-update-change               |
| Delete branch(local) | `git branch -D wrk`                    | master-update        | x          | master-update                       | master-update-change               |
| Update local         | `git pull origin master`               | master update-change | x          | master-update-change                | master-update-change               |
