---
layout: post
title: "Why you should learn Jekyll today"
description: "Use Jekyll to build your next static website (Portfolio or Landing Page)"
category: blog
---

**TL;DR -** Jekyll is great for Portfolios and Landing pages.
And [Faggy Dux](http://www.faggydux.fr) just <strike>rocks</strike> electro-funks.


<img class="inline pull-right" src="/images/posts/jekyll.jpg" alt="Jekyll" />
As a web software engineer, I sometimes get requests from Friends and Family to
help them build a webpage. Usually, they can be translated to landing page to test
a new idea, or portfolio to showcase their work. My goal is to be the most
efficient possible when saying *yes* to these requests, in other words I need to
minimize the time spent to build **and** maintain those projects. This post will
explain how to leverage Jekyll to build a one-page portfolio.

## Idea and Specification

My brother is a music producer student. In the last two years, he worked on different
projects. Next year, School will be over, so basically he will need to find clients
to work for, as a *freelance*. Thus he must let the world know of his talent, and
one good mean is to provide a *web portfolio*.

So after some brainstorming we decided to go for a one page website showcasing videos
and tracks, and came up with this basic mockup:

<div class="center">
  <a href="http://www.faggydux.fr">
    <img alt="Basic mockup of Faggy Dux" src="/images/posts/mockup_faggy_dux.png" />
  </a>
</div>

## Implementation

As you can see in the mockup, nothing fancy here. For the sake of maintainability,
I want my brother to add more videos and tracks without me knowing. One idea which
comes to mind is to use a CMS and a database to store those videos and tracks. Why
don't I setup a Wordpress and go from there?

I felt like Wordpress is too big for the requirement. After all, it's just a *one-page*
can which can be built with HTML in a couple of hours. But the one big HTML file would not be
really easy to handle for my non-technical brother. He might fear to break something
while editing this huge chunk of code.

Here comes [Jekyll](http://jekyllrb.com/). Jekyll is great to generate static blog-aware websites.
Let's ditch the blog part for now,
all we need is two models, videos and sounds, and let Jekyll know about
them. Fortunately, Jekyll has [categories](http://jekyllrb.com/docs/variables/), which
allow you to **tag** information. Let's name them `video` and `remix`.

Now we can simply implement a `for` loop to display all videos, and all remixes.

```html
{% raw %}
{% for video in site.categories.videos %}
  <!-- Display the video -->
{% endfor %}

{% for remix in site.categories.remix %}
  <!-- Display the remix -->
{% endfor %}
{% endraw %}
```

One more requirement is to display both Vimeo **and** Youtube videos. Leveraging Jekyll
posts metadata, let's introduce two post variables, `youtube_id` and `vimeo_id`, and render
different HTML blocks based on what the post contains.

```html
{% raw %}
{% if video.youtube_id %}
  <iframe src="//www.youtube.com/embed/{{ video.youtube_id }}"
          frameborder="0" allowfullscreen></iframe>
{% else %}
  {% if video.vimeo_id %}
    <iframe src="//player.vimeo.com/video/{{ video.vimeo_id }}"
            frameborder="0" allowfullscreen></iframe>
  {% endif %}
{% endif %}
{% endraw %}
```

Then my brother just has to create a new file in [`_posts`](https://github.com/yoo76/yoo76.github.io/tree/master/_posts)
to add a new video, like this:

<div class="highlight">
<pre><code>
---
categories: video
youtube_id: <strong><a href="https://www.youtube.com/watch?v=0QsmwNNp2_k">0QsmwNNp2_k</a></strong>
name: Audi Talents Awards - Symbiosis
---

Our vision of the first corporate video from the Audi Talent Awards 2013.
Our purposal wasn't selected, despite of hard work and many efforts.
But we plan to go further for the next edition!</code></pre>
</div>

Dead easy, right? You can view the full source code for including
[remixes](https://github.com/yoo76/yoo76.github.io/blob/master/_includes/remixes.html#L37)
and [videos](https://github.com/yoo76/yoo76.github.io/blob/master/_includes/music_for_picture.html#L14) on Github.

## Editing

You may argue that this is still a bit more complicated than giving access to a Wordpress back-office,
and you may be right. Creating a new file in `_posts` (following the date name convention) may be
harder than clicking on "New post", testing locally with
[`start_jekyll.sh`](https://github.com/yoo76/yoo76.github.io/blob/master/start_jekyll.sh)
more mysterious than hitting "Preview". And using a text-editor instead of a web form to create content
might be reserved to hackers.

Those points are valid, and Jekyll may not be suited for everybody in its raw form. That's why people
try to make it more user-friendly, like [Prose.io](http://prose.io/#about)
([Read this](http://www.markwk.com/2013/04/prose.io-content-editor-for-jekyll-sites.html))
or [Spinto](http://www.spintoapp.com/). I really like the power of Markdown and static websites.
It feels like the right amount of technology for delivering portfolio and landing pages.

## Publishing

Curious about the result of this little project? Check out [Faggy Dux](http://www.faggydux.fr)
and follow [@faggydux](https://twitter.com/faggydux), especially if you're into Electro-Funk music!
It is hosted on [Github Pages](http://pages.github.com/), and my brother uses
[Github for Mac](http://mac.github.com/) instead of `git push`. He really likes this workflow. There is
no command line involved, he just opens Sublime Text, make changes, create new posts, and deploy with
hitting the `Sync` button of Github for Mac. Dead easy. No FTP, no database. He could even directly
use Github as a web based editor, so Sublime isn't event required.

## Conclusion

Next time you come up with an idea, try it out with a landing page. You can use
[LaunchRock](http://launchrock.co/), [Unbounce](http://unbounce.com/) or setup your own
with Jekyll (you'll have more control on the code). For the latter, you can fork
[quartzmo/email-land-page](https://github.com/quartzmo/email-landing-page) to start
from a solid base, and deploy it to Github Pages or Heroku.

Happy hacking!