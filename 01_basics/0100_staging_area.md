# Git States: Tracked and Untracked Files

Have a look at this file lifecycle in Git as described here: [Figure 8. The lifecycle of the status of your files](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository). A file does either have a tracked or untracked state. A tracked state are unmodified, modified or staged files. An untracked state is a new or deleted file.

# Staging an Untracked File

Exercise: check the Git status after doing each of these steps:
- create a new file
- edit file and add some content
- add/stage the file

```
$ git status
On branch master
nothing to commit, working tree clean

$ mkdir 001_wonderland

$ touch 001_wonderland/rabbit_hole.md # new file

$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        001_wonderland/

nothing added to commit but untracked files present (use "git add" to track)

$ vi 001_wonderland/acme.md # edit file

$ tail 001_wonderland/rabbit_hole.md
# Chapter One - The Rabbit Hole

$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        001_wonderland/

nothing added to commit but untracked files present (use "git add" to track)

$ git add 001_wonderland/rabbit_hole.md # stage untracked file

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   001_wonderland/rabbit_hole.md
```

As a result we have a new file that is staged and can now be committed.

# Modifying a Staged File

Before we commit our staged file, let's change it again:
```
$ vi 001_wonderland/rabbit_hole.md

$ tail 001_wonderland/rabbit_hole.md
# Chapter One - The Rabbit Hole

Following the white rabbit, Alice fell into a hole.

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   001_wonderland/rabbit_hole.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   001_wonderland/rabbit_hole.md
```

As a result we have the same file being staged (as a new file) and unstaged (as a modified file). You can check the changes of untracked files (compared to the latest commit) by using 'git diff':

```
$ git diff # show changes of modified file
diff --git a/001_wonderland/rabbit_hole.md b/001_wonderland/rabbit_hole.md
index a76dcb5..e6e20e2 100644
--- a/001_wonderland/rabbit_hole.md
+++ b/001_wonderland/rabbit_hole.md
@@ -1,2 +1,3 @@
 # chapter 1 - rabbit hole

+Following the white rabbit, Alice fell into a deep hole.

$ git diff --staged # show changes from the stage area
diff --git a/001_wonderland/rabbit_hole.md b/001_wonderland/rabbit_hole.md
new file mode 100644
index 0000000..a76dcb5
--- /dev/null
+++ b/001_wonderland/rabbit_hole.md
@@ -0,0 +1,2 @@
+# chapter 1 - rabbit hole
+
```

# Commit completed tasks

Normally you commit your work, when you are done or has a reached a certain level of completion. Especially when you work on a complex story/feature, you split up your work into several tasks. Every time you have reached at some working level of a feature you'd like to 'save' your intermediate result.

It is up to you whether you would like to a) commit your intermediate work or wait until everything is finished and b) commit the final work.

Starting from the above example, with the same file being staged and modified, git status showed us this:
```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   001_wonderland/rabbit_hole.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   001_wonderland/rabbit_hole.md
```

In case of a) committing your intermediat work you would do this:

- commit your stage file(s)
- keep working on your modified file(s) until you have a reached another intermediate state
- staged your work
- commit your next intermediate work.

In our example, we go for b) commit when work is final. Here it may still makes sense to stage our intermediate work - except we do not commit it.

Finally, we stage all our work and commit it:

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   001_wonderland/rabbit_hole.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   001_wonderland/rabbit_hole.md


$ git add 001_wonderland/rabbit_hole.md

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   001_wonderland/rabbit_hole.md


$ git commit -m 'finished with chapter one'
[master f97a220] finished with chapter one
 1 file changed, 3 insertions(+)
 create mode 100644 001_wonderland/rabbit_hole.md
```

# shortcut: committing directly and skip staging

Sometimes this workflow is too complex than you need. You may want to directly commit all your changes using the '-a' option:

```
$ vi 001_wonderland/rabbit_hole.md

$ git diff
diff --git a/001_wonderland/rabbit_hole.md b/001_wonderland/rabbit_hole.md
index e6e20e2..4e30b7d 100644
--- a/001_wonderland/rabbit_hole.md
+++ b/001_wonderland/rabbit_hole.md
@@ -1,3 +1,5 @@
 # chapter 1 - rabbit hole

 Following the white rabbit, Alice fell into a deep hole.
+
+End of chapter 1.

$ git add 001_wonderland/rabbit_hole.md

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   001_wonderland/rabbit_hole.md

$ vi 001_wonderland/rabbit_hole.md

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   001_wonderland/rabbit_hole.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   001_wonderland/rabbit_hole.md

$ git commit -a -m 'chapter 2: red or blue potion'
[master 25960e5] chapter 2: red or blue potion
 1 file changed, 6 insertions(+)

$ git status
On branch master
nothing to commit, working tree clean

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/myrepo (master)
$ tail
.git/           001_wonderland/ README.md

$ tail 001_wonderland/rabbit_hole.md
# chapter 1 - rabbit hole

Following the white rabbit, Alice fell into a deep hole.

End of chapter 1.

# chapter 2 - blue or red pill

Alice has to choose between two colored potions - one blue, one red, to either continue her adventure or go back home.
```

Please note that the '-a' option considers only modified and deleted files, but NOT new files - as explained in the man pages using 'git commit --help':

```
git-commit(1) Manual Page
...
OPTIONS
-a
--all
Tell the command to automatically stage files that have been modified and deleted, but new files you have not told Git about are not affected.
...
```
