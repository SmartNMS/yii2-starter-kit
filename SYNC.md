# Yii 2 Starter Kit Extra


##SYNC A FORK

### Configuring a remote for a fork
To sync changes you make in a fork with the original repository, you must configure a remote that points to the upstream repository in Git.
(https://help.github.com/articles/configuring-a-remote-for-a-fork/#platform-linux)

Open Terminal.

List the current configured remote repository for your fork.
```
git remote -v
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

Specify a new remote upstream repository that will be synced with the fork.

git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```
git remote add upstream https://github.com/trntv/yii2-starter-kit.git
```

Verify the new upstream repository you've specified for your fork.
```
git remote -v
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

### Syncing a fork
Sync a fork of a repository to keep it up-to-date with the upstream repository.

Before you can sync your fork with an upstream repository, you must configure a remote that points to the upstream repository in Git.
(https://help.github.com/articles/syncing-a-fork/#platform-linux)

1. Open Terminal.
2. Change the current working directory to your local project.
3. Fetch the branches and their respective commits from the upstream repository. Commits to master will be stored in a local branch, upstream/master.
```
git fetch upstream
```

4. Check out your fork's local master branch.
```
git checkout master
```

5. Merge the changes from upstream/master into your local master branch. This brings your fork's master branch into sync with the upstream repository, without losing your local changes.
```
git merge upstream/master
```

If your local branch didn't have any unique commits, Git will instead perform a "fast-forward":
```
git merge upstream/master
```

```
Tip: Syncing your fork only updates your local copy of the repository. To update your fork on GitHub, you must push your changes.
```

