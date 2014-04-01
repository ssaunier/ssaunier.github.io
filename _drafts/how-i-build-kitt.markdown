---
layout: post
---

<a href="http://www.lewagon.org"><img class="inline pull-right" src="/images/posts/lewagon.jpg" alt="Le Wagon logo" width="128"/></a>
Have you heard of [Le Wagon](http://www.lewagon.org)? It's a 9-weeks dev bootcamp in Paris for at entrepreneurs who want to learn how to code (from scratch). I am one of the [teachers](http://www.lewagon.org/profs). For the second edition, the founders wanted a new version of the teaching platform, and I was in charge of its development. This is the story of **Kitt**.

## Context

During 9 weeks, every day, teachers do a lecture about a subject, and then students will practise with programming exercises. Those exercises are stored in a public Github repository, which serves as the datasource for the teaching platform. After a meeting with the other teachers, we came up with the following user stories.

## User Stories

There are 4 characters at play. The student, the teacher, the admin and the inspector (a robot checking students' solutions for the exercises). We assume there is a public Github repository containing the exercises, each exercise being a folder with a `README`, a `lib` and `tests` folders, and a `Rakefile`. The command `rake` would run the `minitest` tests and if all of them are green, then the student has passed the exercise (and can move on to the next!). There is also a private Github repository containing solutions (green tests).

The platform would then need that:

1. `[2.0h]` A teacher/admin/student can sign in to the platform with Github
1. `[1.0h]` An admin can create a camp
1. `[0.5h]` A student belongs to a given camp, and choose it after signing up
1. `[1.0h]` An admin validates that the student belongs to the camp
1. `[0.5h]` A student can view his classmates in a list
1. `[2.0h]` A student can fork the main exercises repository + setup webhook with a simple click
1. `[0.5h]` A student gets instruction to `git clone` his forked repository
1. `[1.5h]` A student can browse through the exercises
1. `[0.5h]` A student can read the exercise (`README.md`)
1. `[1.0h]` The inspector listens for github webhook `push` events
1. `[2.0h]` The inspector enqueues a rake task which test the student's solution
1. `[0.5h]` The inspector stores the rake task results
1. `[1.0h]` A student can view the exercice completion status
1. `[1.0h]` A student can view his classmates' completion status
1. `[1.0h]` An admin can promote a user to teacher
1. `[2.0h]` A teacher can view the classroom completion dashboard of all exercises
1. `[1.0h]` A student can download the solution if he passed the exercise
1. `[1.0h]` A teacher can grant access to the solution for a given exercise to all students


## Planification

I used [Planscope](https://planscope.io/) for that matter. I wrote down all the user stories above, and put a time estimation of how much work I'd need to do for each one. This added up to 20h and helped me communicate with *Le Wagon* about how much the project would cost.

## Methodology

I put up in place a continuous deployment strategy starting at the very first story (One can login with a Github account). Hence the first story not only was about creating a [Github application](https://developer.github.com/v3/oauth/#web-application-flow) for the OAuth sign in, but also setting up a Heroku app with a postgresql database.

For each story, I would start the Planscope timer, create a feature branch from `master` and start hacking. When done, I would merge the branch to `master` and deploy on Heroku. I then would check on the production application that the story was behaving correctly, stop the timer and mark the story as approved.

## In the end

It took me 25 hours to complete the project. The story where a student can download an exercise solution from a private github repository (and just this one solution) took me 4 hours instead of the 1 planned. Here is the Planscope report:

<figure class="center">
  <img src="/images/posts/planscope_kitt.png" alt="Planscope Aggregate Time logged">
  <figcaption>Planscope Aggregate time logged and daily activity</figcaption>
</figure>

## Technicalities

I used Ruby on Rails 4 to build this project. Gem-wize, I used `devise`, `omniauth-github` and `pundit` for OAuth Authentication and Authorization. I used `activeadmin` for the few administrative tasks.

I used `octokit` to interact with the Github API, and `redcarpet` with `pygmentize` and `sass-pygments-rails` to get the `README.md` displayed as on Github.

For the inspector part, I used `sucker_punch` to spawn threads at will and not rely on additional dynos for now. We'll see in the future if we need to migrate to `sidekiq` or `resque`. Oh, and `unicorn` is powering the whole, with 2 workers only (to keep some space for `sucker_punch`). So far, the hosting cost on Heroku is zero, might bump to 9$/month to get rid of the 10K rows limit on the postgresql DB.

Here is the full `Gemfile`:


```ruby
source 'https://rubygems.org'
ruby '2.1.0'

gem 'rails', '4.0.4'
gem 'sprockets', '2.11.0'  # http://stackoverflow.com/a/22394505/197944

gem 'pg'

gem 'devise'           # Authentication
gem 'omniauth-github'  # Github OAuth
gem 'pundit'           # Authorization

gem 'sucker_punch'  # Background jobs in threads
gem 'zipline'       # Zipping a directory (private github solution repo)

gem 'activeadmin', github: 'gregbell/active_admin'  # Rails 4

gem 'octokit', "3.0.0.pre"  # Github API wrapper
gem 'redcarpet'             # Markdown processor (README.md => HTML)
gem 'pygmentize'            # Syntax Highliter
gem 'sass-pygments-rails'   # CSS for pygmentize

gem 'draper'                # Decorator pattern for views
gem 'uglifier', '>= 1.3.0'
gem 'coffee-rails', '~> 4.0.0'
gem 'sass-rails'
gem 'jquery-rails'
gem 'bootstrap-sass'
gem 'font-awesome-rails'
gem 'quiet_assets', group: :development

gem 'friendly_id', '~> 5.0.0'  # Get some vanity URLs for users

gem 'unicorn'  # Great server!

# Testing
gem 'rspec-rails', group: [:development, :test]
gem 'factory_girl_rails', group: [:development, :test]
gem 'ffaker', group: [:development, :test]
gem 'letter_opener', group: [:development, :test]

# Debugging
gem 'pry-rails', group: [:development, :test]
gem 'pry-debugger', group: [:development, :test]

gem 'rails_12factor', group: :production  # Heroku needs it
gem 'dotenv-rails', group: :development   # No passwords in source control
```

For those interested by the minitest rake runner which is called by the `sucker_punch` job, here's the [gist](https://gist.github.com/ssaunier/9713130).

## Conclusion

I loved working on that project. Planscope helped me to write the user stories in the purest form without any technicality, and come up with a realistic estimate. Continuous deployment after each story was a big win as I catched setup bug (environment variables missing, etc.), which are easier to fix on the spot rather than at the end if you deploy when everything is implemented.

I'm looking forward to see the students use the platform, get some design work done, and build upon it! Thank you for reading.