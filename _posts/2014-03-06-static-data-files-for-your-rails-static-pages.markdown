---
layout: post
title: "Static data files for your Rails static pages"
description: "Avoid including a CMS engine or creating models for your Rails static pages"
category: blog
---

Suppose you have a rails application and you want to add some static pages to communicate about your product. You may want to add a **page with all your team members** on it. Where should you store this not so dynamic data?

You could copy-paste all the HTML needed to display the first team member and update the content for every other one, but this is not really <acronym title="Don't repeat yourself">DRY</acronym>.

So should you add a CMS engine to your application or create a model for your team and set up active_admin?

## YAML as your data repository

Let's <acronym title="Keep it simple, stupid!">KISS</acronym>.

Your "dynamic" team data can be put in a simple YAML file.

```yaml
# config/data/team.yml
members:
  - name: George Abitbol
    title: CEO
    photo: george_abitbol.png
  - name: Un dénommé José
    title: CFO
    photo: jose.png
```

which you make available in your rails app through an initializer and a global variable.

```ruby
# config/initializers/team.rb
TEAM = YAML.load_file("#{Rails.root}/config/data/team.yml")
```

Then you can use `TEAM` in your team view (I use [high_voltage](https://github.com/thoughtbot/high_voltage) for my static pages).

```erb
<%# app/views/pages/team.html.erb #>
<% TEAM['members'].each do |member| %>
  <div>
    <h3><%= member['name'] %></h3>
    <h4><%= member['title'] %></h4>
    <%= image_tag "team/#{member['photo']}" %>
  </div>
<% end %>
```

What do you think? How do you handle this use case?