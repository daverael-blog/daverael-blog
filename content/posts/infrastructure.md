---
title: "Starting with Infrastructure"
date: 2023-05-25T18:43:51-06:00
tags: ["software", "infrastructure"]
---

Having decided to create software and aware of a need for a [place for it to run](/posts/place), now we start thinking about what infrastructure to create.

The infrastructure necessary depends on the problem.

You could run off and create a footprint in a datacenter and spend hundreds of thousands of dollars (or other monetary units) on hardware and setup.

You could spend similar sums in one of the big cloud providers. This might be exactly the thing to do. It could be you're dealing with a big space and you need to support lots of users and you need scale.

It could be, however, that a free website is the right thing to do.

If what you're setting up is a blog to serve static content, you could do what I've done here with this blog and set up a free site on [Cloudflare Pages](https://pages.cloudflare.com/), [Netlify](https://www.netlify.com/), or another static site hoting offering. It's very easy to use the likes of [Hugo](https://gohugo.io/) to create a website quickly, easily, and for free on these services.

If all you want is a static website, there's no need to do anything else. There are also offerings on these products for serving APIs as well. They are good choices for a great many use cases.

Eventually, though, if you're working as a software professional and doing more than the simplest of use cases, you'll need to do more and you'll need more flexibility to control your infrastructure.

When you reach this point and make this choice, you still have more decisions to make.

Unfortunately, it's a long road to get to the point of actually implementing anything because there are so many things to think about on the path of deciding where you want to deploy.

In [Stephen Covey's The 7 Habits of Highly Effective People](https://www.amazon.com/dp/B09LZ766JN/tag=devonfir-20), the second habit is `Begin With the End in Mind`. It's this habit we intend to observe.

Our desired end state is to wind up with running software serving our users or otherwise fulfilling the purpose for which it was designed. To serve getting to this state, we're going to need to set up automation that builds and tests the code we'll create. We need automation to create artifacts we can deploy. We need automation to deploy these artifacts.

For any of this to matter, we need a place to deploy. We need infrastructure.

The type of infrastructure in which you run your software is another important decision to make. In fact, it's a series of decisions. For the moment, we'll set aside thinking about `what` to create and move in `how`.

Creation of infrastructure is a creative act in and of itself.

Creative acts need controls, iterations, reviews, structure, automation, and repeatabiilty. You likely don't want a single environment, but potentially many of them.

So you need infrastructure you can create again and again. You need infrastructure you can version.

In the simplest of cases, you likely don't need these things. For real projects in the real world, you want to treat your infrastructure the same way you treat your code. You want to use tools to build the environments where you'll run the software you create.

Luckily, tool exist for this purpose.

The most popular of the infrastructure as code tools is [Terraform](https://www.terraform.io/). It's not the only tool and whether it's best for what you want to do and create depends on what you want to do and create. Nonetheless, my next post is going to be about Terraform and what it has to offer as an example of a tool for infrastructure as code.
