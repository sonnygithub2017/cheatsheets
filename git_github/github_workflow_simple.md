* See [youtube-simple-github-workflow](https://www.youtube.com/watch?v=uj8hjLyEBmU)

* Steps:
  1. Create a new repository on GitHub
  2. Clone the repository to your local machine
  3. Create a new file in the repository
  4. Add the file to the staging area
  5. Commit the file
  6. use `git pull --rebase` get pull update, and put my change on top of it
  7. Push the file to the remote repository
  8. Pull the file from the remote repository
  9. Create a new branch
  10. Switch to the new branch
  11. Merge the new branch with the master branch
  12. Push the new branch to the remote repository
  13. Delete the new branch
  14. Pull the changes from the remote repository
  15. Delete the repository
  16. more steps ...
  17. steps 17
  18. steps 18


Stages: with disk, staging, local and remote entities
  1. initial:
    - only remote have main branch
  2. clone to local: `git clone <github-url>`,
     1. ex: `git clone https://github.com/sonnygithub2017/cheatsheets.git`
     2. local will have git source control now (.git folder and multiple config files will be created)
     3. disk and local have main branch
  3. create mybranch: `git checkout -b mybranch`,
    - create mybranch in local. local have 2 branches: main and mybranch.
    - and switch to mybranch in local, so not mess main
  4. add/edit file: `vi <file>`,
    - change file in disk, but local does not change
    - to see changes between disk and local: `git diff`
  5. add to stage: `git add <file>`,
    - changed file added to "staging"
  6. commit: `git commit -m "msg"`,
    - changed file added to "local"
    - local have new commit
  7. push branch to remote: `git push origin mybranch`,
    - remote will have new branch `mybranch`, in addition to main
    - note: you can use `git push -u origin mybranch` to set upstream, so next time you can use `git push` or `git pull` only
  8. maybe now remote main have update, we need to make sure our change are working over that updates
    - local switch to main (disk and local): `git checkout main`,
    - local sync up with remote main update (disk and local) `git pull origin main`
  9.  put our mybranch change on top of main update
    - local switch to mybranch (disk and local): `git checkout mybranch`,
    - local put mybranch change on top of main:  `git rebase main`
    - NOTE: if there is conflict,
      - resolve it manually, `vi <conflict-file>`
      - mark the conflict as resolved:  `git add <resolved-file>`
      - continue rebase:  `git rebase --continue`
  10. push to remote `git push -f origin mybranch`,
    - push my new changes on mybranch to remote mybranch,
  11. remote github:
      1.  "compare & pull request": request main to pull mybranch into main
      2.  "squash and merge": squash multiple changes in mybranch into one, and merge into main
      3.  "delete mybranch": remove mybranch from remote
  12. local remove mybranch:
      1.  `git checkout main` switch to main
      2.  `git branch -D mybranch` delete mybranch from local

| stage          | cmd                    | disk      | staging   | local-git | remote-git |
| -------------- | ---------------------- | --------- | --------- | --------- | ---------- |
| initial        |                        | x         | x         | x         | main init  |
| clone to local | git clone <github-url> | main init | X         | main init | main init  |
| create branch  | git checkout -b mybranch`, |mybranch`, init |           | x         | main init  |
| add/edit file  | (change) git diff      |mybranch`, init |mybranch`, init | x         | main init  |
| add to stage   | git add <file>         |mybranch`, init |mybranch`, init | x         | main init  |
