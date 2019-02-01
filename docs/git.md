# Git

## Rebasing a fork before making a Pull Request

Rebasing can be a tricky process. I typically do it on a new branch to avoid messing anything up.

First make sure you origin is up to date before making any changes locally.

`git push`

Optional: If you have commits that you want to squash into one, you can do it by the following command.

`git rebase -i HEAD~<number of commits to squash>`

or

`git rebase -i <hash of first commit to keep>`

This will open an editor with a list of commits. Each commit will have `pick` before the hash.  Change `pick` to `squash` if you want a commit to be squashed down to the commit before it.


Add a remote for the upstream repository.

`git remote add upstream <url to upstream repository>`

Fetch latest commits from the upstream repository.

`git fetch upstream`

Rebase your repo with the upstream. This essentially updates your repository with the branch on upstream that you specify. Its simmilar to doing a merge except, it takes your commits and puts them at the top of the commit history.

`git rebase -i upstream/master`

It is recommended to check your repository's history to make sure everything looks correct.

`git log`

If it is correct then do

`git push --force`

Since the history is different than your origin, it is necessary to force push to update it.