---
title: "Upgrading To A New CI/CD Platform"
date: 2021-04-02T20:12:05-04:00
draft: true
---

## Background
I started a new job in 2020 as a DevOps Engineer. I finally got to live my dream of working in the DevOps space! Now I was the one slinging infrastructure as code, continuous integration pipelines, continuous deployment pipelines, and tooling to make product engineering run more smoothly! Before all of this, I wouldn't say I was a DevOps Engineer at all. Did I do things related or work on products for the space? You bet! I was never in the trenches as a user. Being in the trenches using the tools on a daily basis, you learn a lot. The two topics that I think I learned the most about are actually not technical. They are

1. DevOps tooling and infrastructure is more complicated than we give it credit
2. Changing the world in a day while totally doable technically, it will never work

Hopefully by the time you are done reading this story, you will come to similar conclusions that are listed above. Our journey begins within the first few weeks of starting a new job as a DevOps Engineer at a Series A health care startup.

## Day 0 Infrastructure
As it goes when starting any new job, you have to understand what is currently there, no matter if its for product engineering or devops engineering. During my first few weeks, I started looking at what we had setup from a devops standpoint. GitHub was used as our git hosting service. For the CI side of things, Jenkins was being used for that. Jenkins was also used from a deployment standpoint. Terraform was used to create infrastructure in the cloud. Nothing seems out of the ordinary right? Pretty basic setup that I'm sure many many people use. There are always problems with any solution, so I didn't think too much of it. I had other things to do during my ramp up. Until we hit a production issue.

The production issue was reported by product engineering and the issue was Jenkins was running out of space to store build data and any other metadata that it needs. Jenkins was running in a Kubernetes cluster and using a persistence volume to store information. Should be a simple fix right, increase the storage on the backing volume. Doesn't it always take a production issue to look deeper at the setup? That is what I did.

Before we dive into what issues were found during the production investigate, let's talk about what was learned after talking to product engineering.

### Running Tests And Building Containers
When someone creates a pull request in GitHub it will kick off a Jenkins pipeline. The pipeline runs some unit tests and builds a Docker image. The problem is Jenkins never reports back to GitHub what the status of these jobs are. In order to merge a pull request, one has to have the GitHub page open **AND** be looking in Jenkins to see if the jobs were successful. While this works, it's a bit of a headache to have to jump back and forth. A big downside to this approach is that pull requests could **NOT** be gated on tests passing. If someone wanted to deploy the resulting built image, they would have to hunt down the image tag in the Jenkins logs as well. This part was not as annoying because only a few people did deployments.

### Deploying Code
As mentioned above, Jenkins was also used to deploy code to all of the environments. For deploys to the dev Kubernetes cluster, it happened when code was merged into the master branch. Once the merge button was hit, Jenkins would talk to Hashicorp Vault to get secrets. These secrets would then be added as parameters to `helm install

## Iteration # 1

## Iteration # 2

## End Result

## How To Build Confidence And Buy In
