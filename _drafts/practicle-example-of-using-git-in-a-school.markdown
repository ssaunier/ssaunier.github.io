---
layout: post
title: "Practicle example of using git in a school"
description: ""
category: blog
photo: ecole42.jpg
---

I recently joined [Le Wagon](http://www.lewagon.org/), hosting a
[9 weeks bootcamp for entrepreneurs in Paris](http://www.lewagon.org/premiere).
My role is to teach people who have zero programming education how to build
a web application using Ruby on Rails.

[Boris](https://www.linkedin.com/pub/boris-paillard/70/226/867), CEO of Le Wagon,
wrote the first batch of ruby exercises during last Christmas
holidays for the very first camp. Since then, the teachers and I put some work
into those to refactor the exercises against some standards.

In this article, I explained how we leverarged `git` to create a great Teacher/Student
workflow. I will use Le Wagon as en example, but this applies to any computer class.

## Teachers write exercise templates

Students of the first camp told us they wanted to have some way to know if their
code was correct. Exercises consisted only of a `README.md`, and solutions were
given at the end of the day through live-coding sessions. It was a bit cahotic.

How could we set up a way to automatically check if a student's attempt to a
given exercise was correct? Well, that's quite straightfoward, let's use <acronym title="Test Driven Development">TDD</acronym>!
We agreed on an exercise template:

```
exercise_foo
|-- README.md
|-- Rakefile
|-- lib
|   `-- exercise_foo.rb
`-- spec
    |-- spec_helper.rb
    `-- exercise_foo_spec.rb
```

This way, students would just run `rake` in the `exercise_foo` folder and get an
auto-grading of their attempt.