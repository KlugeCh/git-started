# GitHub repos

Our examples so far are stored in a remote repository and it can be found here on [GitHub](https://github.com/taitruong/git-started-examples). Please have a look. As you can see there are several branches like master and develop. Also there are [releases (tags)](https://github.com/taitruong/git-started-examples/releases).

# git remote add

Let's create a local repo and clone the remote repo:

```
$ mkdir git-started-examples; cd git-started-examples

$ git init
Initialized empty Git repository in D:/workspace/git-started-examples/.git/

$ git remote # shows all existing remotes, for now it's empty

$ git remote add origin https://github.com/taitruong/git-started-examples

$ git remote # a new remote with name 'origin' is added and points to our url
origin
```

Before we download our remote repo let's have a closer look at our local repo:
```
$ git remote -v # verbose view of remote repos and their urls
origin  https://github.com/taitruong/git-started-examples (fetch)
origin  https://github.com/taitruong/git-started-examples (push)

$ git branch --all # show all existing branches (output is empty - no branches, only master exsits)
```

# git fetch - Download Remote Repo
Now download the remote repo and its data to our git repo:

```
$ git fetch origin # download remote named 'origin'
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 22 (delta 4), reused 19 (delta 4), pack-reused 0
Unpacking objects: 100% (22/22), done.
From https://github.com/taitruong/git-started-examples
 * [new branch]      develop               -> origin/develop
 * [new branch]      master                -> origin/master
 * [new tag]         v0.2.1.RELEASE-commit -> v0.2.1.RELEASE-commit

$ git branch --all # now it lists all remote branches
  remotes/origin/develop
  remotes/origin/master

$ git tag # ... and all tags
v0.2.1.RELEASE-commit

```

# git merge - Merge Remote into Working Tree

So far you have only downloaded it. Now you can merge ('copy') them into your folder (working tree):
```
$ git tag
v0.2.1.RELEASE-commit

$ git merge v0.2.1.RELEASE-commit

$ git branch
* master

$ git status
On branch master
nothing to commit, working tree clean

$ ls
001_wonderland/  acme.md  LICENSE  master  README.md
```

Instead of merging a specific tag/release you could also check our a branch like 'origin/master' or 'origin/develop'.

# git clone - Adding a Remote Repo, Fetching it and Merge Master Alltogether

Above we did the following:
- add a remote repo
- fetch the complete repo
- merge a specific branch or tag into our local master

In case you want to clone the whole repo and fetch the master immediately there is a command doing all this:
```
$ git clone https://github.com/taitruong/git-started-examples
Cloning into 'git-started-examples'...
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 22 (delta 4), reused 19 (delta 4), pack-reused 0
Unpacking objects: 100% (22/22), done.

$ cd git-started-examples/

$ ls
001_wonderland/  acme.md  LICENSE  master  README.md  test
```

By convention git always uses the name 'origin' for a cloned remote repo.

# git remote add -f -t - Cloning and Fetching Specific Branches

Sometimes you don't want to fetch all branches, but only one or more branches:
```
$  mkdir git-started-examples; cd git-started-examples

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/git-started-examples
$ git init
Initialized empty Git repository in D:/workspace/git-started-examples/.git/

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/git-started-examples (master)
$ git remote add -f --tags -t master -t develop examples https://github.com/taitruong/git-started-examples
Updating examples
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 22 (delta 4), reused 19 (delta 4), pack-reused 0
Unpacking objects: 100% (22/22), done.
From https://github.com/taitruong/git-started-examples
 * [new branch]      master                -> examples/master
 * [new branch]      develop               -> examples/develop
 * [new tag]         v0.2.1.RELEASE-commit -> v0.2.1.RELEASE-commit
```

In this examples 'git remote add' the following options are used:
- '-f': fetch remote repo
- '-t branch': fetch this specific branch
- '--tags': import all tags

In this case the remote name 'examples' is used (instead of origin):
```
$ git remote
examples
```

# git remote show <name>

There are differences between 'git remote add' and 'git clone'.

A repo adding manually one remote remote using 'git remote add':
```
$ mkdir myrepo;cd myrepo; git init
Initialized empty Git repository in D:/workspace/myrepo/.git/

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/myrepo (master)
$ git remote add -f --tags origin https://github.com/taitruong/git-started-examples
Updating origin
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 22 (delta 4), reused 19 (delta 4), pack-reused 0
Unpacking objects: 100% (22/22), done.
From https://github.com/taitruong/git-started-examples
 * [new branch]      develop               -> origin/develop
 * [new branch]      master                -> origin/master
 * [new tag]         v0.2.1.RELEASE-commit -> v0.2.1.RELEASE-commit

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/myrepo (master)
$ git remote
origin

$ git remote show origin
* remote origin
  Fetch URL: https://github.com/taitruong/git-started-examples
  Push  URL: https://github.com/taitruong/git-started-examples
  HEAD branch: master
  Remote branches:
    develop tracked
    master  tracked

$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

A repo with a cloned remote repo using 'git clone':
```
$ git clone https://github.com/taitruong/git-started-examples                   Cloning into 'git-started-examples'...
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 22 (delta 4), reused 19 (delta 4), pack-reused 0
Unpacking objects: 100% (22/22), done.

$ cd git-started-examples/

$ git remote
origin

$ git remote show origin
* remote origin
  Fetch URL: https://github.com/taitruong/git-started-examples
  Push  URL: https://github.com/taitruong/git-started-examples
  HEAD branch: master
  Remote branches:
    develop tracked
    master  tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)

