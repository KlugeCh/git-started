# Gitflow

The following branching model is based on the Gitflow workflow. More details from its [original blog](https://nvie.com/posts/a-successful-git-branching-model/) and from the [Atlassian git tutorial](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

![gitflow diagram](0400_gitflow.png).
[Source: Vincent Driessen](https://nvie.com/posts/a-successful-git-branching-model/)

The Gitflow workflow has these branches:
- develop branch
  - contains complete history with all commits
  - serves as integration branch for other branches (feature, release, hot fix branches)
  - tracking branch for developers
- master master
  - reflects production environment
  - stores release history
  - fixes from hotfixes branch
- <my-story-xyz> feature branches
  - parent: develop branch
  - short-living fork/branch
  - created on the latest develop branch and may be updated *from* develop all the time until completion
  - may be owned by specific developer
  - every story has its own feature branch
  - merged/rebased into develop branch (never to master)
  - merge/rebase may be done via pull request for code review
  - may be deleted after completion
- release branch(es)
  - parent: develop branch
  - short-living fork/branch
  - fork of develop branch based on defined release (based on features or date)
  - no (feature) further update from develop (unlike feature branches)
  - only commits for bug fixes and non-functional tasks (e.g. dokumentation)
  - every shipment/release will be tagged (e.g. 1.2.0-M1, 1.2.0-M2).
  - further shipments leads to further tags (e.g. 1.2.0-RELEASE)
  - every shipment merges release branch into master
- hotfix branches
  - parent: master
  - hotfixes are the only branches directly forking off of our master branch
  - merged into master and develop
  - may be deleted

# Git's Data Model

Every time you commit Git creates 3 object types:

![commit and tree](0400_commit-and-tree.png)
Source: [branches in a nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell#_git_branches_overview)

1. one or more blob objects: each representing
   - a content for file
2. a tree object: where each line represents
   - file name
   - file type
   - pointer (hash) to blob
   - file mode bits
3. a commit object: containine
   - tree with hash pointer to tree object
   - (if available) parent with hash point to parent object 
   - author details
   - committer details

All these objects are stored in your git repository under .git/objects. For each object a SHA-1 hash checksum is created. The hash code is then used as a file name for storing under ./git/objects.

# .git repository - Under the hood

Let's creat a new repo and add for our first commit a new file:

```
$ mkdir foo;cd foo; git init
Initialized empty Git repository in D:/workspace/foo/.git/

$ echo 'hello world' > file1.txt

$ git add --all

$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   file1.txt

$ git commit -m 'initial commit'
[master (root-commit) 3cdee16] initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 file1.txt

$ git log -1
commit 3cdee168b670b652c3bb8943c8a92dafc4643a61 (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Sat Sep 14 12:05:14 2019 +0200

    initial commit
```

A new commit object with hash code 3cdee168b670b652c3bb8943c8a92dafc4643a61 is created and stored in our git repo:
```
$ ls .git/objects/
3b/   3c/   82/   info/ pack/

$ ls .git/objects/3c/
dee168b670b652c3bb8943c8a92dafc4643a61
```


# git cat-file: view git objects

Let's have a look at our first commit object:
```
$ git cat-file -p 3cdee16 # show content of commit object
tree 82424451ac502bd69712561a524e2d97fd932c69
author Tai 'Mr. T' Truong <tai.truong@software-developer.org> 1568455514 +0200
committer Tai 'Mr. T' Truong <tai.truong@software-developer.org> 1568455514 +0200

initial commit
```

As you can see our commit object contains information about a tree object and further commit details.

Now look at our tree object:
```
$ git cat-file -p 82424451ac502bd69712561a524e2d97fd932c69
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad    file1.txt
```

The tree object points to one file name 'file1.txt' and its content is stored by its given hash:
```
$ git cat-file -p 3b18e512dba79e4c8300dd08aeb37f8e728b8dad
hello world
```

Now let us create a new file in a sub folder:
```
$ mkdir acme

$ echo 'yet another content' > acme/file2.txt

$ git add --all

$ git commit -m 'yet another file'
[master 8176101] yet another file
 1 file changed, 1 insertion(+)
 create mode 100644 acme/file2.txt
```

Our second commit has points to our initial, parent comit:
```
$ git cat-file -p 8176101
tree b32d53fecafef302cbb9a8fae173b428f5cba5cc
parent 3cdee168b670b652c3bb8943c8a92dafc4643a61
author Tai 'Mr. T' Truong <tai.truong@software-developer.org> 1568466098 +0200
committer Tai 'Mr. T' Truong <tai.truong@software-developer.org> 1568466098 +0200

yet another file
```

Our tree object basically reflects our root folder holding:
- a tree object to our acme folder and
- a blob to our existing file 'file1.txt'.

Since file1 hasn't changed it points to the same hash code:
```
$ git cat-file -p b32d53fecafef302cbb9a8fae173b428f5cba5cc
040000 tree 9a9455e766bb146d7e308c1634765ee19a5fc889    acme
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad    file1.txt
```

Now have a look at our new tree object and inside our new blob object:
```
$ git cat-file -p 9a9455e766bb146d7e308c1634765ee19a5fc889
100644 blob f12e4a639dc7c3642a3502e08c94ed8a92c8982b    file2.txt

$ git cat-file -p f12e4a639dc7c3642a3502e08c94ed8a92c8982b
yet another content
```

Finally let's change file2, create file 3, add them and commit it:
```
$ echo '2nd line with more content' >> acme/file2.txt
maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/foo (master)

$ mkdir acme/looney

$ echo 'acme at looney toon' > acme/looney/file3.txt

$ git add --all

$ git commit -m 'more from looney toon'
[master 2026fdd] more from looney toon
 2 files changed, 2 insertions(+)
 create mode 100644 acme/looney/file3.txt
```

Again, our commit object points to our previous, parent object:
```
$ git cat-file -p 2026fdd
tree 7cc38c99b94e04a717c06072fb70783f9c109c2c
parent 8176101ed2e54d69d3778a6d2618aa5a7ea93a7d
author Tai 'Mr. T' Truong <tai.truong@software-developer.org> 1568466667 +0200
committer Tai 'Mr. T' Truong <tai.truong@software-developer.org> 1568466667 +0200

more from looney toon
```


The tree object starts from our root folder showing:

```
$ git cat-file -p 7cc38c99b94e04a717c06072fb70783f9c109c2c
040000 tree 1e6b228394fef57752e460c5a5d3cf82c681daa0    acme
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad    file1.txt
```

File1 is unchanged and our next tree object shows two new object: one for the changed file2 object and one for the new subfolder.
```
$ git cat-file -p 1e6b228394fef57752e460c5a5d3cf82c681daa0
100644 blob ad4a1fab1e9ca8d87828e9dfc5d2b56381a58906    file2.txt
040000 tree 6b64ed18f10e9df6eb199c925213e49ea1043f36    looney
```

Finally let's have a look at the remainder:
```
$ git cat-file -p ad4a1fab1e9ca8d87828e9dfc5d2b56381a58906
yet another content
2nd line with more content

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/foo (master)
$ git cat-file -p 6b64ed18f10e9df6eb199c925213e49ea1043f36
100644 blob 0efb5b216c566f1c5950d8f6decc62514924045c    file3.txt

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/foo (master)
$ git cat-file -p 0efb5b216c566f1c5950d8f6decc62514924045c
acme at looney toon
```

Read more on the official documentation about [Git Objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) and on this [blog](https://www.daolf.com/posts/git-series-part-1/).


