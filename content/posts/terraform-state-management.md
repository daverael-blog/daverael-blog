---
title: "Terraform State Management"
date: 2023-06-07T15:10:01-06:00
tags: ["software", "infrastructure"]
---

Ok, so [you've decided you want to use Terraform](/posts/terraform-idea) for your projects's infrastrucure.

Armed with that decision, you'll need to decide what to do with the state Terraform needs to keep. The default when you run Terraform is that it uses a backend called `local`. This is fine for playing around, but it's not what you want to use for a real project, especially one with a team with multiple people working against your repositories and infrastructure. It's also not suitable for setting up and managing infrastructure from pipelines/workflows in your CI/CD tooling.

What, then, are the characteristics of a good place to store state?

It needs a few things:

- Network addressable access from multiple machines
- Locking capabilities to perform serialization of activity against it
- Durable and persistence of text documents
- Reliable atomicity of updates

Choosing state management is done by choosing a [backend](https://developer.hashicorp.com/terraform/language/settings/backends/configuration) by using a configuration block in your HCL.

There are many backends you can choose that just work. Some of the obvious examples are using storage from the cloud providers, like Amazon S3 buckets or Azure Storage Accounts. If you're already using such a provider, using the backend for their storage can be a good choice. Other options include HashiCorp's own [Terraform Cloud](https://developer.hashicorp.com/terraform/cloud-docs). It's not only a provider for storing Terraform state, but also a platform for planning and executing Terraform modules.

In most of my projects, I'm using Azure as the ultimate destination and Azure DevOps as the platform that builds, tests, and deploys my infrastructure and my application code. Because of this, I tend to use Azure Storage for my backend in Terraform and Azure DevOps pipelines as the execution engine for planning nad applying my configurations and I lean heavily on the [azurerm provider](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs). In future posts, I'll get into more detail on why this makes sense, how to set it up and how it all works.

