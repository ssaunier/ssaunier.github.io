---
layout: post
title: "Dump a PostgreSQL production database and mount it locally"
description: "Useful to debug with production data or avoid writing seeds"
category: blog
---

When working with a Rails app, sometimes you need to debug a production problem
regarding production data. One way to do that without touching the production
code is to dump all the data from the production database and mount it locally.

The following code works only for the PostgreSQL database vendor.


## Your database is on [Heroku](https://www.heroku.com/postgres)

**Edit**: this is now [built-in](https://devcenter.heroku.com/articles/heroku-postgresql#pg-pull) in the `heroku` command-line, thanks [@will](https://github.com/will) for pointing it out!

You can add this rake task to your code:

<script src="https://gist.github.com/ssaunier/8c88cbdce09d47581975.js"></script>

Then run it with

```bash
$ rake db:import_from_heroku
```

## Your database is on [AWS RDS](aws.amazon.com/rds/)

You probably use that if you are on [Elastic Beanstalk](http://aws.amazon.com/elasticbeanstalk/) or EC2 instances you built. With the following, I suppose you are on Mac OSX and you stored your
postgresql admin password into the Apple Keychain. You can adapt those commands to your setup
by ignoring the `PGPASSWORD` environment variable and providing manually the password to the
`pg_dump` command.

```bash
$ export PGPASSWORD=`security find-generic-password -l "postgresql://YOUR_INSTANCE.AWS_REGION.rds.amazonaws.com" -g 2>&1 | grep "password" | cut -d \" -f 2`
$ pg_dump -i -h YOUR_INSTANCE.AWS_REGION.rds.amazonaws.com -U DATABASE_USER -d DATABASE_SCHEMA -F c -b -v -f prod.dump
$ unset PGPASSWORD
$ pg_restore -i -h localhost --clean --no-acl --no-owner -d LOCAL_DATABASE_SCHEMA -v prod.dump
$ rm prod.dump
```

Enjoy!
