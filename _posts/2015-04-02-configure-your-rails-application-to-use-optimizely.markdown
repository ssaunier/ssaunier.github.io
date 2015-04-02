---
layout: post
title: "Configure your Rails application to use Optimizely"
description: "By default, a rails app cannot be embedded in an iframe, but the Optimizely Editor uses one."
category: blog
---

I just spent a couple of hours trying to have a rails app  in the Optimizely Editor looks good,
but the CSS and images were not showing up. Apparently, if Optimizely cannot load your
website in the editor iframe, it uses a proxy, `www.optimizelyedit.com` to load the HTML.
Fine. A rails application uses helpers (`stylesheet_link_tag` and `image_tag`) for CSS ang images which generate **relative** links.
So basically it tried to download the CSS here:

```
https://www.optimizelyedit.com/assets/application-261102d8007ae1fff68a817ec78c9d94.css
```

Of course it does not exist...

By default, Rails is secure and prevents other website to load yours in an iframe.
It's done with the following HTTP header:

```
X-Frame-Options: SAMEORIGIN
```

You can override this behavior to allow Optimizely to load your site, and just them.

```ruby
# app/controllers/application_controller
class ApplicationController < ActionController::Base

  after_action :allow_optimizely_editor

  private

  def allow_optimizely_editor
    response.headers['X-Frame-Options'] = 'ALLOW-FROM https://www.optimizely.com'
  end
end
```

That's it! The Optimizely editor should now work. Oh, and of course, use [ngrok](http://ngrok.com/)
to expose your port `3000` on the Internet and run Optimizely experiment on your development environment.