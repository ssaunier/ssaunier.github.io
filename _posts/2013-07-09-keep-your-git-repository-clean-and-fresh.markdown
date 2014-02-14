---
layout: post
title: "Keep your git repository clean and fresh"
description: "Learn how to quickly remove merged branches from your local repository once your pull request has been approved on GitHub"
category: blog
---

If you use GitHub, follow the [GitHub flow](http://scottchacon.com/2011/08/31/github-flow.html),
and use the pull request system to review code, you must
be creating a lot of git branches. From GitHub, you can merge a branch to `master` and
even [delete](https://github.com/blog/1335-tidying-up-after-pull-requests) it from `origin` once merged.

But locally the branch is still here and if you don't pay attention you will accumulate
a mess, that is a bunch of merged branches. And I really hate getting 10+ items
when running `git branch`.

Once I receive a confirmation email from GitHub that a pull request I opened has been merged
by a fellow coworker, I have this simple routine I run in 4 steps:

```bash
$ git m      # Go back to master,
$ git fo     # and fetch origin,
$ git mom    # merge origin/master into local master branch.
$ git sweep  # At last, clean up merged branches and prune.
```

All this magic happens because of some aliases:

```bash
[alias]
  m = checkout master
  fo = fetch origin
  mom = merge origin master
  sweep = !git branch --merged master | grep -v 'master$' | xargs git branch -d\
          && git remote prune origin
```

You can review my full [`.gitconfig`](https://github.com/ssaunier/dotfiles/blob/master/gitconfig)
on GitHub for more details.