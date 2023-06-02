---
title: "The Idea Behind Terraform"
date: 2023-06-02T07:10:01-06:00
tags: ["software", "infrastructure"]
---

I promised [here](/posts/infrastructure) to write a bit about [Terraform](https://www.terraform.io/).

Terraform, and other tools like it, is important because infrastructure is important.

Given the importance of the infrastructure on which the software will run, treating infrastructure as something worthy of all the controls, versioning, auditing, repeatability, reliability, and attention we give application code.

To that end, [HashiCorp](https://www.hashicorp.com/) created Terraform and it's a great product that fits the need well.

So, what is Terraform?

Put simply, it's an infrastructure automation tool.

It allows you to declare what infrastructure you want and it brings it to life. It allows you to make modifications in a reviewed, versioned, and persistent manner. It allows you to have the full history you're used to having in Git repositories for application source code. It allows you to have review and discussion and collaboration via pull requests like you're used to have for application source code.

It's a brilliant innovation.

Terraform makes use of the [HashiCorp Configuration Language (HCL)](https://github.com/hashicorp/hcl) as the way to specify your desired infrastructure. It's a straightforward language that read a lot like JSDN, but with sensible improvements and less ceremony. It can also be expressed with a JSON variant for great flexibility and works nicely with embedding YAML and JSON configurations.

The Terraform tool itself takes the form of an executable, available for whatever platform you use, with a rich command-line interface for planning and applying the infrastructure you specify.

It manages state in a place you specify. By default, this is in a file on your filesystem, which is useful for getting started, learning Terraform, and creating initial configurations. When you work with teams and start to move beyond the workstation and into infrastructure management in more sophisticated and better managed ways, moving that state to other places becomes necessary and Terraform supports many ways of managing this state.

The state Terraform persists tracks what you have currently deployed and is useful for making sense of what needs to change to bring your infrastructure up to date with change you make. It also helps to identify and rectify drift - changes to infrastructure that didn't happen via Terraform.

Ultimately, this leads to infrastructure in states you want and the ability to avoid and/or deal with states you don't want.

Terraform is built on a provider model such that you can use many providers for many different services and types of infrastructure to create what you need. Hashicorp offers many providers and other operations offer more, including big corporations, open source groups and individuals, and everythign in between.

There are providers freely available for the Major and lesser cloud providers you use, like Amazon Web Services and Microsoft Azure and the cloud-native products you use like Kubernetes and Helm and pretty much everything else you can imagine. You can also create custom providers for anything you need that isn't already supported.

It's a powerful and useful tool.

It doesn't solve everything, though, and you still need to be smart about controls on where you run your software and there's a learning curve associated with using Terraform and especially with managing state.

It's also not the only game in town and there are other tools you can use to accomplish the smae things. Among them are the likes of [CloudFormation for AWS](https://aws.amazon.com/cloudformation/) and [Azure Resource Manager (ARM) templates for Azure](https://azure.microsoft.com/en-us/products/arm-templates). [Pulumi](https://www.pulumi.com/) is a more direct comparison to Terraform as it crosses many services and has a privider model and has a lot in common with Terraform.

There are many reasons to choose one of these tools over another and choosing an alternative to Terraform is not necessarily a wrong choice.

Ultimately, though, using something for infrastructure as code is a necessary step for creating a modern software project and using Terraform is a good choice in moving forward on this path.
