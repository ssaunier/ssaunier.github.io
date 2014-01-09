---
layout: post
title: "Quickly hack an algorithm with a TDD template"
description: "Next time you have a phone interview, be ready with this template of mine"
category: blog
---

A few days ago, I was on a phone interview, with my screen shared. I was asked
to write an algorithm, and I had 20 minutes. I decided to go with Ruby, and TDD.
But I spent 15 minutes to setup correctly `guard` and `rspec` before actually
working on the algorithm. With these instructions, I would have had 19 minutes.

{% highlight bash %}
$ mkdir lab && cd $_
$ echo "source 'https://rubygems.org'\ngem 'guard-rspec'" > Gemfile
$ bundle install
$ bundle exec guard init
$ echo "--color" > .rspec
$ mkdir -p lib spec/lib
{% endhighlight %}

The interview problem was:

{% highlight bash %}
Devise a function that takes an integer and returns a string that
is the decimal representation of that number grouped by commas
after every 3 digits. For instance:
1 -> "1"
1000 -> "1,000"
57154351 -> "57,154,351"
{% endhighlight %}

Let's code this feature in an `IntegerFormatter` class, so let's create our files:

{% highlight bash %}
$ echo "class IntegerFormatter\nend" > lib/integer_formatter.rb
$ echo "require './lib/integer_formatter'\n" >> spec/lib/integer_formatter_spec.rb
$ echo "describe IntegerFormatter do\nend" >> spec/lib/integer_formatter_spec.rb
{% endhighlight %}

I could also just have `touch`ed both files and add some code afterwards.

Open the `lab` folder in your favorite text editor, then run `guard`.
It will automatically relaunch your specs on each
file change. Very handy for the red / green cycle of TDD.

{% highlight bash %}
$ bundle exec guard
{% endhighlight %}

Now go hack your solution! FWIW, here's [mine](https://gist.github.com/ssaunier/8336988) :)

**TL;DR** - I created [`ssaunier/lab`](https://github.com/ssaunier/lab#lock-and-load) to start hacking even faster!
