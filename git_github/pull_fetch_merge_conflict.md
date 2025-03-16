references: https://www.youtube.com/watch?v=HosPml1qkrg&t=254s

### git fetch:
  * `git fetch origin` -- fetch all branches
  * `git fetch origin main` -- fetch only main branch
  * git fetch will:
    - download the latest changes from the remote repository
    - update **remote-tracking branches** in the local repository, e.g., `origin/main`
    - the downloaded files will be stored in the .git folder under remote-tracking branches (e.g., `origin/main`), **not visible in the working directory or the local main branch**. NOT see with `ls` or `git log`
    - To see the changes:
      - see logs: `git log main..origin/main`
      - see changes: `git diff main..origin/main`
      - see content: `git show origin/main` or `git show origin/main:path/to/file`
### git merge:
  * switch to local main: `git checkout main`
  * `git merge origin/main` -- merge the remote changes into the local main branch
  * if there is a conflict, resolve it manually, then `git add <file>`, `git commit -m "msg"`
### git pull -- fetch and merge:
  * switch to main: `git checkout main`
  * `git pull origin main` -- fetch and merge changes from the remote main branch
    - internally combines `git fetch` and `git merge`:
      - `git fetch origin main`: fetch changes from the remote main branch
      - `git merge origin/main`: merge changes into the local main branch
### Example of conflict resolution for git pull:
  * `git checkout main`
  * Local changed file, e.g., `example.txt`
    ```
    Line 1
    Line 2
    Line 3
    Line 4 from local main branch
    ```
  * Remote also changed file `example.txt`
    ```
    Line 1
    Line 2
    Line 3
    Line 4 from remote main branch
    ```
  * Changes in the working directory or staging: **Can NOT** `git pull`. Use `git fetch` (without merge) instead. To proceed with `git pull`, commit or stash your changes:
    * commit:
      * `git add <example.txt>` and `git commit -m "msg"`
      * `git pull origin main` will cause a conflict
    * stash:
      * `git stash` -- save changes in a stack and remove them from the working directory or staging
      * `git pull origin main` will not cause a conflict
      * `git stash pop` to apply changes back to the working directory -- may cause a conflict
  * Resolve the conflict:
    - use `vi <file>`: you will see **conflict markers** (<<<<<<<, =======, >>>>>>>), make changes to accept yours or theirs, and remove the markers
        ```
        Line 1
        Line 2
        Line 3
        <<<<<<< local main (current changes)
        Line 4 from local main branch
        =======
        Line 4 from remote main branch
        >>>>>>> remote main (incoming changes)
        ```
      - After resolving, use `git add example.txt` to stage it
    - vscode **Merge Editor**: 3 windows will open
      - top left: incoming changes (theirs)
      - top right: current changes (yours)
      - bottom: result of merge (editable) - Also see "X conflicts"
      - after resolving, see **"0 conflict"**, click "complete merge" to stage the resolved files
  - commit the merge: `git commit -m "Resolve merge conflict in example.txt"`
  - push the changes: `git push origin main`(GitHub) or `git push origin HEAD:refs/for/main` (Gerrit)


