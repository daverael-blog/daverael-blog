---
title: "Prelude to How"
date: 2023-05-11T20:28:20-06:00
tags: ["software", "guiding-principles"]
---

This post is the fourth in a series on writing software.

[The first](/posts/guiding) introduced the most fundamental of the principles of concern in creating software. That of `purpose`.

[The second](/posts/solution) took that idea further. It spoke of turning a problem into a vision for a `solution`.

[The third](/posts/what) moved from the question of `why?` into that of `what?`. It emphasized that software is not always the solution and we do well to consider other answers, but when software is the best answer, the we `deliver`.

With a clear picture of what problem we're trying to solve, who it's for, what they want to accomplish, and how we'll know when we're successful, we have answers to `why?` in hand. Having decided we'll build software and with a vision in mind for what it should look like and what it should do, we've addressed `what?`. Still, we have many decisions to make.

Jumping into implemenation would be premature.

Among the decisions the software designer needs to make are:

- How will you know it works?
- How will you know it continues to work as it operates?
- How will you know it continues to work when you make changes?
- How will you build artifacts useful for running hte software?
- What state must be maintained?
- What state should be volatile and what should be durable?
- Does the problem/solution justify a distributed system or should it be a single application?
- What type of user interface is necessary (if any)?
- What concerns do you have about privacy?
- What concerns do you have about security?
- How will you know if there are problems?
- How will you know what happened if you know there are problems?
- How will you know it performs adequately?
- What tech stack should you use?
- Should you use a single, standard tech stack or are there subsystem with subproblems best served by differing approaches?
- Where should your software run?
- How will you deploy it where it needs to go?
- How will you create the infrastructure on which to deploy it so that you have a where it needs to go to deploy it to where it needs to go?
- How will you know your deployments are successful?
- What will you do if your deployments are not successful?
- Can your system tolerate downtime?
- How will you know how much load your system can bear?
- How will you know how much capacity you need to create?
- How will you bring these things into reality?
- What nonfunctionctional requirements will drive perception of the stability of the system?
- What service level agreeements have been made explicitly?
- What service level agreeements have not been made explicitly with other parties, but should be articulated internally to have thresholds for success and stability?
- How will you measure what matters and compare it against imprtant metrics?

As you can see, there are many things that must get consideration when one builds software. There's also likely a lot more 

Developers don't often think about, let alone articulate, these many decision points explicitly.

Still, every action taken when starting to construct software is addressing these concerns, whether thought about explicitly or not.

Almost all of these questions are expressed with the question `how?` and those that aren't articulated that way could be reworked into that wording.

It's the how where the software engineer gets into the parts of the craft that are unique to being a technical worker in information techology. The first step in implementation is in deciding these fundamental answers to the design of the product.

The smart engineer has answers to these questions, or at least direction, before writing a line of code.
