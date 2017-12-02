---
layout: post
title: "Speed up the AWS Elastic Beanstalk deployment of a Ruby on Rails application"
description: "How to cache artifacts from bundle install and rake assets:precompile commands"
category: blog
---

At [Le Wagon](https://www.lewagon.com), we use [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) to host the pedagogic platform used by hundrends of students across the world. This is a Rails 5.1 application using the latest niceties like ActionCable, [Webpacker](https://github.com/rails/webpacker) and Sidekiq. If you read [my previous article](/blog/2015/10/20/aws-elastic-beanstalk-commands-for-rails.html), you know that the command to deploy is:

```bash
eb deploy
```

We have two instances behind a load balancer, with a one-at-a-time deployment strategy. When deploying, a first instance unregisters itself from the Load Balancer and stops serving production traffic. Then the deployer will push the new code and triggers a series of script. If you connect to one of the EC2 instance, you can have a look at them:

```bash
eb ssh
ls -l /opt/elasticbeanstalk/hooks/appdeploy/pre/
-rwxr-xr-x 01_unzip.rb
-rwxr-xr-x 02_setup_envvars.sh
-rwxr-xr-x 03_mute_sidekiq.sh
-rwxrwxr-x 101_yarn_setup.sh
-rwxrwxr-x 102_yarn_install.sh
-rwxr-xr-x 10_bundle_install.sh
-rwxr-xr-x 11_asset_compilation.sh
-rwxr-xr-x 12_db_migration.sh
-rwxr-xr-x 13_test_for_puma.sh
```

The `03_mute_sidekiq.sh`, `101_yarn_setup.sh` and `102_yarn_install.sh` are three tasks I added to support Sidekiq and Webpacker. The other scripts are the one installed by default when you create a new Ruby application on AWS Elastic Beanstalk.

Overall, AWS EB works quite well. But the documentation is scarce / uncomplete / outdated and we are miles away from the speed of a `git push heroku master`. One reason is that by default, AWS EB **won't cache and re-use gems** between two deployments. This means that for every `eb deploy`, you wait for a **full** `bundle install` to run, download every gems and install them. That's pretty long.

Hopefully, thanks to the hooks sytem, you can write your own. That's what I did. The idea is to copy the downloaded gems somewhere on the machine, and make sure to re-use that on the next deployments before running the `bundle install`. You can also use this techniques for **assets** and reduce the time that `rake assets:precompile` needs to run.

Here is the script you can take and drop in your `.ebextensions` folder as a `cache.config` file:

<script src="https://gist.github.com/ssaunier/67b86f628154ede29f16b74608a97240.js"></script>

This script is caching the content of the three following folders:

1. `vendor/bundle` which is built by `bundle install` command
1. `node_modules` which is built by the `yarn install` command (Webpacker)
1. `tmp/cache/assets` which stores intermediate Sprocket files built by the `rake assets:precompile` command

Go ahead and add this to your Rails app hosted on Elastic Beanstalk! We dropped the deployment time on two EC2 machines from 20+ minutes to ~8 minutes!
