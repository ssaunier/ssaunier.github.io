---
layout: post
title: "Be lazy, don't build that feature (yet)"
description: "Validate the feature/market fit with a Minimum Viable Feature using other SaaS products."
category: blog
---

**TL;DR -** Validate the feature/market fit with a <abbr title="Minimum Viable Feature">MVF</abbr> using SaaS products (APIs, Web Hooks, Concierge).

<img class="inline pull-right" src="/images/posts/hammock.jpg" alt="Hammock" />
One of the best quality a software engineer should have is laziness.
Not wanna-sleep-all-day kind of lazy, but more in a way of seeking to minimize
the amount of stuff built. He knows the best code is the one you don't write.

Product came today with a wonderful idea. They want you to build feature _X_.
According to your experience, you know some of the great ideas Product had were implemented
just to find out that users/customers aren't using it. Being _Agile_ and _Lean_, you want to
minimize the amount of noise produced by Engineering, and by noise
I mean the code barely touched by users.

I will now use [MobiCheckin](http://www.mobicheckin.com/en),
a SaaS solution for event organizers, as a successful example of using SaaS
products to validate the feature/market fit to build truly useful features.

## First feature: The Registration Form

When I started working on [MobiCheckin](http://www.mobicheckin.com/en) in March 2012,
the unique value proposition was to replace the guest list paper sheet
with an iPad at an event entrance.
The client (event organizer) would upload his Excel guest list and would sync it on his iPad
with the MobiCheckin iOS app, from there he could check-in guests. Very good.

How did the client put together his Excel list? It could be from a
<abbr title="Customer Relationship Management">CRM</abbr> of his,
or a registration form. Product wanted to follow a _Land and Expand_ strategy
to compete with other solutions involved. Thus MobiCheckin needed
a new feature, a **registration form**.

Building a user interface where one can build any web form is hard. Rough estimations told us
it would take months to have a buggy prototype, with the limited resources our startup had.
There had to be another way.

We investigated SaaS tools providing an easy way to build web forms and picked up
[Wufoo](http://www.wufoo.com). It has a great
<abbr title="What You See Is What You Get">WYSIWYG</abbr>
user interface to build forms, a well documented API and it supports Web Hooks.
As a reminder, a web hook is a HTTP request (usually `POST`) triggered by a specific event.
That way, Wufoo could tell MobiCheckin someone had just registered.

It took us a couple of days to implement and quickly delight our clients.
The <abbr title="Minimum Viable Feature">MVF</abbr> approach was a success.

Fast forwarding a couple of months later. Wufoo works great but I hear some clients
complaining they can't change _Y_ or _Z_. Moreover, the feature is being used systematically.
Time for a new iteration: this feature has become key for the product,
so we need 100% control over it. What now? Should we build A WYSIWYG web form
editor inside MobiCheckin? Again, laziness tells you that building a copycat of
what another company took years to make awesome is not a good idea.

We went for a hybrid approach using [Liquid](http://liquidmarkup.org) to get
web forms up and running quickly without a complex and long development to build <abbr title="User Interface">UI</abbr>.
And it works great now! You can see how we started with a simple integration of an
existing SaaS product (providing an API and web hooks) to pivot the feature and
build it in our tool.

## Second feature: The Mailing Campaign

We figured out that the goal of some of our clients is to maximize the number
of attendees at their events. They must have a marketing strategy around their
event, so that people register. One classic tool is the mailing campaign with
a single call to action, a link to the registration form.


For the past few months, each time our clients asked us if we could set up their
event mailing campaigns, we used another <abbr title="Minimum Viable Feature">MVF</abbr>.
This time, the great SaaS product was [Mailchimp](http://www.mailchimp.com).

Our project management team acted like a _Concierge_.
They imported the contact list, set up the campaign with the design, and once sent,
shared the report with the most tech-savvy clients. That did the job. The report
were great, as clients could see open rates and click rates. But something was
missing. Something Mailchimp, being a general tool, could not give, the
**registration rate**.

We can define the registration rate of a mailing campaign as the ratio of people
who registered to the event after clicking on the mail call to action over the
total number of campaign recipients. Our customer development revealed that this number is the most important for
our clients, so we decided to bite the bullet and implement a small mailing
campaign tool inside MobiCheckin.

Again, we went from using a <abbr title="Minimum Viable Feature">MVF</abbr> to
validate the exact feature specifications our clients would be delighted with. And I
think this is what _Lean Startup_ and _Agile_ are about.
