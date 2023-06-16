---
title: "Getting Started with Terraform"
date: 2023-06-16T08:35:01-06:00
tags: ["software", "infrastructure"]
---

In getting moving with Terraform, [you'll want to have an eye on what you're going to do with your state](/posts/terraform-state-management). Proper management and planning is going to be critical for any real project.

However, getting familiar with the tool itself is a good starting point in using it. For this post, we'll think only about the fundatmentals of Terraform and just start on deploying infrastructure without worrying about state management. We'll use state, but will just use the default of storing on disk locally. This is not a safe way of managing infrastructure with a team, but it will get us started on using the tool.

To illustrate starting with Terraform, I'm just going to create a new Git repository and use it to set up a provider and create some simple resources. The provider I'll use, as an example, is a [provider for Docker](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs). This provider makes resource types available to be able to pull Docker images from registries and create containers from images. I'll just here play a little bit with creating such resources locally on my machine. I'll likely write more about Docker in the future. For now, it's just an example of something we'll use to create resources and we'll do that via the above mentioned provider in a Terraform configuration.

I'll use a POSIX-compatible shell in these examples. This should work in pretty much every environment. It should be straightforward to just use a default terminal (with a default shell or your favorite shell) anywhere other than Windows. If you're using Windows, this will not work with the `cmd` or `Powershell` shells. I generally recommend doing most of your dev work in a Linux distro in [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/) when you're using Windows. If not that, you can still use these commands on Windows using an emulation layer or something of the sort, such as Git Bash. I can write more about these topics at some point in the future.

To successfully execute everything in the example, you'll need to have Docker, Git, curl, and Terraform installed.

First, a new repository from nothing:

``` sh
mkdir sample-docker-infrastrcture
cd sample-docker-infrastrcture
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

In this new file, we'll set up a provider. We do this with the [HashiCorp configuration language](https://github.com/hashicorp/hcl)

``` terraform
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
      version = "3.0.2"
    }
  }
}

provider "docker" {
}
```

Generally, you can find good documentation for whatever provider you want to use with Terraform on the [Terraform Registry](https://registry.terraform.io/) site. In this case, we've just lifted the required configuration from the Docker provider from the information on the documentation for that provider (via the purple `Use Provider` button on that site).

Terraform requires, after setting up providers, that you initialize your workspace to work with your selected providers. This downloads the providers you have configured, creates a lock file for consistent versioning and execution, and creates a starting point for your state. Had we configured a backend for our state to store it somewhere other than locally, it would go and set up state in that location (and/or read existing state already there). Because we didn't set up a backend, it it here, instead create files on local disk.

To initialize, use:

``` sh
terraform init
```

If you look at your filesystem in the current directory, you should notice there's a new, hidden, subdirectory created called `.terraform`. You should also see a new, hidden, file called .terraform.lock.hcl. These are expected. It is inside the .terraform directory that the files implementing the provider(s) where downloaded and your state is stored. `git status` should reveal that the only untracked files showing in your repository are the new `main.tf` file you created explicitly, and the lock file created by Terraform when you ran `terraform init`.

This is a good place to go ahead and create a commit, having completed an atomic operation in our setup.

``` sh
git add main.tf.
git add .terraform.lock.hcl
git commit -m "Add and initialize Docker provider

to enable creating resources with Terraform using Docker with the version
pinned for consistent operation"
```

Note that, due to our ignore file, we've commited our Terraform HCL confdiguration and the version lock file, but not any of the provider binaries or any Terraform state. This is as it should be and the reason we set up the ignore file.

Now, in the same file (main.tf), go ahead and create a couple resources (below the provider configuration already there).

``` terraform
# Pulls the image
resource "docker_image" "nginx" {
  name = "nginx:latest"
}

