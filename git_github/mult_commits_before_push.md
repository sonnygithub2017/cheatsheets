### Situation
You have made multiple commits in your local repository and want to push them to the remote repository, aiming for a single review for all commits.

### For GitHub
- No need to squash commits before pushing.
- GitHub will display all commits in one pull request. Even if you have new pushes, they will be added to the same pull request **as long as the pull request is open**.

- ***Option 1: Push once for 3 commits to remote***:
  1. Checkout your branch: `git checkout mybranch`
  2. Have 3 commits:
    ```bash
    git add file1
    git commit -m "Commit message 1"
    git add file2
    git commit -m "Commit message 2"
    git add file3
    git commit -m "Commit message 3"
    ```
  3. Rebase your branch on top of the updated main: `git rebase main` (local main) or `git pull --rebase origin main` (remote main)
  4. Resolve conflicts if any, then `git add <resolved-file>` and `git rebase --continue`
    - Before rebase: old-main -> commit1 -> commit2 -> commit3
    - After rebase: new-main -> commit1 -> commit2 -> commit3
  5. Push your branch to remote: `git push origin mybranch`
  6. On GitHub, create a pull request for review. The pull request will show all commits.

- ***Option 2: Push multiple times, one push for one commit***. You still have a single pull request for review.
  1. Checkout your branch: `git checkout mybranch`
  2. Have 3 commits, each commit is pushed to remote:
    ```bash
    git add file1
    git commit -m "Commit message 1"
    git pull --rebase origin main
    git push origin mybranch

    git add file2
    git commit -m "Commit message 2"
    git pull --rebase origin main
    git push origin mybranch

    git add file3
    git commit -m "Commit message 3"
    git pull --rebase origin main
    git push origin mybranch
    ```
  3. On GitHub, create a pull request for review. The pull request will show all commits.

### For Gerrit
- Squash commits before pushing, or multiple pushes. In both cases, you will have only one code review **as long as the change-id is the same**.

- ***Option 1: Squash commits before pushing*** (one push for 1 squashed commit):
  1. Initial commits:
    ```bash
    git add file1
    git commit -m "Commit message 1"
    git add file2
    git commit -m "Commit message 2"
    git add file3
    git commit -m "Commit message 3"
    ```
  2. Interactive rebase: `git rebase -i HEAD~n` where `n` is the number of commits to squash, e.g., `git rebase -i HEAD~3`
    This opens an editor with the list of commits:
    ```
    pick abcdef1 Commit message 1
    pick abcdef2 Commit message 2
    pick abcdef3 Commit message 3
    ```
  3. Squash commits: change `pick` to `squash` for all commits except the first one:
    ```
    pick abcdef1 Commit message 1
    squash abcdef2 Commit message 2
    squash abcdef3 Commit message 3
    ```
    Save and close the editor.

    NOTE: if you want to drop a commit, change `pick` to `drop`.

  4. Edit the commit message if needed:
    The editor will open again with the combined commit message:
      ```
      Commit message 1

      # This is a combination of 3 commits.
      # The first commit's message is:
      #
      # Commit message 1
      #
      # The second commit's message is:
      #
      # Commit message 2
      #
      # The third commit's message is:
      #
      # Commit message 3
      ```
  5. Rebase your branch on top of the latest master: `git pull --rebase` or `git pull --rebase origin master`
  6. Resolve conflicts if any, then `git add <resolved-file>` and `git rebase --continue`
  7. Push changes to remote for review: `git push origin HEAD:refs/for/master`
  8. On Gerrit, you will see only one commit for review.

- ***Option 2: Multiple pushes, each push for one commit***. But still the same code review.
  1. Checkout your branch: `git checkout mybranch`
  2. Have 3 commits, each commit is pushed to remote. All commits except the first one will use `git commit --amend` for the same change-id:
    ```bash
    git add file1
    git commit -m "Commit message 1"
    git pull --rebase origin main
    git push origin HEAD:refs/for/master

    git add file2
    git commit --amend -m "Commit message 2"
    git pull --rebase origin main
    git push origin HEAD:refs/for/master

    git add file3
    git commit --amend -m "Commit message 3"
    git pull --rebase origin main
    git push origin HEAD:refs/for/master
    ```
  3. On Gerrit, you will see multi-commits but same code review (with the same change-id).