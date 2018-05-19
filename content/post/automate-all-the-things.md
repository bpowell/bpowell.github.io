---
title: "Automate All the Things"
date: 2018-05-19T13:12:05-04:00
draft: false
---

## Forward
This year I was supposed to go to Open Apereo 2018 and do a talk on devops and uPortal. Unfortunately I am not able to go, but I had some people contact me about writing something up about the talk. This post will focus more on the automation section of what I was going to present about.

## Introduction
One of the things that devops helps with is automating parts of the development cycle. Before I get into the things that I have helped automate, let's go through how everything was done before automation.

The project that I work on is an open source Java web portal called uPortal. The main web application, uPortal, talks to things called portlets that follow the portlet specification. These portlets can be very simple to extremely complex web applications. Most of our portlets focus on academic information for students.

I lead a team of student developers to work on this large system. Students don't have the same access that full time employees do, so what they can do can be a bit limiting sometimes. When students complete work and want to see if the changes they made work in our server environment, they would have to wait for me to manually go to the server, pull their code down, compile and deploy it for them. While this seems like an easy task (it is), it's very time consuming when I have other projects to do.

From a server perspective, we have a development environment consisting of one web application server and one database. We then have a test environment that has two load balanced web application servers and one database. Production is a bigger system setup similar to our test environment. When code changes come in, they first get promoted to development, then if that passes, we pass it up to the test environment. If it passes test, it will make it to production when we do a production upgrade. All of these steps to deploy code to each system is a manual process. See a problem yet?

For code to make it into production we have to cut releases of each project. We have many portlets and other Java web applications that need to be managed. The first step for cutting a release is to make sure the git repository is clean. We don't need any testing changes to make it to production - I have messed this up before, pointed to the development URL for a service and that change made it to production. Next we have to compile the code. After that, we can upload the resulting war file to our local maven server. Since switching to gradle, uploading war file to our maven server was more painful. With maven based builds, we could upload the `pom.xml` file and the server would determine `groupId` and other artifact information. In the gradle based projects, we have to type that in by hand - another area for a mistake to pop up. The newer versions of the maven server software we use removes the ability to manually upload artifacts. Next we create a tag of the version in git, this way know what version of the code has been in production. The last step is to update our main project, uPortal, with the new version of the web application that was changed.

That's a very long winded introduction, but necessary to show how things used to be done. When you have multiple projects and multiple people doing lots of work, it gets to be a lot to manage.

## First Steps: GitLab Is Awesome
GitLab is an awesome service. Without GitLab, we would not be able to easily automate all the things we have over the past two years. For those who don't know, GitLab is a service that can host git repositories. GitLab can either be used on-site or in the cloud. It is very similar to GitHub, but I personally think it is better. The main reason for that is their CI and CD offerings. CI stands for continuous integration. CD is continuous deployment. These two offerings are amazing and are the basis for automating everything that we have so far. 

GitLab has the concept of pipelines. Pipelines is the collection of steps that you want to happen when a certain action takes place. Our normal pipeline process has two steps. The first step in our pipeline is to build the web application. The next step is to deploy the built application. There is also parts of the pipeline that can activated in very specific circumstances, but we will cover that later. The pipeline concept is very powerful.

The first thing that I automated away was deploying of code to our development environment. Now when anyone on my team makes a change to the code base and pushes up their code to GitLab, GitLab CI takes over and compiles and deploys the code to our development environment. No longer do any of the students that work with me need to wait for me to deploy their code. They can do it themselves!

Doing this has caught many bugs much faster than we could have ever expected. It allows me to test changes without having to mess up my local environment with what ever changes that I am currently working. I can just log into our development server and try the changes right there. If I don't notice anything wrong, I can just go straight to the merge request and start reviewing the code. There has been times where a random `;` was forgotten in the code and it was committed. Well when it went to the CI step, the build failed! We could then look at the output and see that some code was messed up. People make mistakes all the time, CI can help find those before it gets bad.

A great example of this working in action is when we updated to PostgreSQL 10. When we did the upgrade we changed how hot replication works. We were doing essentially a `pg_dump` as a way of replication. Now that we were updating the database, we wanted to use hot replication. Hot replication requires the database to be configured in a certain way. After the upgrade, we kicked off our pipeline to compile, database init, and deploy our code. The build failed! We looked through the build output and found a bug in our database code. We were missing a primary key in one of our tables. When a build fails in GitLab, it will give you an option to make an issue and it will reference the failed build log. It was a one stop shop to create an issue, reference why it failed, which allowed us to just start working on a fix right away.

## Tag And Cutting Releases
Another step that takes a while and has the possibility of more errors being introduced is cutting releases. The logically way to cut down on random human errors is to take the human out of the equation. GitLab CI to the rescue! Both gradle and maven offer a way to automate the uploading of artifacts to a maven server. We modified our `build.gradle` files for all of our portlets to run a task that can be used to automatically upload the artifact to our maven server. The next step was to add that to our pipeline process.

Now we don't want all changes done to be uploaded to our maven server, that just wouldn't make sense. Again, GitLab has ways to control when parts of a pipeline can be ran. This ties into tagging in git. We modified our CI configuration file to have a section of when a tag is created, it will automatically build the artifact and run the gradle task to upload it for us. To make it so a rogue employee can't just start uploading artifacts, we have our maven server control access and in the GitLab repository settings, only masters of the project can create tags.

What used to take about 10 minutes if everything went correctly, now takes seconds. Just need to go and create a tag and the process kicks off! The best part is the human component is now gone.

## Automating Security Reports
Something we just recently done is the automating of generating and emailing security reports. There is a gradle plugin by OWASP that will check your dependencies for security vulnerabilities. Just adding it to your project doesn't do much. You still have to manually run that task and look at the output. Why not automate generating the reports and then have it sent directly to your email.

GitLab has the ability to run scheduled pipelines. Anytime code gets pushed up and having the security reports emailed to you would be insane. Your inbox would be full very quickly. But having to manually run it would make it so that it never gets ran. GitLab scheduled pipelines to the rescue. Every Sunday at 4am, all of the projects will run the pipeline to generate and email a security report to a few of the developers.

## Test And Development Environments
As I have already mentioned, code can already be automatically deployed to our development environment. Why not the test environment?

We read more up on how GitLab works with the CI configuration and found a way to do this. We already have it so the `master` branch is protected so only masters of the project can do anything to it. And the `master` branch should always be stable and be ready to be deployed to production. All other branches will be deployed to our development environment. Now when things are pushed into the master branch, they will be auto deployed on test.

## Automate All The Things
We have automated many parts of what we do. Moving code changes to the development and test environments have all been automated. Cutting releases of projects by hand is now a thing of the past. We even added something new to the process, automated security reports. All of these changes have made things so much easier for us as we have grown over the past two years. 

If you haven't noticed yet, I have been talking about devops for quite a bit and haven't mentioned things like Docker, Kubernetes, Open Shift and a handful of other awesome utilities to make devops easier. These are really great tools, but not every place has the ability to move quickly to make use of these tools. If we had access to these tools we could do a lot more.

## The Future
We still have a manual process for deploying to production. Using GitLab would could eventually set it up so even production is deployed to automatically. We don't need tools like Docker or Kubernetes to do this.

BUT if we had access to something like Kubernetes, we could do some really cool things. We don't always need to have as many nodes as we do in production during certain times of the year. During the summer when we have not many students using our service we can get away with less nodes. But during the start of a semester or when grades are released, we see huge spikes in traffic. Kubernetes allows us to auto scale. We can make rules so that when we notice a large spike of traffic, it will auto provision new nodes. 
