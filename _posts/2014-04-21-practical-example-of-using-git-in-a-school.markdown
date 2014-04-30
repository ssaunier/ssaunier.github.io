---
layout: post
title: "Practical example of using git in a school"
description: "How Le Wagon harnesses the power of git remotes to serve exercises to students"
category: blog
photo: lewagon_mutinerie.jpg
---

I recently joined [Le Wagon](http://www.lewagon.org/), hosting a
[9 week-long bootcamp for entrepreneurs in Paris](http://www.lewagon.org/premiere).
My role is to teach people who have had zero programming education how to build
a web application using Ruby on Rails.

[Boris Paillard](https://www.linkedin.com/pub/boris-paillard/70/226/867), CEO of Le Wagon,
wrote the first batch of ruby exercises during last year's
holidays for the very first camp. Since then, the teachers and I worked on improving and extending these exercises.

In this article, I explain how we leverage `git` to create a great Teacher/Student
workflow. I will use [Le Wagon](http://www.lewagon.org/) as an example, but this approach applies to any computer class.

## Teachers write exercise templates

When we first ran the camp, students kept telling us they wanted a way to know if their code was correct. Exercises consisted only of a `README.md`, and solutions were
given at the end of the day through live-coding sessions. It was a bit chaotic.

How could we automatically check if a student's answer to an exercise was correct? Well, it's quite straightforward, let's use <acronym title="Test Driven Development">TDD</acronym>!
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

## How Git and Github is used by teachers

We created the exercise templates repository
and added `push` permissions to all teachers. Then it's just a simple collaboration strategy
with branches and pull-requests. Each new exercise is created/refactored inside a git branch,
and a pull-request is opened when the job is done. Here is a figure showing the
GitHub repository (`origin` in the context of teachers):

<figure class="center">
  <img class="two-third" src="/images/posts/git-teachers.png" alt="Teachers push and pull to GitHub">
  <figcaption>Teachers can push and pull to the exercise templates repo</figcaption>
</figure>

## Students work on the exercises and submit their attempt

Enter the students. Each student will **fork** the exercise templates repository to their
own GitHub account. Then,

1. Students can submit their attempts using `git push origin master`, where `origin` in this
context is the student's forked repository.
1. Students can get an update of the exercises using `git pull upstream master`, where `upstream`
is the original teachers' repository.

So each students has 2 remotes configured on their local exercises repository. Thank you, git!
Here is a figure to have a full picture over the workflow:

<figure class="center">
  <img class="two-third" src="/images/posts/git-students.png" alt="Students push their attempts">
  <figcaption>Students push their attempts and pull exercise updates</figcaption>
</figure>

## Icing on the cake, teachers get a dashboard

With all the automation in place, you can see there is a way to automatically compute a
student's attempt and create a nice dashboard of the class. That way, we can see who is
thriving and more importantly, who is having trouble.

We needed a way to be alerted when a student `push`es an attempt, so that we can run
`rake`, parse the `rake` results and store that into a database. Fortunately, GitHub
provides us with [Webhooks](https://developer.github.com/v3/repos/hooks/), so each
time a student `push`es, a `POST` request is sent to the pedagogic platform we built.
The pedagogic platform will then `clone` and `pull` the student's repository, `cd`
to the exercise repository and run `rake`. It is summarized in the following
sequence diagram:

<figure class="center">
  <img class="two-third" src="/images/posts/kitt-sequencedigram.png" alt="Pedagogic Platform is notified by GitHub via a Webhook">
  <figcaption>What happens when a student git <code>push</code>es</figcaption>
</figure>

So putting all together, we get this great Git workflow:

<figure class="center">
  <img class="two-third" src="/images/posts/git-kitt.png" alt="Git is powerful">
  <figcaption>Students push, GitHub notifies, Pedagogic Platform grades</figcaption>
</figure>

## Conclusion: Git is awesome!

Using [git remotes](http://git-scm.com/book/en/Git-Basics-Working-with-Remotes), we managed
to create an efficient workflow where teachers can easily update and add new exercises,
while students are working. The Webhook provided by GitHub are priceless, thanks guys!

Follow [Le Wagon](https://twitter.com/intent/follow?screen_name=Lewagonparis), we have great stuff coming. If you are in Paris in July/August, or October/November, there are still some seats left for our <a href="http://www.lewagon.org/premiere">9 week-long bootcamp</a>, they won't last long!

