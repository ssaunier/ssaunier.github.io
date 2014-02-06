---
layout: post
title: "Poor man's continuous integration"
description: "Setup a git hook to run tests before each git push"
category: blog
---

When working with public Github repositories, you want to setup
[TravisCI](https://travis-ci.org/), it's free and awesome. You will get
nice comments in your pull request telling you about your build status.

If you work with private Github repositories, you will need to pay at least [$129 / month](http://travis-ci.com/plans) for the same awesomeness. If you have money, it's a no-brainer, if not it may be a bit expensive. So here is my poor man's solution.

## Git Hooks are awesome too

You can setup a git hook on your local repository so that each time you `git push`, it will run a script of your choice. Suppose you have a rails project with RSpec and Karma (for testing angular, see [my post](/blog/2014/02/04/angular--rails-with-no-fuss.html)), then the script would be:

```bash
#!/bin/sh
echo "Running RSpec"
rspec spec
spec=$?

echo "Running Karma"
rake karma:run
karma=$?

if [ $spec -eq 0 ] && [ $karma -eq 0 ]; then
  echo "\\033[32mTests are green, pushing...\\033[0;39m"
  exit 0
else
  echo "\\033[1;31mCannot push, tests are failing. Use --no-verify to force push.\\033[0;39m"
  exit 1
fi
```

If one of your test fails, the script will return `1` telling git to abort the push. This way you won't push any broken code for a pull request. You can still bypass this verification step with `git push --no-verify`.

## That looks great! I want it!

Just go to your git repository root, and create or open the `.git/hooks/pre-push` file. Copy paste my script or write your own. Mine is in shell, but you can write a ruby script as well!

The downside here (compared to TravisCI) is that every developer in the team has to create this `.git/hooks/pre-push` file on their local repository.

Hope this helps.