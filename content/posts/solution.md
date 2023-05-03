---
title: "Solution"
date: 2023-04-27T19:50:10-06:00
tags: ["software", "guiding-principles"]
---

This post is the second in a series on writing software.

[The first](/posts/guiding) introduced the most fundamental of the principles of concern in creating software. That of `purpose`.

This one is a further articulation of the meaning of purpose in software.

Like humans, software needs [a reason for being](https://en.wikipedia.org/wiki/Ikigai). Software, at least good software, is a solution to a problem. A solution without a problem is not a solution at all. It's a nomad, wandering in the desert, providing nothing to anyone.

A true solution is something that makes life better for someone in some way.

Thus, a solution requires a problem and a problem requires a stakeholder and a stakeholder requires a need.

There are some writings I find worthy of reqading again and again. Among those is the seminal post on behavior-driven development by Dan North, [Introducing BDD](https://dannorth.net/introducing-bdd/). In that post, North presented language templates for expressing needs for software. One of these would become the basis of [the Gherkin  language](https://cucumber.io/docs/gherkin/reference/) for expressing specifications and the other has become the de-facto standard for how user stories are typically expressed.

North didn't claim to have invented this sentence structure that served as a story template, but stated that it was in use within [Thoughtworks](https://www.thoughtworks.com/) when he worked there. As articulated the the `Introducing BDD` post, it looks like this:

``` text
As a [X]
I want [Y]
so that [Z]
```

His example looks like this:

``` text
Title: Customer withdraws cash**

As a customer,
I want to withdraw cash from an ATM,
so that I don't have to wait in line at the bank.
```

Diving into behavior-driven development is quite beyond where I want to go in this series about foundations and principles of software, but this sentence structure reveals a lot about what we're trying to accomplish when we create something in the realm of creationg of programs.

To have a solution, you have to have a problem. To have a problem, you have to have a human being (ideally more than one, potentially legions of them) who could have an imporvement to their experience of life with a solution to the problem at hand.

There's a reason most organization have landed on using the template shared in that post written by Dan North way back in 2006. It's because it captures exactly this idea.

To understand a problem, you need to understand who has the problem, what they need, and why they need it.

It's simple, but powerful.

Gaining this understanding is the beginning of being able to create a soluttion.

When we're armed with a problem and an understanding of what we're trying to accomplish, we start to have a purpose.

Purpose opens all possbilities.

> He who has a why to live for can bear almost any how.

-- Friedrich Nietzsche

Good software begins with problem to solve. Software that has a why to solve is the only kind that matters.
