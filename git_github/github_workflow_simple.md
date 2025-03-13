**Reference**: [YouTube - GitHub With Review Workflow](https://www.youtube.com/watch?v=uj8hjLyEBmU)

**Entities**:
  1. Remote-git: GitHub
  2. Local-git: Local repository
  3. Staging: `git add`
  4. Disk: File system

**Stages**:
  1. Initial:
     - Only the remote repository has the main branch.
  2. Clone to local: `git clone https://github.com/username/repo.git`
     - Example: `git clone https://github.com/sonnygithub2017/cheatsheets.git`
     - The local machine will have Git source code control (.git folder and multiple config files).
     - Both disk and local repository have the main branch.
  3. Create a new branch to avoid messing up the main branch: `git checkout -b mybranch`
     - The local repository now has two branches: main and mybranch.
     - Switches to `mybranch` locally.
  4. Modify files on `mybranch`: `vi <file>` or use VSCode
     - Changes are made on disk, but not yet in local-git.
     - To see changes between disk and local-git: `git diff <file>`
  5. Add and commit the file:
     - Add the changed file to staging: `git add <file>`
     - Commit the changes to the local repository: `git commit -m "message"`
     - The local repository now has a new commit based on original main-init.
  6. Get new updates from remote main:
     - Switch to main locally: `git checkout main`
     - Pull updates from remote main: `git pull origin main`
     - Switch back to `mybranch`: `git checkout mybranch`
     - Get new base (updates) from main (mybranch's change on top of main-updates): `git rebase main`
     - If there is a conflict:
       - Resolve it manually using `vi <conflict-file>` or VSCode.
       - Mark the conflict as resolved: `git add <resolved-file>`
       - Continue rebase: `git rebase --continue`
     - Note: `git rebase` creates new commit IDs (hashes) based on new updates.
       - Temporarily removes original commit from `mybranch`.
       - Applies the latest main commits to `mybranch`.
       - Replays original commit (same contents and message) on top of main commit with new commit IDs.
  7. Push `mybranch` to remote: `git push origin mybranch`
        - The remote repository will have a new branch `mybranch`, in addition to main.
        - NOTE: use `git push -f origin mybranch` if there is mybranch in github.
        - `-f` force push, as we have rebased `mybranch` (commit IDs changed).
  8.  GitHub: Create a Pull request for review:
       - Click on "Pull requests" tab
       - click on "New pull request" and select your branch: `mybranch`
       - Send for review. Example: https://github.com/sonnygithub2017/cheatsheets/pull/1
  9.  GitHub: merge the Pull request
       - review the pull request
       - Click "Squash and merge" to squash multiple changes in `mybranch` into one and merge into main.
  10. Github: Delete branch
     - Click "Delete branch" to remove `mybranch` from remote.
  11. Remove `mybranch` locally:
      - Switch to main: `git checkout main`
      - Delete `mybranch`: `git branch -D mybranch`
  12. Update local main: `git pull origin main`

**Table summary**:
| Stage                 | Command                    | Disk                             | Staging         | Local-git                                | Remote-git                                     |
| --------------------- | -------------------------- | -------------------------------- | --------------- | ---------------------------------------- | ---------------------------------------------- |
| Initial               |                            | x                                | x               | x                                        | main init                                      |
| Clone to local        | `git clone <github-url>`   | main init                        | x               | main init                                | main init                                      |
| Create branch         | `git checkout -b mybranch` | mybranch init      | x               | *mybranch init<br>main init              | main init                                      |
| Add/edit file         | (change) `git diff`        | mybranch change                  | x               | *mybranch init                            | main init                                      |
| Add to stage          | `git add <file>`           | mybranch change                  | mybranch change | mybranch init                            | main init                                      |
| Commit                | `git commit -m "message"`  | mybranch change                  | x               | mybranch change                          | main init                                      |
| switch to main        | `git checkout main`        | main init    | x               | *main init<br>mybranch change            | main update                                   |
| Update main           | `git pull origin main`     | main update                     | x               | *main update<br>mybranch change                             | main update                                   |
| switch to mybranch    | `git checkout mybranch`    | mybranch change | x               | *mybranch change<br>main update         | main update                                   |
| Rebase main           | `git rebase main`          | mybranch change                  | x               | *mybranch update-change                 | main update                                   |
| Push to mybranch      | `git push origin mybranch` | mybranch change                  | x               | *mybranch update-change                 | mybranch update-change<br>main update        |
| create Pull request   | GitHub: "Pull request"     | mybranch change                  | x               | *mybranch-update-change                 | mybranch-update-change<br>main update        |
| merge Pull request    | GitHub: "Squash&Merge"     | mybranch change                  | x               | *mybranch-update-change                 | mybranch-update-change<br>main-update-change |
| delete branch(Github) | GitHub: delete mybranch    | mybranch change                  | x               | *mybranch-update-change                 | main-update-change                            |
| switch to main        | `git checkout main`        | main-update | x               | *main-update<br>mybranch-update-change | main-update-change                            |
| Delete branch(local)  | `git branch -D mybranch`   | main-update                     | x               | main-update                             | main-update-change                            |
| Update local          | `git pull origin main`     | main update-change              | x               | main-update-change                      | main-update-change                            |
