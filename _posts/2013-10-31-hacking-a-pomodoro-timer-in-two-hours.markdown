---
layout: post
title: "Hacking a Pomodoro Timer in two hours"
description: "Leverage Github Pages hosting, and build favicons with canvas"
category: blog
---

**TL;DR -** This [pomodoro timer](http://sebastien.saunier.me/colortimer/) uses colors
from Green to Black to tell you that time flies.


My friend [@Julien_Mottet](https://twitter.com/Julien_Mottet) told me he would
like a tool to help him focus on his work and give him very short deadlines to
commit to. But he didn't want an obnoxious countdown timer, too much distracting.

I decided to build it and it took me only 2 hours to get it live in "production".
At zero cost. Without any sysadmin work.

## Idea

<img class="inline pull-right" src="/images/posts/pomodoro.jpg" alt="Pomodoro Timer" />


The [Pomodoro Technique](https://en.wikipedia.org/wiki/Pomodoro_Technique)
says you should work in batches of 25 minutes, and then take a 5 minutes break.
The idea is to get a visual feedback of where we are in this 25-minute interval
with *color*. I chose 6 colors from [Flat UI Colors](http://flatuicolors.com/),
Emerald, Peter River, Sun Flower, Pumpkin, Pomegranate and Black.

Having those, we want to use Emerald for the first half of 25 minutes, then Peter River
for the first half of the remaining, etc... We should see the current color twice as
less time than the color before. The last 45 seconds should be black.

## One HTML file app

The app is a simple HTML ([ssaunier/colortimer](https://github.com/ssaunier/colortimer) on GitHub),
with some javascript to handle the dynamic change of background colors.

Dependencies are voluntarily restricted to:

1. `cdnjs.cloudflare.com/ajax/libs/zepto/1.0/zepto.min.js` for easy DOM maniupation
2. `fonts.googleapis.com/css?family=Lato:100` for pretty fonts :)

I also implemented a simple feature: matching the favicon color to the body background. This
is a pattern we see more and more as it gives you feedback of a tab status without focusing
on this particular tab. [Buffer](http://bufferapp.com/) does that using [tommoor/tinycon](https://github.com/tommoor/tinycon).

```javascript
var canvas = document.createElement("canvas");
var size = 16 * (window.devicePixelRatio || 1);
canvas.width = size;
canvas.height = size;
var context = canvas.getContext("2d");
context.arc(size / 2, size / 2, size / 2, 0, 2 * Math.PI, false);
context.fillStyle = $('body').css('background-color');
context.fill();

// Having in <head />: <link id="favicon" href="" rel="icon" type="image/x-icon">
$('#favicon').attr('href', canvas.toDataURL(0)).remove().appendTo('head');
```

Please note that the app is fully responsive thanks to the use of [`vh`](http://dev.opera.com/articles/view/css-viewport-units/) units.

## Hosting

[Github Pages](http://pages.github.com/) is a great hosting solution for
these little hacks. Just create a `gh-pages` branch from `master`, and push it.
Keeping it in sync with `master` will be equivalent to deploying a new version.

```bash
$ git checkout master
$ git checkout -b gh-pages
$ git push origin gh-pages
```

This way you get a nice `github.io` or use your own [CNAME](https://help.github.com/articles/setting-up-a-custom-domain-with-pages).
Try it here:
[sebastien.saunier.me/colortimer](http://sebastien.saunier.me/colortimer/) !
