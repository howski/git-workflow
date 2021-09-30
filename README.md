# Direction for Git based on Gitflow model
This is a summary of the [successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/), developed by Vincent Driessen. This is largely the underlying structure of Gitflow, which is a popular addition to Git to add ease of use. However, I believe it is more transparent to work directly with the standard git commands.

## Introduction
For any given repository, `origin` should be treated as the only central repository. Each person who is working on the repository should have their own local copy, which they pull down from `origin`. Once they are satisfied with the work they have done, or they need to share it with the rest of the team, they then push the changes present in their local repository to `origin`. The exact push and pull commands will depend on the stage of work, which is covered below.

### Branches
This model is heavily based on utilising branches. The two primary branches are `master` and `develop`, which are supplemented by `feature`, `release`, and `hotfix` branches. The supplementary branches can (and should) contain multiple commits as work is done on them. These are then merged back into `master` or `develop` as appropriate.

* The `origin/master` branch is where the source code is in a production-ready state.

* The `origin/develop` branch is where the code is in preparation for the next release. Once the code is ready, then it should be merged back to `origin/master`.

* The `feature` branches are used to add new features to the code. These typically exist only at the local level, although may have `origin` counterparts if multiple developers are working on the same `feature` branch.

* The `release` branches are used to add final changes to the code before merging with `master` and pushing back to `origin/master`. This allows `develop` and `origin/develop` to progress forward and new features to be implemented without needing to wait for small changes such as formatting. When `release` is merged into `master` and `develop`, the code is considered to be at a new version, and this can then be pushed to `origin`.

* The `hotfix` branches are similar to `release` branches, but are used to address more immediate issues that are present in `master` and `origin/master`, such as critical bugs. Again, this allows others to continue working on `develop` and `origin/develop` while the bug is fixed. After the issue is resolved, `hotfix` is merged into `develop` and `master`, which can then be pushed to `origin`.

As an additional note:

merge is used to combine local branches
push is from local to origin
pull is from origin to local
checkout -b is used to create a branch
checkout is used to move to a branch
anything enclosed in <> indicates that you need to supply the appropriate name

## Relevant Commands
### Initialisation

1. Clone an existing git repository to your local machine: `git clone` `https://github.com/<username>/<repo>.git/`

Or, if no existing repository exists, run the following from the project folder:

1. `git init`
2. `git commit --allow-empty -m "<Initial commit message>"`
3. `git checkout -b develop master`
4. Create a repository on [github](https://github.com)
5. `git remote add origin https://github.com/<username>/<repo>.git`
6. `git push -u origin --all`

### Get up to date

Before you do any work, make sure your local `master` and `develop` branches are up to date with `origin`:

git pull origin master
git pull origin develop
Feature Branches

Create

Create the feature branch from the local develop branch: git checkout -b <featurename> develop
Work

Check you’re in the right branch:
List branches: git branch
If not in the feature branch: git checkout <featurename>
Do work
Commit locally:
Add the files you want to commit: git add <filename>
Commit the files at the local branch level: git commit -m "<commit message>"
If you need to send your work on the feature to others:
Push commits to origin: git push origin <featurename>
If you need to receive work done on the feature by others:
Pull commits from origin: git pull --rebase origin <featurename>
Finalise

Move to the develop branch: git checkout develop
Merge into develop without fast-forwarding: git merge --no-ff <featurename>
Delete the feature branch: git branch -d <featurename>
Push develop to origin/develop: git push origin develop
If you’ve previously pushed the feature branch to origin: git push origin :<featurename>
Release Branches

Create

Create the release branch from the local develop branch: git checkout -b release-<releasenumber> develop
Update documents to reflect change in release number (e.g. 1.1 -> 1.2)
Commit changes: git commit -a -m "Changed to version <releasenumber>"
Work

Check you’re in the right branch:
List branches: git branch
If not in the release branch: git checkout release-<releasenumber>
Do work
Commit locally:
Add the files you want to commit: git add <filename>
Commit the files at the local branch level: git commit -m "<commit message>"
If you need to send your work on the release to others:
Push commits to origin: git push origin release-<releasenumber>
If you need to receive work done on the release by others:
Pull commits from origin: git pull --rebase origin release-<releasenumber>
Finalise

Move to the master branch: git checkout master
Merge into the master branch without fast-forwarding: git merge --no-ff release-<releasenumber>
Tag the release: git tag -a <releasenumber>
Move to the develop branch: git checkout develop
Merge into develop without fast-forwarding: git merge --no-ff release-<releasenumber>
Delete the release branch: git branch -d release-<releasenumber>
Push master to origin/master: git push origin master
Push develop to origin/develop: git push origin develop
Push tags: git push origin --tags
If you’ve previously pushed the release branch to origin: git push origin :release-<releasenumber>
Hotfix Branches

Create

Create the hotfix branch from the local master branch: git checkout -b hotfix-<hotfixnumber> master
Update documents to reflect change in release number (e.g. 1.2.1 -> 1.2.2)
Commit changes: git commit -a -m "Changed to version <hotfixnumber>"
Work

Check you’re in the right branch:
List branches: git branch
If not in the hotfix branch: git checkout hotfix-<hotfixnumber>
Do work
Commit locally:
Add the files you want to commit: git add <filename>
Commit the files at the local branch level: git commit -m "<commit message>"
If you need to send your work on the hotfix to others:
Push commits to origin: git push origin hotfix-<hotfixnumber>
If you need to receive work done on the release by others:
Pull commits from origin: git pull --rebase origin hotfix-<hotfixnumber>
Finalise

Move to the master branch: git checkout master
Merge into the master branch without fast-forwarding: git merge --no-ff hotfix-<hotfixnumber>
Tag the release: git tag -a <hotfixnumber>
Move to the develop branch: git checkout develop
Merge into develop without fast-forwarding: git merge --no-ff hotfix-<hotfixnumber>
Delete the hotfix branch: git branch -d hotfix-<hotfixnumber>
Push master to origin/master: git push origin master
Push develop to origin/develop: git push origin develop
Push tags: git push origin --tags
If you’ve previously pushed the hotfix branch to origin: git push origin :hotfix-<hotfixnumber>
