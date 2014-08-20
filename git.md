# Git and GitHub

Key commands:

```shell
$ git clone <repo URL>
$ git init
$ git add .
$ git commit -am "Commit message"
$ git push
$ git branch
$ git checkout
$ git status
$ git log
$ git diff
```

Your .gitignore file contains a simple list of files you don't want Git to place under version control. You can't ignore your .gitignore file, for obvious reasons.

## Git model

Git's working model looks like this:

```
----------
| server |
----------
  ^
  |  git push
  |
----------------------------
| staging area on computer |
----------------------------
  ^
  |  git add && git commit
  |
--------------------
| local filesystem |
--------------------
```

## Collaboration

Collaborators have full access to a repo, including the ability to delete code, so only add people you implicitly trust their ability and intentions!

# Fork and pull

If you don't fully trust your collaborators, forking and pulling is a better option.

```
----------
| master |
----------
  |
  |
  |\  <--- fork
  | \  
  |  \
  |   \
  |    \
  |    |
  |   (changes to fork)
  |    |
  |   /
  |  /
  | /
  |/
  |   <--- pull request && merge
  |
  |
  v
--------------
| new master |
-------------- 

```