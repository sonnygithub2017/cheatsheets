* git fetch:
  * `git fetch origin` -- fetch all branches
  * `git fetch origin main` -- fetch only main branch
  * git fetch will do following:
    - download the latest changes from the remote repository
    - update remote-tracking branches in local repository, e.g. `origin/main` or `origin/feature`
    - does not change disk (working directory) or the local master branch
    - To see the changes of local-main and remote main: `git log main..origin/main`
* git merge:
  * switch to local main: `git checkout main`
  * `git merge origin/main` -- merge the remote changes into the local main branch
  * if there is a conflict, resolve it manually and then `git add <file>`, `git commit -m "msg"`, and `git push origin main`
* git pull -- fetch and merge:
  * switch to main `git checkout main`
  * git pull to fetch and merge: `git pull origin master`
    - internal it combine git fetch and git merge:
      - `git fetch origin master`: fetch the changes from remote master branch
      - `git merge origin/master`: merge the changes into the local master branch
* example of conflict resolution for git pull:
  * `git chechout main`
  * you are change some git tracked files, like `example.txt`
    ```
    Line 1
    Line 2
    Line 3
    Line 4 from local main branch
    ```
  * remote also have that file changed like , like `example.txt`
    ```
    Line 1
    Line 2
    Line 3
    Line 4 from remote main branch
    ```
  * If changed files are in working directory or staging, can't use `git pull`, but you can use `git fetch`, the merge in `git pull` will be be prevented. you must first commit or stash them,
    * if commit:
      *  `git add <example.txt>` and `git commit -m "msg"`
      *  `git pull origin main` will cause conflict
    * stash them:
      * `git stash` -- will save the changes in a stack, and remove them from working directory
      * `git pull origin main` will not cause conflict
      * `git stash pop` to apply the changes back to working directory -- cause conflict
  * Resolve the conflict:
    - use `vi <file>`: accept yours, theirs or both, and remove the conflict markers (<<<<<<<, =======, >>>>>>>)
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
      - once modified, the saved file will just be in disk (working directory), with `git add example.txt` to put it in staging
    - vscode with merge editor: 3 windows will open
      - you will 3 windows open and number of conflicts
        - top left: incoming changes (theirs): you can accept incoming, both, or ignore
        - top right: current changes (yours): you can accept current, both, or ignore
        - bottom: result of merge (editable): you can edit the result, or reverse to either incoming or current
      - after finish editing/resolving, you will see "0 conflict"
      - click "complete merge" button to put the resolved files in staging
  - commit the merge: `git commit -m "Resolve merge conflict in example.txt"` -- create new commit
  - push the changes: `git push origin main` or `git push origin HEAD:refs/for/master` if using Gerrit


