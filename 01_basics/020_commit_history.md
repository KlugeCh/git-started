# Show change of latest commit

With 'git diff' respectively 'git diff --staged' you can see changes of tracked files being unstaged or staged.

'git show' displays all changes from the latest commit:
```
$ git show
commit 25960e5db970e30c6c8d27c24513e17851f8c52b (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:53:30 2019 +0200

    chapter 2: red or blue potion

diff --git a/001_wonderland/rabbit_hole.md b/001_wonderland/rabbit_hole.md
index e6e20e2..9224c58 100644
--- a/001_wonderland/rabbit_hole.md
+++ b/001_wonderland/rabbit_hole.md
@@ -1,3 +1,9 @@
 # chapter 1 - rabbit hole

 Following the white rabbit, Alice fell into a deep hole.
+
+End of chapter 1.
+
+# chapter 2 - blue or red pill
+
+Alice has to choose between two colored potions - one blue, one red, to either continue her adventure or go back home.
```

# Commit history

'git log' shows all commits, starting with the latest, on your working branch (more on branches later):

```
$ git log
commit 25960e5db970e30c6c8d27c24513e17851f8c52b (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:53:30 2019 +0200

    chapter 2: red or blue potion

commit f97a220d93a8948b04528e283e8c3994a04178a9
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:31:04 2019 +0200

    finished with chapter one

commit 667f2377085e651af65ede3c19bd5fa8c40e425b
Author: Tai 'Mr. T' Truong <tai.truong@aliensource.org>
Date:   Mon Sep 9 17:46:23 2019 +0200

    readme describing this repo

$ git log -2 # show last 2 logs
commit 25960e5db970e30c6c8d27c24513e17851f8c52b (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:53:30 2019 +0200

    chapter 2: red or blue potion

commit f97a220d93a8948b04528e283e8c3994a04178a9
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:31:04 2019 +0200

    finished with chapter one

```

# SHA1-checksum

[Git is a content-addressable filesystem](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects). For each commit a unique SHA-1 checksum is created - based on each file's content.

Using the SHA1 hash you can for example show the previous commit:

```
$ git show f97a220d93a8948b04528e283e8c3994a04178a9
commit f97a220d93a8948b04528e283e8c3994a04178a9
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:31:04 2019 +0200

    finished with chapter one

diff --git a/001_wonderland/rabbit_hole.md b/001_wonderland/rabbit_hole.md
new file mode 100644
index 0000000..e6e20e2
--- /dev/null
+++ b/001_wonderland/rabbit_hole.md
@@ -0,0 +1,3 @@
+# chapter 1 - rabbit hole
+
+Following the white rabbit, Alice fell into a deep hole.
```

Instead of using the full 40-character SHA1 hash you can use its short form as shown each time you commit:
```
$ git commit -m 'finished with chapter one'
[master f97a220] finished with chapter one
 1 file changed, 3 insertions(+)
 create mode 100644 001_wonderland/rabbit_hole.md

$ git show f97a220
commit f97a220d93a8948b04528e283e8c3994a04178a9
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:31:04 2019 +0200

    finished with chapter one

diff --git a/001_wonderland/rabbit_hole.md b/001_wonderland/rabbit_hole.md
new file mode 100644
index 0000000..e6e20e2
--- /dev/null
+++ b/001_wonderland/rabbit_hole.md
@@ -0,0 +1,3 @@
+# chapter 1 - rabbit hole
+
+Following the white rabbit, Alice fell into a deep hole.q
```

# vi Editor

Since vi is our default editor it used everywhere. One common use case is, you are searching for change e.g. in the last 10 commits ('git log -10') and wants to show all changes for each commit using the p option. 'git log -10 -p' will show:
```
commit 25960e5db970e30c6c8d27c24513e17851f8c52b (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:53:30 2019 +0200

    chapter 2: red or blue potion

diff --git a/001_wonderland/rabbit_hole.md b/001_wonderland/rabbit_hole.md
index e6e20e2..9224c58 100644
--- a/001_wonderland/rabbit_hole.md
+++ b/001_wonderland/rabbit_hole.md
@@ -1,3 +1,9 @@
 # chapter 1 - rabbit hole

 Following the white rabbit, Alice fell into a deep hole.
+
+End of chapter 1.
+
:
```

Please note the double colon ':' at the last line. This means it waits for input entering a command like 'q' for quit. But you may also search through the last 10 commits using '/searchterm'.

# Show log with files

```
$ git log --name-status
commit 25960e5db970e30c6c8d27c24513e17851f8c52b (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:53:30 2019 +0200

    chapter 2: red or blue potion

M       001_wonderland/rabbit_hole.md

commit f97a220d93a8948b04528e283e8c3994a04178a9
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:31:04 2019 +0200

    finished with chapter one

A       001_wonderland/rabbit_hole.md

commit 667f2377085e651af65ede3c19bd5fa8c40e425b
Author: Tai 'Mr. T' Truong <tai.truong@aliensource.org>
Date:   Mon Sep 9 17:46:23 2019 +0200

    readme describing this repo

A       README.md

$ git show --name-status
commit 25960e5db970e30c6c8d27c24513e17851f8c52b (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@software-developer.org>
Date:   Tue Sep 10 14:53:30 2019 +0200

    chapter 2: red or blue potion

M       001_wonderland/rabbit_hole.md
```
