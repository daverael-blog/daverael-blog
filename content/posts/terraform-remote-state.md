---
title: "Terraform Remote State"
date: 2023-06-30T08:04:49-06:00
tags: ["software", "infrastructure"]
---

In [my last post](/posts/terraform-start), I gave a little introduction on working with Terraform.

Because it was just an introduction, it focused setting up a repository, writing HCL, and the mechanics of executing Terraform.

It did was not concerned with proper state management.

Here, we'll put a little thought into something we can work with in a more rigorous manner than just playing around.

By using a backend for storing, accessing, and manipulating state, we begin to work with infrastructure with a record beyond just the workstation. We enable working with a team and managing our infrastructure, rather than just playing with it.

To start with state management, we'll have to [select a backend to use and configure it with a backend block](https://developer.hashicorp.com/terraform/language/settings/backends/configuration). There are many choices available for backends you can use and you could implement your own if you really wanted to. Generally, you'll want to choose something close to where you deploy and how you deploy. For example, if you're using AWS for where you deploy and/or some of the AWS DevOps tools for deployment, you'll likely want to store your state in a S3 bucket. If Azure using Azure DevOps and/or GitHub Actions, you'll like want to use Azure Storage. For the purposes of this post, we'll use Azure Storage as the example.

To use Azure Storage, you'll need to have a subscription in Azure in which you can create resources. In that subscription, you'll need to create a Storage account and a blob container. The need for such resources presents a bit of a [chicken/egg problem](https://en.wikipedia.org/wiki/Chicken_or_the_egg). In order to create resources using Terraform, you need to have resources. This can be resolved by either just creating starter resources that exist to store your state manually, using the likes of the [Azure Command-Line Interface](https://github.com/Azure/azure-cli) or Azure portal or by creating those resources with Terraform using local state and then importing that local state into a backend in that newly created blob container. The latter is a better approach if you want to have a policy where everything in your subscription is created via infrastucture as code, which is a smart way to operate. For our purposes here, I'll just create resources using the Azure CLI.

To do this, you'll need to have the Azure CLI installed (best done with the likes of apt, Homebrew, Chocolatey, etc.) and logged in (via `az login`).

To create a blob container, we have to first create a storage account in which to put it and to create a storage account, we have to first create a resource group in which to put it. We can do all three with something like:

``` sh
az group create --location "Central US" --name rael-demo-infrastructure-state
az storage account create --name raeldemoinfrastate --resource-group rael-demo-infrastructure-state
az storage container create --name terraformstate --account-name raeldemoinfrastate
```

Note: In a real environment you'd likely want to create keyless storage accounts and authenticate to them with organiztion credentials and would be much more careful about how you set this up. This is for demonstration only.

With a blob container now existing, we're able to set up a repository and start to work.

First, a new repository from nothing:

``` sh
mkdir sample-infrastrcture-azure-state
cd sample-infrastrcture-azure-state
git init
```

Then, let's add a [gitignore](https://git-scm.com/docs/gitignore) file, approriate for Terraform, so we'll not have inordinate noise in Git status and we'll not risk committing files we don't want in our repository. This downloads a file from offered and maintained by GitHub as a default starting point for a gitignore for Terraform.

``` sh
curl --output .gitignore https://raw.githubusercontent.com/github/gitignore/main/Terraform.gitignore
git add .gitignore
git commit -m "Add gitignore to reduce noise and increase safety"
```

With that overhead out of the way, we're ready to start doing the Terraform things. The first of those will be to set up a provider. To start on that, we'll first need to create a file in which to configure the provider. Let's just make a file called `main.tf`.

``` sh
touch main.tf
```

In this new file, we'll set up our backend. We do this with the `backend` block within the `terraform` block.

``` terraform
terraform {
  backend "azurerm" {
    resource_group_name  = "rael-demo-infrastructure-state"
    storage_account_name = "raeldemoinfrastate"
    container_name       = "terraformstate"
    key                  = "demo.terraform.tfstate"
  }
}
```

Note that different backends will require different parameters. Now, initialize Terraform to set up our backend.

``` sh
terraform init
```

The output should look like:

``` sh
Initializing the backend...

Successfully configured the backend "azurerm"! Terraform will automatically
use this backend unless the backend configuration changes.

Initializing provider plugins...

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

You should now see that Terraform has created a new blob in your container that is the state file. When you execute terraform commands, you will be dealing with that state that's in a safe, remote location, off your machine. As you start working with state, you'll realize that the blob gets locked every time you are operating against it and unlocked when you complete. In this way, you and your teammates can share this state without running the risk of corruption. If the state is locked by someone else when you try to execute, you'll just wait until they complete.

You'll want to commit this backend configuration to your repository and push to your shared, central repository to make sure your teammates are using the same configuration. If you want to have different state files, such as for different environments, you can omit some of the parameters to the provider configuraiton and instead pass them as parameters to `terraform init` with the `-backend-config` parameter option.

This has really only scratched the surface of working with Terraform state, but hopefully give you a look at the idea, why this matters, and how you can execute on using the concept.

