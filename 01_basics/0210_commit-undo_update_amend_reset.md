# git amend - Updating a Commit

Sometimes a commit is a missing file. For example you forgot to stage it. Or you've staged to many files.

## Add File

In this example there is already a committed file without content. Now we will edit this file and update the existing commit:
```
$ git log -1 --name-status # show last log
commit 9436f945d690bbb0350f301bfea962b152d2fedd (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Wed Sep 11 09:29:35 2019 +0200

    latest news from acme

A       acme.md

$ vi acme.md # add some content

$ git diff
diff --git a/acme.md b/acme.md
index e69de29..93395fb 100644
--- a/acme.md
+++ b/acme.md
@@ -0,0 +1 @@
+Acme town - much ado about nothing.
```

After we edit it we commit it to the latest commit using the '--amend' option. Instead of staging it, we just use the '-a' option:
```
$ git commit -a --amend # amend opens vi with commit message, just call ':q' to quit.
[master b91f35a] latest news from acme
 Date: Wed Sep 11 09:29:35 2019 +0200
 1 file changed, 1 insertion(+)
 create mode 100644 acme.md

$ git log -1 --name-status
commit b91f35a4cdaa02c3730d321bd02472368d4519c6 (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Wed Sep 11 09:29:35 2019 +0200

    latest news from acme

A       acme.md
```

In case we just want to change our commit message, we can simply call 'git commit --amend', edit the message in vi, call 'i' for inserting/change the message, and finally call ':wq' or 'ZZ' for saving and quitting the editor. As a result our commit gets a new message with a new SHA1 hashcode.

## Remove File

We could also remove a file and amend our commit:
```
$ rm undome

$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    undome

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -a --amend
[master d137ac3] latest news from acme
 Date: Wed Sep 11 09:29:35 2019 +0200
 1 file changed, 1 insertion(+)
 create mode 100644 acme.md

$ git status
On branch master
nothing to commit, working tree clean

$ git log -1 --name-status
commit d137ac36eba1fa30c2c1c76e733bde58626684cb (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Wed Sep 11 09:29:35 2019 +0200

    latest news from acme

A       acme.md
```

Another way of removing it, is using 'git rm'. This removes a file and staged it at the same time:
```
$ git rm yet-another-story.md
rm 'yet-another-story.md'

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    yet-another-story.md
```

# git rm - Removing File(s) from the Working Tree and from the Index

What if we just want extract a file from a commit and add it to a new commit? Similar to to 'git diff --cached' (which is the same like 'git diff --staged'), there is a git rm --cached:
```
$ git rm yet-another-story.md --cached
rm 'yet-another-story.md'

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    yet-another-story.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        yet-another-story.md


$ git commit --amend
[master 6fe3f5f] latest news from acme
 Date: Wed Sep 11 09:29:35 2019 +0200
 1 file changed, 1 insertion(+)
 create mode 100644 acme.md

$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        yet-another-story.md

nothing added to commit but untracked files present (use "git add" to track)

$ git log -1 --name-status
commit 6fe3f5f6356fe924ed1a17557cee2341eaef6b5e (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Wed Sep 11 09:29:35 2019 +0200

    latest news from acme

A       acme.md
```

# git reset - Unstaging a Staged File

In case you have accidentially staged a file, you can undo it with 'git reset':
```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md
        modified:   acme.md


$ git reset README.md acme.md # unstaged two files
Unstaged changes after reset:
M       README.md
M       acme.md

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md
        modified:   acme.md

no changes added to commit (use "git add" and/or "git commit -a")
```

In the above example you could also use 'git reset' for unstaging all files.

In case you want to complete discard your changes you can use the '--hard' option:

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   acme.md

$ git reset --hard
HEAD is now at 105b1e3 the latest news from acme

$ git status
On branch master
nothing to commit, working tree clean
```

Hard resetting discards all tracked changes, including modified and staged files.

# git checkout - Unmodfying a Modified File

Using 'git checkout' allows us discarding modified files:
```
$ ls
001_wonderland/  acme.md  master  README.md

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   acme.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md
        modified:   master

$ git checkout README.md
Updated 1 path from the index

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   acme.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   master
```

Now let's try the same and discard our master file:
```
$ git checkout master
Already on 'master'
M       acme.md
M       master

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   acme.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   master
```

It didn't do anything, saying we are already on 'master'. The reason is that 'master' is [ambiguous](https://git-scm.com/docs/git-checkout#_argument_disambiguation): 'git checkout' has an argument for defining a branch and another for a path. In our case there is a master branch and a file named 'master'. Therefor 'git checkout master' is switching to the master branch and tells us, we are already on this branch.

How can we address our master file? Let's try this:

```
$ git checkout master master
fatal: ambiguous argument 'master': both revision and filename
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
```

Ups, it doesn't work. But git tells us how to solve it, by using '--':

```
$ git checkout -- master

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   acme.md
```

Simply said '--' tells git to stay on the same branch.
