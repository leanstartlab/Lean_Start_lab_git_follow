
## Lean Start Lab Git flow

Reference flow: [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

### Precondition
* Already created central repository on Github (or Bitbucket)
* Default branch of central repository is master
* New member will fork central reposity
* Decided reviewer and person who will merge pull request

### Principle
* Each pull request will match with only 1 redmine ticket.
* Each pull-request does not limit the number of commits.
* Title of commit will be `refs [tracker of ticket] #[number of ticket] [title of ticket]`. (For example: `refs bug #1234 Can not remove cache`).
* For the commit title, in the case of pull-request there is only one commit, then the commit name is the same as above. `Refs [Ticket Type] # [Ticket Number] [Ticket Content]` (Example: `refs bug # 1234 Fixed a cache error. \
* However, if a ppull-request contains multiple commits, it must be specified in the commit title that the response handles in the commit.
     * For example:
         Pull-request title: `refs bug # 1234 Fixed a cache error
         2. In case pull-request has 2 commits, the commit title of commit 2 will be as follows
             * `Create method for clearing cache in Model`
             * `The controller calls the method in the model to perform a clear cache
* In the local environment, the code can not be changed in the branch master. Must work on branch initialization to do task.

### Prepare

1. Fork central repository to your account on Github (Bitbucket)

2. Clone forked repository to local. At this time, forked repository will be automatically registered as `origin` on local.
    ```sh
    $ git clone [URL of forked repository]
    ```

3. Enter the directory created by `clone` command, register central repository as `upstream`.
    ```sh
    $ cd [created directory]
    $ git remote add upstream [URL of central repository]
    ```

### Develop

From now, we will call central repository as `upstream`, forked repository as `origin`.

1. Sync local's master branch with upstream's master branch.
    ```sh
    $ git checkout master
    $ git pull upstream master
    ```

2. Create new branch to do task from master branch on local. Name of branch should be number of that task.(For example: `task/1234`)
    ```sh
    $ git checkout master # <--- unnecessary in case already on master branch
    $ git checkout -b task/1234
    ```

3. Do your task (Freedomly create multiple commits).

4. Push code to `origin`

    ```sh
    $ git push origin task/1234
    ```

5. On Github (Bitbucket), create pull request from origin's  `task/1234` to upstream's `master` branch.

6. Paste pull request page's URL to Slack group, ask reviewer to have code-review.

    6.1. If reviewer asks you to fix something, do 3. ~ 5. step.
    6.2. Repaste pull request page's URL to Slack group, ask reviewer to have code-review.

7.  When more than 2 reviewers gave you OK comment, reviewer who gave the final OK comment will merge that pull request.

8. Back to 1 step.

### For projects corresponding provisions applicable with 1 pull-request only allows 1 commit

From now, we will call central repository as `upstream`, forked repository as `origin`.

1. Sync local's master branch with upstream's master branch.
    ```sh
    $ git checkout master
    $ git pull upstream master
    ```

2. Create new branch to do task from master branch on local. Name of branch should be number of that task.(For example: `task/1234`)
    ```sh
    $ git checkout master # <--- unnecessary in case already on master branch
    $ git checkout -b task/1234
    ```

3. Do your task (Freedomly create multiple commits).

4. If you created multiple commits when doing task, please use `rebase i` command to group them into 1 commit before moving to step 5.
    ```sh
    $ git rebase -i [hash value of the commit before the first commit you created or the number of commits needs to be grouped]
    ```

5. Move to local's master branch and update to latest version.
    ```sh
    $ git checkout master
    $ git pull upstream master
    ```

6. Move back to your task branch, rebase this branch with master branch.
    ```sh
    $ git checkout task/1234
    $ git rebase master
    ```
    **If conflict error occurs when rebasing, please refer to "fix conflict error when rebasing" procedure.**

7. Push to `origin`

    ```sh
    $ git push origin task/1234
    ```

8. On Github (Bitbucket), create pull request from origin's  `task/1234` to upstream's `master` branch.

9. Paste pull request page's URL to Slack group, ask reviewer to have code-review.

    9.1. If reviewer asks you to fix something, do 3. ~ 6. step.

    9.2. Use `push -f` command to force push to the same remote branch.
    ```sh
    $ git push origin task/1234 -f
    ```

    9.3. Repaste pull request page's URL to Slack group, ask reviewer to have code-review.

10. When more than 2 reviewers gave you OK comment, reviewer who gave the final OK comment will merge that pull request.

11. Back to 1 step.

### Fix conflict error when rebasing

If conflict error occurs when rebasing, it will be displayed as below (at this moment, you will be moved to anonymous branch automatically).
```sh
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: refs #1234 Can not remove cache
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging path/to/conflicting/file
CONFLICT (add/add): Merge conflict in path/to/conflicting/file
Failed to merge in the changes.
Patch failed at 0001 refs #1234 Can not remove cache
The copy of the patch that failed is found in:
    /path/to/working/dir/.git/rebase-apply/patch

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
```

1. Fix all the conflicts manually (code which is surrounded by <<< an >>>)
Use `git rebase --abort` if you want to abort rebase process.

2. After fixing all conflicts, continue your rebase process.

    ```sh
    $ git add .
    $ git rebase --continue
    ```
