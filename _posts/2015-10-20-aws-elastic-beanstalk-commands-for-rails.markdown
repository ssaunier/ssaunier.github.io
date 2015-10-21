---
layout: post
redirect_from:
  -/blog/2015/10/20/aws-elastic-banstalk-commands-for-rails.html
title: "AWS Elastic Beanstalk commands for Rails"
description: "Learn all the Heroku/Beanstalk command line equivalents"
category: blog
---

As a Rails developer, my guess is that you've been using Heroku a lot. At some point, you want
to try AWS Elastic Beanstalk, a PaaS built on top of AWS. I did, but I was a bit lost
in the documentation trying to reproduce the well-known Heroku commands. Here's what
I've found (please add a comment if you have more tips to share!).

## Tools

In order to use the `heroku` command in your terminal, you need to install the [Heroku Toolbelt](https://toolbelt.heroku.com),
on OSX it would be as simple as running:

```bash
$ brew install heroku
```

AWS Elastic Beanstalk has its own command, `eb`, which you can install on OSX with:

```bash
$ brew install aws-elasticbeanstalk
```

(There a Python dependency for this `eb` command to work)

## Environment variables

On Heroku, you can list, get and set `ENV` variables with these commands:

```bash
$ heroku config
$ heroku config:set MY_VARIABLE=my_value
```

With Beanstalk, you need to run:

```bash
$ eb printenv
$ eb setenv MY_VARIABLE=my_value
```

## Deployment

Deploying to Heroku is a joy. Just setup a git remote, and then you can run:

```bash
$ git push heroku master
```

The prompt will give you back the focus as soon as the deployment is done. That's great,
you'll see all the steps (`bundle`, `rake assets:precompile`, etc.) running and can
quickly identify a problem if something goes wrong (usually, there's an issue with the
assets). Also, once the command is done, you **know** that your app is being restarted
and the new version available. If you want zero-downtime deployment, you can try
the [preboot](https://devcenter.heroku.com/articles/preboot) feature on your app (in that
case, you don't know exactly when the new version is available).

On AWS Elastic Beanstalk, it's a bit different. You have to run:

```bash
$ eb deploy
```

The thing is, you don't get nice messages, you don't see the progress, and you don't really
know why something is failing. For instance, I have an issue with the `bundle install` step,
the machine on which it was running was too small (not enough RAM / no swap). How can you
get a better understanding of how your deployment is going?

Here's what I do, I open a new tab, and run:

```bash
$ eb ssh
ssh> tail -f /var/log/eb-activity.log
```

This way I can *follow* the deployment.

## Database migration

On Heroku, after a deploy, you need to run the database migrations.

```bash
$ heroku run rake db:migrate
```

With AWS Elastic Beanstalk, this command will be run automatically, so nothing
to do after the `eb deploy`.

## Rails console

On Heroku, you can easily launch a rails console with:

````bash
$ heroku run rails c
```

On AWS Beanstalk, you need to first ssh to the server:

```bash
$ eb ssh
ssh> cd /var/app/current && bin/rails c
```

## Rails logs

On Heroku, you can tail all the application logs to see incoming requests
and 500 with

```bash
$ heroku logs --tail
```

On AWS Beanstalk, you need to SSH first again:

```bash
$ eb ssh
ssh> cd /var/app/current && tail -f log/*.log
```

## Conclusion

AWS Beanstalk can auto-scale where Heroku won't (you have to manually run `heroku ps:scale` commands),
but overall, from my experience, I think that Heroku is till ahead of AWS Beanstalk when it comes to fully
trusting a PaaS with my app.

