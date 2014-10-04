---
layout: post
title: "On testing"
description: "My notes on Sandi Metz' great tech talk, The Magic Tricks of Testing"
category: blog
---

A while ago, I watched [The Magic Tricks of Testing](https://www.youtube.com/watch?v=URSWYvyc42M)
by [Sandi Metz](https://twitter.com/sandimetz) and had an epiphany. This was the perfect
approach at testing because it answered perfectly the question I always have:

> What should I test exactly?

This blog note are my notes about that talk, for future reference. It only deals
with **unit tests**, not integration test.
You can check the [slides on Speaker Deck](https://speakerdeck.com/skmetz/magic-tricks-of-testing-railsconf).

## Goals of unit tests

They must be:

1. **<span title="(minutieux, rigoureux)">Thorough</span>**: must prove completely that
the *single* object under test is behaving correctly
2. **Stable**: must not break with an implementation detail change
3. **Fast**: minimize development feedback loop
4. **Few**: the tests must be the most parsimonious proofs

## Focus on messages

Messages are from three origins for an object under test: Incoming, Sent to Self and Outgoing.
Those messages come in two flavors:

1. **Query**: Return something and Change nothing (no side effect)
2. **Command**: Return nothing and Change something

<div class="spacer without-border">

</div>

<div>
  <figure class="center pull-left">
    <img src="/images/posts/object_under_test.jpg" alt="Object under test Diagram" width="330">
    <figcaption>Object under test diagram, by Sandi Metz</figcaption>
  </figure>

  <figure class="center pull-right">
    <img src="/images/posts/unit_testing_minimalist_grid.jpg" alt="Unit testing minimalist grid"  width="330">
    <figcaption>Unit Testing Minimalist Grid, by Sandi Metz</figcaption>
  </figure>
</div>

<div class="clear">

</div>

<div class="spacer without-border">

</div>

## Rules of testing

1. Test **incoming query messages** by making assertions about **what they send back**.
Test only the interface, not the implementation (do not look inside the capsule)
2. Test **incoming command messages** by making assertions about **direct public side effects**.
3. **Do not test private methods**, do not make assertions about their result, do not
expect to send them
4. **Do not test outgoing query messages**
5. **Expect** to <span style="text-decoration:underline; font-weight: bold">send</span> **outgoing command messages**

Rule 3 can be broken if you deal with a complex private algorithm that you need to TDD. The tests
have value early on, but as time passes they prevent people from refactoring/improving your code.
So just delete them.

Rule 5 can be broken if side effects are stable and cheap (close, not far away).

## About mocks

A mock is a fake of the real object, and it's the developer's job to make sure there
is no API drift, to ensure test doubles (mocks) stay in sync with the API.

Test frameworks can help you do that. For instance, with [`rspec-mocks`](https://github.com/rspec/rspec-mocks), you have:

```ruby
expect(Person).to receive(:find).and_call_original
Person.find # => executes the original find method and returns the result
```

Source: [Readme](https://github.com/rspec/rspec-mocks#delegating-to-the-original-implementation).

Here is a small experiment I setup to test `rspec-mocks` and
[`rspec-fire`](https://github.com/xaviershay/rspec-fire) way of preventing
stubbing nonexistant methods (API drift). You can see that `rspec-fire` is more explicit
at telling you that you stubbed an nonexistant method than `rspec-mock`.

<script src="https://gist.github.com/ssaunier/6864350.js">
</script>
