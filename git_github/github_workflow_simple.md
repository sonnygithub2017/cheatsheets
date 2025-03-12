**Reference**: [YouTube - Simple GitHub Workflow](https://www.youtube.com/watch?v=uj8hjLyEBmU)

**Entities**:
  1. remote-git: GitHub
  2. local-git: Local repository
  3. staging: `git add`
  4. disk: File system

**Stages**:
  1. Initial:
     - Only the remote repository has the main branch.
  2. Clone to local: `git clone <github-url>`
     - Example: `git clone https://github.com/sonnygithub2017/cheatsheets.git`
     - The local machine will have Git resource control (.git folder and multiple config files).
     - Both disk and local repository have the main branch.
  3. Create a new branch to avoid messing up the main branch: `git checkout -b mybranch`
     - Creates `mybranch` locally. The local repository now has two branches: main and mybranch.
     - Switches to `mybranch` locally.
  4. Modify files on `mybranch`: `vi <file>` or use VSCode
     - Changes are made on disk, but not yet in local-git.
     - To see changes between disk and local-git: `git diff`
  5. Add and commit the file:
     - Add the changed file to staging: `git add <file>`
     - Commit the changes to the local repository: `git commit -m "message"`
     - The local repository now has a new commit.
  6. Push `mybranch` to remote: `git push origin mybranch`
     - The remote repository will have a new branch `mybranch`, in addition to main.
     - Note: Use `git push -u origin mybranch` to set upstream, so next time you can use `git push` or `git pull` only.
  7. Get new changes from remote main:
     - Switch to main locally: `git checkout main`
     - Pull updates from remote main: `git pull origin main`
     - Switch back to `mybranch`: `git checkout mybranch`
     - Rebase `mybranch` on top of main: `git rebase main`
     - If there is a conflict:
       - Resolve it manually using `vi <conflict-file>` or VSCode.
       - Mark the conflict as resolved: `git add <resolved-file>`
       - Continue rebase: `git rebase --continue`
     - Note: `git rebase` creates new commit IDs (hashes).
       - Temporarily removes commits from `mybranch`.
       - Applies the latest main commits to `mybranch`.
       - Replays original commits (same contents and message) on top of main commits with new commit IDs.
       - Pull request URLs remain the same.
  8. Push to remote: `git push -f origin mybranch`
     - The remote `mybranch` will have changes on top of the main updates.
     - `-f` force push, as we have rebased `mybranch` (commit IDs changed).
  9. On GitHub:
     - `mybranch` owner:
         - Click "Pull request" to request merging `mybranch` into main.
         - Send for review. Example: https://github.com/sonnygithub2017/cheatsheets/pull/1
     - Main branch owner:
         - Click "Squash and merge" to squash multiple changes in `mybranch` into one and merge into main.
     - `mybranch` owner: Click "Delete branch" to remove `mybranch` from remote.
  10. Remove `mybranch` locally:
      - Switch to main: `git checkout main`
      - Delete `mybranch`: `git branch -D mybranch`
  11. Update local main: `git pull origin main`

**Table summary**:
| Stage          | Command                       | Disk             | Staging         | Local-git        | Remote-git       |
| -------------- | ----------------------------- | ---------------- | --------------- | ---------------- | ---------------- |
| Initial        |                               | x                | x               | x                | main init        |
| Clone to local | `git clone <github-url>`      | main init        | x               | main init        | main init        |
| Create branch  | `git checkout -b mybranch`    | mybranch init    | x               | mybranch init    | main init        |
| Add/edit file  | (change) `git diff`           | mybranch change  | x               | mybranch init    | main init        |
| Add to stage   | `git add <file>`              | mybranch change  | mybranch change | mybranch init    | main init        |
| Commit         | `git commit -m "message"`     | mybranch change  | x               | mybranch commit  | main init        |
| Push to remote | `git push origin mybranch`    | mybranch change  | x               | mybranch commit  | mybranch commit  |
| Update main    | `git pull origin main`        | main updated     | x               | main updated     | main updated     |
| Rebase         | `git rebase main`             | mybranch rebased | x               | mybranch rebased | main updated     |
| Push rebased   | `git push -f origin mybranch` | mybranch rebased | x               | mybranch rebased | mybranch rebased |
| Pull request   |                               | x                | x               | x                | mybranch PR      |
| Merge          |                               | x                | x               | main merged      | main merged      |
| Delete branch  | `git branch -D mybranch`      | x                | x               | main merged      | main merged      |
| Update local   | `git pull origin main`        | main updated     | x               | main updated     | main updated     |
