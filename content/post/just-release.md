---
title: "Just Release"
date: 2019-02-07T13:12:05-04:00
draft: false
---

## Release Early And Release Often
This is an extension to my previous post about [Automate All the Things]({{< ref "automate-all-the-things.md" >}}). You should really read that before you continue. In the last section of that post, I mentioned about automating pushing code to production. Well I have left that job and was never able to fully automate all the things. The place that I'm at now has really taught me the importance of the phrase "release early and release often".

## Just Release!
Before leaving my last job, we were using uPortal version 5.0 in production. Version 5.0 came out October 24, 2017. I left that job the first day of August 2018. Software no longer sits still, the current version of uPortal is 5.4. Releasing software is a hard task when you have to do it manually.

You might already see a problem. The longer release cycles are, the longer it takes to upgrade. At a certain point you either stop updating or spend all your time updating. As everyone knows, not upgrading is a giant security risk. If you really think about it, you are left with only one choice: spend all your time updating. In this state you can't easily push out new features when your developers are spending their time making sure the upgrade works. At that point management gets upset that nothing is really being done. It's never a good state to be in.

Now if you are lucky enough to have a decent sized team and can dedicate a small amount engineers for upgrade work, you can still push out new features. Wait... no we can't push out new features! I mean you could, but then you need to make sure they work with the old code base and the new one! That's time that usually doesn't exist for any project. If your team is large enough, you can probably manage, but that means that if something goes wrong, there is a lot more things to debug.

## New Job And Awesome Engineers!
I said above that my new job has taught me the importance of the phrase "release early and release often". When I first started we were building a new tool to help people install Spinnaker. I started work right away to help make this an awesome tool, BUT I wasn't thinking about releasing this new tool just yet. It wasn't finished. My background in higher education was always about release when it is "perfect". Yes, I know, software can never be perfect, but you don't release alpha software. Start ups are way different. You have to release as soon as possible and get feedback. You can make a really cool application, but if it doesn't solve the need of the people using it, then what good is it? This was an area that I never experienced before. Luckily the people I work with taught me this important lesson. Now any new project I work on, I start off with a way to get the product out there in front of users.

The TLDR of this post is just release your software! Get it in front of the people who want to use it. Get feedback, make it better!
