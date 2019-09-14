# git tag - Tagging

It is possible to tag every commit and branch.

# List All Tags

```
$ git tag
v0.2.1.RELEASE-commit
```

# Creating a Tag

```
$ git tag v.0.3.3-RELEASE-git-remote # create local tag

$ git tag
v.0.3.3-RELEASE-git-remote
v0.2.1.RELEASE-commit

$ git push origin --tags # push to remote branch
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/taitruong/git-started-examples
 * [new tag]         v.0.3.3-RELEASE-git-remote -> v.0.3.3-RELEASE-git-remote
```

Here it tags the latest commit. It is also possible tagging a specific commit with 'git tag <tagname> <commit-hash>'.

# Removing a Tag

```
$ git tag -d v.0.3.3-RELEASE-git-remote # delete local tag
Deleted tag 'v.0.3.3-RELEASE-git-remote' (was 52c0e04)

$ git push origin --delete v.0.3.3-RELEASE-git-remote # delete remote tag
To https://github.com/taitruong/git-started-examples
 - [deleted]         v.0.3.3-RELEASE-git-remote
```
