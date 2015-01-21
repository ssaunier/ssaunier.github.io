---
layout: post
title: "You do not need a database for your contact form"
description: "Learn how to simply embed a contact form on your static website"
category: blog
---

**Edit** (Dec 22th, 2014): Brace forms closed and open sourced their product, thanks! The article has been update to reflect those changes.

I love static websites. They are simple and fast. But they are just files, so how do I setup a contact form?

## Jekyll and Github Pages

As you may know, this blog is built with [Jekyll](http://jekyllrb.com/) and hosted on [Github Pages](https://pages.github.com/) which has the several advantages:

1. I know HTML/CSS ⇒ I have full control on my website
1. Deployment via `git push`
1. Free hosting

This blog is just a bunch of files put together with `jekyll` and hosted on a folder somewhere on Amazon S3. GitHub Pages put those together with an internal webhook last time I `git push`ed the website.

## Contact form

You may already have implemented a contact form page, and it relied on a database for storing the user inputs. I do not have a database, as I said, my website is just a bunch of files. So how do I do?

### Wufoo - For power users

If you plan to get more than 5 contacts per day through this contact form page, you should sign up to [Wufoo](http://www.wufoo.com/). It's a great service which lets you build a form in seconds (with a great drag'n'drop <acronym title="User Interface">UI</acronym>), which you can embed in your webpage with an iframe. It's great if you plan to have multiple forms on your website.

### Formspree - For hackers

I did not choose Wufoo for this blog. Overkill. I stumbled upon [Formspree](http://formspree.io/), and I fell in love.

You just need to open your text editor, and paste the following code:

```html
<form action="http://formspree.io/you@email.com">
  <input type="email" name="_replyto">
  <textarea name="body"></textarea>
  <input type="submit" value="Send">
</form>
```

That's it, your form already works! It will post the form onto an external domain, `formspree.io`, and send you an email with all the form content. No database. And you can just hit reply in your mailbox to continue the conversation with your visitor.

This tool was built by the guys from [Brace](http://brace.io/), then open-sourced and hosted by [Assembly](https://assembly.com/formspree).

## Convention over Configuration

The reason why I fell in love with Brace Forms as I was reading their landing page is that everything was there, before my eyes. There was not weird configuration file to setup, some mapping to do by hand, or some database schema to figure out. It just worked. That's powerful.

Some engineers will tend to choose the most complex solutions, because they feel that it will increase their positions as expert and because it makes them feel smarter. Or maybe they feel more at ease one they have to do all the plumbing by hand. Then time passes and they have to deal with the consequences.

Do you know another tool which lives by those principles? You're right, that's [Ruby on Rails](http://www.rubyonrails.org), maybe it's time to [learn it](http://www.lewagon.org/) right? :)

**TL;DR** - Embrace simplicity.
