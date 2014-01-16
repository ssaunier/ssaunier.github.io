---
layout: post
title: "Display current git branch in your web app"
description: "Visual help for your rails app to figure out if you are in development or production mode"
category: blog
---

<img class="inline pull-right" src="/images/posts/facepalm.jpg" alt="Computer Face Palm" />
If you work with Ruby on Rails on a web application already released to production,
you surely have one browser tab on the prod app, and another one on `localhost:3000`.
You may already have tried to reproduce a bug wondering why the `debugger` command won't
work just to realize five long minutes later that you were on the `production` tab...

Here is a trick to display the current `git` branch you are working on in your development
browser tab. That way, your eye will catch on the orange badge and figure out quicker where
you are. Plus you get a nice benefit of knowing the feature context.


```ruby
# app/helpers/application_helper.rb
module ApplicationHelper
  def branch_info
    branch_name = `git rev-parse --abbrev-ref HEAD`
    content_tag :li do
      content_tag :p, :class => "navbar-text" do
        content_tag :span, branch_name, :class => "label label-warning"
      end
    end
  end
end
```

This helper is ready to be used in your bootstrap top bar menu, inside a `ul.nav`.

```html
<!-- app/views/layout/application.html.erb -->
<div class="nav-collapse collapse">
  <ul class="nav">
    <%= branch_info if Rails.env.development? %>
  </ul>
</div>
```

You will get a nice orange badge in the top bar of your web application reminding
you that:

1. You are in your `development` environment.
2. Which feature branch you are working on.

Before coming with the shell approach, I was previously using the gem [`grit`](https://github.com/mojombo/grit):

```ruby
repository = Grit::Repo.new(Rails.root)
branch_name = repository.head.name
```

But I figured out that I just needed the branch name, so including a gem was a
bit of overhead!

What about you? Which trick do you use in the `development` environment? Have you tried
[`rails-footnotes`](https://github.com/josevalim/rails-footnotes)?