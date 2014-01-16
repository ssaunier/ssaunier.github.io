---
layout: post
title: "Create Pull Requests from the command line within an organization"
description: "Use the command line tool `hub` with some alias of mine to easily create pull requests"
category: blog
---

**EDIT**: `hub` has been [updated](https://github.com/github/hub/releases/tag/v1.11.0) to deal with this issue. Just run ```brew update && brew upgrade hub```.


If your company uses Github on a daily basis for your private git repositories,
you may be using Pull Requests at the heart of the Github flow. You may already
know [`hub`](http://hub.github.com/), a little command-line tool to help you
use Github from your prompt.

```bash
$ brew install hub
```

Then you get the following helpers:

```bash
$ hub help
GitHub Commands:
   pull-request   Open a pull request on GitHub
   fork           Make a fork of a remote repository on GitHub and add as remote
   create         Create this repository on GitHub and add GitHub as origin
   browse         Open a GitHub page in the default browser
   compare        Open a compare page on GitHub
```

By default, the `hub pull-request` command will try to create a Pull Request
from `you:feature-branch` to `org:master`. That's a problem since you are
an organization member, you have commit access to the repo so you want the
Pull Request to be from `org:feature-branch` to `org:master`. Here is the
command which lets you do that. Add it to your [`~/.aliases`](https://github.com/ssaunier/dotfiles/blob/master/aliases#L19-L21)!

```bash
$ hub pull-request -h \
`git remote -v | grep -oE -m1 "github.com:([^/]*)" \
| sed "s/github.com://"`:`git rev-parse --abbrev-ref HEAD`
```

When run, this command will open `vim`, you will be able to type your Pull Request
title and body, `:wq`, and voilà! No need to fire up your browser anymore :)