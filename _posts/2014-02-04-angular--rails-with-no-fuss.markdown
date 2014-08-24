---
layout: post
title: "Angular + Rails with no fuss"
description: "Use Angular to structure your front-end code without turning it into a single page application"
category: blog
---

I haven't been satisfied with the way my front-end code (mainly based on JQuery) has grown with my previous rails app, so I took the time to sit and explore the great front-end framework we now have. I still like handling the routing in Rails with several pages, it feels wrong to move that logic to the javascript layer. I don't want to build a single-page application. But having some canonical way to write front-end code plus a great framework is really important.

For this article, I chose **Angular** for its approach on models (plain old javascript objects) and the two-way binding. I will describe how to add angular to your rails application, and how to setup **Karma** and **Jasmine** for testing it, making sure they play nice with **Sprockets**.

## Add Angular to your rails application

Open your `Gemfile` and add the `angular` and `angular-mocks` package. The latter will be used in the test part.

```ruby
source 'https://rails-assets.org'
gem "rails-assets-angular"
group :development, :test do
  gem 'rails-assets-angular-mocks'
end
```

You now need to require `angular` in your `app/assets/javascripts/application.js` so that it is loaded by Sprockets.
You can put this instruction just before the `require_tree .`

```js
//= require angular
```

To check if this worked, restart your `rails s`, open your browser, open its development console, type `angular` and press enter. You should see something like `[object Object]`.

