# Gitflow

The following branching model is based on the Gitflow workflow. More details from its [original blog](https://nvie.com/posts/a-successful-git-branching-model/) and from the [Atlassian git tutorial](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

![gitflow diagram](0400_gitflow.png).

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

# git clone
Before we start let's clone our Git repo example from GitHub:
```
$ git clone https://github.com/taitruong/git-started-examples                     Cloning into 'git-started-examples'...
remote: Enumerating objects: 20, done.
remote: Counting objects: 100% (20/20), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 20 (delta 3), reused 17 (delta 3), pack-reused 0
Unpacking objects: 100% (20/20), done.
```

Next let's check what tags the are defined and checkout the 0.2.1 release:
```

# Create a New Branch


