# Preliminary

## Git Install

- [git-scm.com - installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), note: for Windows preferably use Vim as default editor
- [An Illustrated Guide to Git on Windows](http://nathanj.github.io/gitguide/)

## Git configuration

There are some important configs to be set. Like user name and email, which is used for commits. 
Open shell/Git bash:
```
$ git config --global --list # initially the global config is empty

$ git config --global user.name 'John Doe' # sets globally your user name

$ git config --global user.email 'user@acme.com'

$ git config --global --list
user.email=tai.truong@software-developer.org
user.name=Tai 'Mr. T' Truong
```

Converting line-endings:
```
$ git config --global core.autocrlf true # converts linefeed (lf) to carriage return + linefeed (crlf) source: https://stackoverflow.com/a/5834094
```
NOTE: when using Windows tools like notepad then set it to true, when using linux tools like Vi in Git Bash then set it to false.

## Vi/Vim Basics

- short introduction here: [Basic vi commands](https://www.thegeekdiary.com/basic-vi-commands-cheat-sheet/)
  - there are two modes: when starting vi you are in i) command mode and from there you can switch to ii) insert mode
  - commands can be executed in two modes: vi mode and command mode
    - commands in command mode starts with a double colon ':', e.g. ':q' for quitting the vi editor
    - all other commands in vi mode starts with another character than ':'
  - execute inser command by pressing 'i'
  - in insert mode press escape key for switching back to command mode
- basic commands:
  - 'i' - insert starting from cursor
  - 'I' - insert starting beginning of line
  - 'a' - append starting from cursor
  - 'A' - append starting at end of line
  - 'x' - delete character at cursor
  - 'dd' / 'Ndd' - delete/cut current line, delete more lines like: 2dd, 3dd, 4dd ...
  - 'p' - paste
  - ':q' - quit editor, in case of changes an error message is shown
  - ':q!' - force quit (and discard changes)
  - ':w' - write to file
  - 'ZZ' - save and quit
  - '/searchtext' + ENTER - search for text, press 'n' for next
  - ':s/search/replace/' - search and replace

For example if you enter 'git config --global --edit' it uses vi for editing the global git configuration.

## Markdown files (like this README file)

- guideline: [GitHub Markdown](https://guides.github.com/features/mastering-markdown/)
- every repo should have a README.md file on top

# First steps

## git init - Create Local Git Repo

First we open a shell (Git Bash in Windows) and create a new Git repo:
```
$ git --version
git version 2.21.0.windows.1

$ cd path_to_your_workspace

$ pwd # show current path
/path_to_your_workspace

$ ls # list files from current path

$ mkdir myrepo;cd my repo # create folder for new Git repo; and cd/change to this directory 

$ git init # initialize/create Git repo with a master branch
Initialized empty Git repository in D:/workspace/myrepo/.git/
```

## git status - Show the Working Tree Status

'git status' shows all tracked and untracked files (more about this later in the basic chapters). For initial project it is empty:
```
$ git status # show status on current branch
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)

```

While we keep working it may look like this:
```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   file1.txt
        new file:   test1

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   acme/file2.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        test2
```

Let us create a readme file for our repo:
```
$ echo 'hello world' > README.md

$ tail README.md
hello world

$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.md

nothing added to commit but untracked files present (use "git add" to track)

$ less README.md # show file content, press 'q' for quit
```

## git add - Staging files

In Git only those files are committed that are in the staging area. For committing one or more files Git requires two steps:

1. git add - new (untracked) or changed (modified) files need to be staged / added to the staging area
2. git commit - commit staged files

```
$ git add README.md

$  git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.md
```

## git rm / git rest - Unstaging Files

One possibility to unstage a file is:
```
$ git rm --cached README.md
rm 'README.md'

$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.md
```

Another is:
```
$ git reset README.md

maita@LAPTOP-P4D7LDG2 MINGW64 /d/workspace/myrepo (master)
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.md
```

## Committing files from our staged area

```
$ git commit -m 'readme describing this repo'
[master (root-commit) 667f237] readme describing this repo
 1 file changed, 1 insertion(+)
 create mode 100644 README.md

$ git status
On branch master
nothing to commit, working tree clean

$ git log
commit 667f2377085e651af65ede3c19bd5fa8c40e425b (HEAD -> master)
Author: Tai 'Mr. T' Truong <tai.truong@aliensource.org>
Date:   Mon Sep 9 17:46:23 2019 +0200

    readme describing this repo

```