$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

The difference between both is that a cloned repo has these additional configurations:
```
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

This leads to difference 'git status' messages, where a local repo keeps track with a cloned repo:
```
$ git status # local is in sync with remote
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

Here, wet  have locally committed a new change:
```
$ git rm test
rm 'test'

$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    test

$ git commit -m 'remove dummy file'
[master 005a809] remove dummy file
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 test

$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

As a result our local repo is now ahead with one commit compared to the remote repo.

How can we keep track in our other, manually configure remote repo example? Well, we need to configure which branch on our remote repo (named 'origin') is our [upstream branch](git-scm.com/book/en/v2/Git-Branching-Remote-Branches#_tracking_branches), that is going to be tracked with our local master.

# git branch -u / --set-upstream-to: Define Remote Repo as Upstream

Defining an upstream requires the option '--set-upstream-to', '--track', or shorter using '-u':
```
$ git branch --set-upstream-to origin/master                                    fatal: branch 'master' does not exist

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/myrepo (master)
$ git branch
```

What happened here? We get an fatal error and even though we are working on the master, git says there is no such branch?!

Let's try creating another branch using 'git checkout -b' (we'll cover branches in another chapter), then we set our upstream branch to this branch:
```
$ git checkout -b other
Switched to a new branch 'other'

$ git branch -u origin/master
fatal: branch 'other' does not exist

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/myrepo (other)
$ git branch
```

Again, same error, other branch does not exist, and 'git branch' displays an empty message of existing branches.

This works as designed by git: every branch points to a commit and initially a git repo has no commit. So one solution is a commit something and then sets our upstream:
```
$ git checkout -b master # switch back to master
Switched to a new branch 'master'

$ touch dummy;git add --all

$ git commit -m 'dummy'
[master (root-commit) 31ebd7c] dummy
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 dummy

$ git branch -u origin/master
Branch 'other' set up to track remote branch 'master' from 'origin'.
```

When looking at git config you'll see at the end of the line:
```
$ git config --list
...
branch.other.remote=origin
branch.other.merge=refs/heads/master
```

Git adds two entries with 'branch.<name>.remote' and 'branch.<name>.merge' when executing 'git branch' with '-u' / '--set-upstream-to'.

With this configuration git knows what to do when calling 'git pull' (more below):
```
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/taitruong/git-started-examples
  Push  URL: https://github.com/taitruong/git-started-examples
  HEAD branch: master
  Remote branches:
    develop tracked
    master  tracked
  Local branch configured for 'git pull':
    other merges with remote master
```


# git branch --track: track between local and remote branch

Now we want to keep track and check wether or local branch is in sync with our remote branch:
```
$ git branch --track # check what's track so far
* other

$ git branch --track origin/master # track origin/master
Branch 'origin/master' set up to track local branch 'other'.

$ git branch --track
  origin/master
* other

$ git remote show origin
* remote origin
  Fetch URL: https://github.com/taitruong/git-started-examples
  Push  URL: https://github.com/taitruong/git-started-examples
  HEAD branch: master
  Remote branches:
    develop tracked
    master  tracked
  Local branch configured for 'git pull':
    other merges with remote master

$ git status
On branch other
Your branch and 'remotes/origin/master' have diverged,
and have 1 and 7 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean

$ git config --list
branch.other.remote=origin
branch.other.merge=refs/heads/master
branch.origin/master.remote=.
branch.origin/master.merge=refs/heads/other
```

