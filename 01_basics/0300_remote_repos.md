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