As I write this post, I dogfood my own instructions with a brand new rails app, and this step is covered in [this commit](https://github.com/ssaunier/angular-rails-example/commit/f1f8a86d7c8d6c3cf5498f78a480a9f9700cb3a6).

## Generate the angular folder structure

All the angular-related code will sit in `app/assets/javascripts/angular`. Here is handy folder structure to bootstrap your app:

```bash
mkdir -p app/assets/javascripts/angular && cd $_
mkdir controllers directives filters services
touch controllers/.keep directives/.keep filters/.keep services/.keep
echo "angular.module('app', []);" > app.js
cd ../../../..
```

Refresh your browser, open your console, type `angular.module("app")` and hit enter.
You should see `[object Object]` which proves that the angular module has been correctly
created. To enable it into your application, open `app/views/layout/application.html.erb`
and update the `<html>` opening tag:

```html
<html ng-app="app">
```

Once again, you can find this step in [this commit](https://github.com/ssaunier/angular-rails-example/commit/1b425ecb324c768eb0d5be86b3d757731a48cc35).

## Setup Karma and Jasmine for testing your Angular code

As you may have noticed, we are not directly using Bower. Also we don't have a Grunt file launching a different server for serving our front-end assets, and this is a choice I made. I don't want to introduce a middleware component to bridge the front-end and the back-end code. Setting up karma was then a bit more difficult to make play it well with Sprockets (handling the assets pipleine).

Run the following commands to setup the `spec` folder structure:

```bash
mkdir -p spec/javascripts/angular && cd $_
mkdir controllers directives filters services
touch controllers/.keep directives/.keep filters/.keep services/.keep
cd ../../..
mkdir -p spec/karma/config
```

Now you must fetch some files:

1. [`package.json`](https://github.com/ssaunier/angular-rails-example/blob/master/package.json) to automatically install node packages with `npm install`
1. [`spec/karma/config/unit.js`](https://github.com/ssaunier/angular-rails-example/blob/master/spec/karma/config/unit.js) the Karma configuration file
1. [`spec/karma/application_spec.js`](https://github.com/ssaunier/angular-rails-example/blob/master/spec/karma/application_spec.js) Adding `angular-mocks` to the tested files
1. [`lib/tasks/karma.rake`](https://github.com/ssaunier/angular-rails-example/blob/master/lib/tasks/karma.rake): The key ingredient to make Karma aware of Sprockets

You can get those files with these commands:

```bash
ROOT=https://raw.githubusercontent.com/ssaunier/angular-rails-example/master/
curl $ROOT/package.json > package.json
curl $ROOT/spec/karma/config/unit.js > spec/karma/config/unit.js
curl $ROOT/spec/karma/application_spec.js > spec/karma/application_spec.js
curl $ROOT/lib/tasks/karma.rake > lib/tasks/karma.rake
npm install
echo "node_modules" >> .gitignore
mkdir -p tmp
```

You can have a look at [this commit](https://github.com/ssaunier/angular-rails-example/commit/058bc21292e1bb119b37c01ced95c8a775529b4f).

## Using Karma

All is now set up, you should be able to run two new rake tasks: `karma:start` and `karma:run`.

```bash
$ bundle exec rake karma:run
# INFO [karma]: Karma v0.10.9 server started at http://localhost:9876/
# INFO [launcher]: Starting browser PhantomJS
# WARN [watcher]: Pattern "/Users/cb/code/ssaunier/angular-rails-example/app/assets/javascripts/angular/*/*.{coffee,js}" does not match any file.
# WARN [watcher]: Pattern "/Users/cb/code/ssaunier/angular-rails-example/spec/javascripts/**/*_spec.{coffee,js}" does not match any file.
# INFO [PhantomJS 1.9.7 (Mac OS X)]: Connected on socket 7wmxSTkcCTW2TcUF9Uk0
# PhantomJS 1.9.7 (Mac OS X): Executed 0 of 0 ERROR (0.129 secs / 0 secs)
```

## Writing your first angular test

You get warnings as we don't have any code and specs in our angular folders. Let's do some TDD.

First run the guard command which will watch for file updates in angular folders.

```bash
bundle exec rake karma:start
```

This command does not return and wait. Keep an eye on it somewhere on your terminal.
First let's create the spec file for an angular service which will interface with the
Rubygems JSON API. We assume the service will have a `search(query)` method and
will return an `$http` promise.

```coffee
# $ touch spec/javascripts/angular/services/rubygems_spec.coffee
describe "Rubygems", () ->
  ROOT = "https://rubygems.org/api/v1"

  beforeEach module('app')
  httpBackend = rubygems = null

  beforeEach inject ($httpBackend, Rubygems) ->
    rubygems = Rubygems
    httpBackend = $httpBackend

  afterEach () ->
    httpBackend.verifyNoOutstandingExpectation()
    httpBackend.verifyNoOutstandingRequest()

  describe "search", () ->
    it "should return a list of gems", () ->
      httpBackend.when('GET', "#{ROOT}/search.json?query=rails").respond([])
      rubygems.search("rails").then (data) ->
        expect(data).toEqual([])
      httpBackend.flush()
```

When creating and saving this file, you'll notice in your terminal a new message:

```bash
INFO [watcher]: Changed file "/Users/cb/code/ssaunier/angular-rails-example/spec/javascripts/angular/services/rubygems_spec.coffee".
PhantomJS 1.9.7 (Mac OS X) Rubygems search should return a list of gems FAILED
  Error: [$injector:unpr] Unknown provider: RubygemsProvider <- Rubygems
```

It complains that the `Rubygems` service does not exist. Let's create it
and let's write its skeleton. We know we need to inject the `$http` module.

```coffee
# $ touch app/assets/javascripts/angular/services/rubygems.coffee
app = angular.module("app")
app.service 'Rubygems', ['$http', ($http) ->
]
```

A new error will appear in your terminal:

```bash
INFO [watcher]: Changed file "/Users/cb/code/ssaunier/angular-rails-example/app/assets/javascripts/angular/services/rubygems.coffee".
PhantomJS 1.9.7 (Mac OS X) Rubygems search should return a list of gems FAILED
  TypeError: 'undefined' is not a function (evaluating 'rubygems.search("rails")')
```

The service returns nothing, so there is no `search` method to call. Let's implement it.

```coffee
app = angular.module("app")
app.service 'Rubygems', ['$http', ($http) ->
  ROOT = "https://rubygems.org/api/v1"
  search: (query) ->
    # Return a promise: http://stackoverflow.com/a/12513509
    promise = $http.get("#{ROOT}/search.json?query=#{query}").then (response) ->
      response.data
]
```

And now the test is green! Congrats for writing your first angular service test!

## Conclusion

I should probably package this nicely as a gem. I wrote this post because I could not
find how to use Sprockets with karma tests, in the context of a light angular + rails
application (not a single page app). If you want to use the same approach in your rails app, and
clean up some front-end code, follow the [git log](https://github.com/ssaunier/angular-rails-example/commits/master)!