# Create a container
resource "docker_container" "foo" {
  image = docker_image.nginx.image_id
  name  = "samplecontainer"
}
```

This sample has two blocks, each of them resources. This is essentially what is shown in the documentation as samples for the Docker provider. Executing on this will create two resources. Before we execute on creating these resources, it's a good idea to first use Terraform's plan verb to see what we will create.

That looks like this:

``` sh
terraform plan
```

The output of that command looks like this:

``` sh
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.sample will be created
  + resource "docker_container" "sample" {
      + attach                                      = false
      + bridge                                      = (known after apply)
      + command                                     = (known after apply)
      + container_logs                              = (known after apply)
      + container_read_refresh_timeout_milliseconds = 15000
      + entrypoint                                  = (known after apply)
      + env                                         = (known after apply)
      + exit_code                                   = (known after apply)
      + hostname                                    = (known after apply)
      + id                                          = (known after apply)
      + image                                       = (known after apply)
      + init                                        = (known after apply)
      + ipc_mode                                    = (known after apply)
      + log_driver                                  = (known after apply)
      + logs                                        = false
      + must_run                                    = true
      + name                                        = "nginxsample"
      + network_data                                = (known after apply)
      + read_only                                   = false
      + remove_volumes                              = true
      + restart                                     = "no"
      + rm                                          = false
      + runtime                                     = (known after apply)
      + security_opts                               = (known after apply)
      + shm_size                                    = (known after apply)
      + start                                       = true
      + stdin_open                                  = false
      + stop_signal                                 = (known after apply)
      + stop_timeout                                = (known after apply)
      + tty                                         = false
      + wait                                        = false
      + wait_timeout                                = 60
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id          = (known after apply)
      + image_id    = (known after apply)
      + name        = "nginx:latest"
      + repo_digest = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
```

This is what we expect. Two resources will be created. One will represent the pulling of the image and the other the creation of a container from that imaage. Deploy these resources to Docker, running on your machine, with the following command. `terraform apply` first does a plan, and then it asks for confirmatino that you really want to execute on the plan. You can either give a parameter to the command to run without the prompt, or respond `yes` to the prompt to cause Terraform to actually create (and/or destroy) actual resources. Becauase we started with empty state, indicating resources don't already exist, and there are two resources in the configuration not in that state, the plan indicates the creation of two resources and application of the plan will actually create those resources.

``` sh
terraform apply
```

The output will look like this (including having keyed `yes` in response to the prompt):

``` sh
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.foo will be created
  + resource "docker_container" "foo" {
      + attach                                      = false
      + bridge                                      = (known after apply)
      + command                                     = (known after apply)
      + container_logs                              = (known after apply)
      + container_read_refresh_timeout_milliseconds = 15000
      + entrypoint                                  = (known after apply)
      + env                                         = (known after apply)
      + exit_code                                   = (known after apply)
      + hostname                                    = (known after apply)
      + id                                          = (known after apply)
      + image                                       = (known after apply)
      + init                                        = (known after apply)
      + ipc_mode                                    = (known after apply)
      + log_driver                                  = (known after apply)
      + logs                                        = false
      + must_run                                    = true
      + name                                        = "samplecontainer"
      + network_data                                = (known after apply)
      + read_only                                   = false
      + remove_volumes                              = true
      + restart                                     = "no"
      + rm                                          = false
      + runtime                                     = (known after apply)
      + security_opts                               = (known after apply)
      + shm_size                                    = (known after apply)
      + start                                       = true
      + stdin_open                                  = false
      + stop_signal                                 = (known after apply)
      + stop_timeout                                = (known after apply)
      + tty                                         = false
      + wait                                        = false
      + wait_timeout                                = 60
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id          = (known after apply)
      + image_id    = (known after apply)
      + name        = "nginx:latest"
      + repo_digest = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

docker_image.nginx: Creating...
docker_image.nginx: Creation complete after 9s [id=sha256:2d21d843073b4df6a03022861da4cb59f7116c864fe90b3b5db3b90e1ce932d3nginx:latest]
docker_container.foo: Creating...
docker_container.foo: Creation complete after 0s [id=c5c4e317d9b1c0d34185267549db5b357662416fc0f7f764b32e7c956438539d]
```

You should now be able to confirm, using `docker image ls` and `docker container ls` that you have pulled an image and created a container.

Now, do one more thing. Let's tear down what we've created.

``` sh
terraform destroy
```

The same confrmation will be required. When you give it, the output should look like this:

``` sh
docker_image.nginx: Refreshing state... [id=sha256:2d21d843073b4df6a03022861da4cb59f7116c864fe90b3b5db3b90e1ce932d3nginx:latest]
docker_container.foo: Refreshing state... [id=c5c4e317d9b1c0d34185267549db5b357662416fc0f7f764b32e7c956438539d]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.foo will be destroyed
  - resource "docker_container" "foo" {
      - attach                                      = false -> null
      - command                                     = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - container_read_refresh_timeout_milliseconds = 15000 -> null
      - cpu_shares                                  = 0 -> null
      - dns                                         = [] -> null
      - dns_opts                                    = [] -> null
      - dns_search                                  = [] -> null
      - entrypoint                                  = [
          - "/docker-entrypoint.sh",
        ] -> null
      - env                                         = [] -> null
      - group_add                                   = [] -> null
      - hostname                                    = "c5c4e317d9b1" -> null
      - id                                          = "c5c4e317d9b1c0d34185267549db5b357662416fc0f7f764b32e7c956438539d" -> null
      - image                                       = "sha256:2d21d843073b4df6a03022861da4cb59f7116c864fe90b3b5db3b90e1ce932d3" -> null
      - init                                        = false -> null
      - ipc_mode                                    = "private" -> null
      - log_driver                                  = "json-file" -> null
      - log_opts                                    = {} -> null
      - logs                                        = false -> null
      - max_retry_count                             = 0 -> null
      - memory                                      = 0 -> null
      - memory_swap                                 = 0 -> null
      - must_run                                    = true -> null
      - name                                        = "samplecontainer" -> null
      - network_data                                = [
          - {
              - gateway                   = "172.17.0.1"
              - global_ipv6_address       = ""
              - global_ipv6_prefix_length = 0
              - ip_address                = "172.17.0.3"
              - ip_prefix_length          = 16
              - ipv6_gateway              = ""
              - mac_address               = "02:42:ac:11:00:03"
              - network_name              = "bridge"
            },
        ] -> null
      - network_mode                                = "default" -> null
      - privileged                                  = false -> null
      - publish_all_ports                           = false -> null
      - read_only                                   = false -> null
      - remove_volumes                              = true -> null
      - restart                                     = "no" -> null
      - rm                                          = false -> null
      - runtime                                     = "runc" -> null
      - security_opts                               = [] -> null
      - shm_size                                    = 64 -> null
      - start                                       = true -> null
      - stdin_open                                  = false -> null
      - stop_signal                                 = "SIGQUIT" -> null
      - stop_timeout                                = 0 -> null
      - storage_opts                                = {} -> null
      - sysctls                                     = {} -> null
      - tmpfs                                       = {} -> null
      - tty                                         = false -> null
      - wait                                        = false -> null
      - wait_timeout                                = 60 -> null
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id          = "sha256:2d21d843073b4df6a03022861da4cb59f7116c864fe90b3b5db3b90e1ce932d3nginx:latest" -> null
      - image_id    = "sha256:2d21d843073b4df6a03022861da4cb59f7116c864fe90b3b5db3b90e1ce932d3" -> null
      - name        = "nginx:latest" -> null
      - repo_digest = "nginx@sha256:593dac25b7733ffb7afe1a72649a43e574778bf025ad60514ef40f6b5d606247" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

docker_container.foo: Destroying... [id=c5c4e317d9b1c0d34185267549db5b357662416fc0f7f764b32e7c956438539d]
docker_container.foo: Destruction complete after 0s
docker_image.nginx: Destroying... [id=sha256:2d21d843073b4df6a03022861da4cb59f7116c864fe90b3b5db3b90e1ce932d3nginx:latest]
docker_image.nginx: Destruction complete after 0s

Destroy complete! Resources: 2 destroyed.
```

Again, `docker image ls` and `docker container ls` should confirm the destruction of these resources.

Don't forget to put your resources into your Git repository so you can create them again and have a record of what you've done.

``` sh
git add main.tf
git commit -m "Add Docker resources

to pull an images and create a container as a sample of what we
can do with a Terraform provider" 
```

This should get you a rudimentary start on seeing what Terraform can do. I have only scratched the surface here, but it's a start. I'll write more about going further and how you might start to create some real and useful resources, especially using providers for the public clouds and such. There is much more to know.
