---
title: "Projects"
date: 2018-02-17T23:06:25-05:00
menu: "navbar"
draft: false
---

## goedit
While I am a huge fan of vim, I saw a really cool [tutorial](http://viewsourcecode.org/snaptoken/kilo/) on how to make a text editor in C. I thought it would make for a great project to learn more Go. goedit uses an array structure for storing and modifying contents of a file. Currently it only supports editing one file at a time. It supports a few vim commands for the users coming from vim.

https://github.com/bpowell/goedit

## brocker
brocker is my attempt to understand how Docker works. brocker uses Linux namespaces to separate the host system from the container that is running. It makes use of the pid, net, and UTS namespaces.

In order to run a container with brocker, you first need to create a service. This is similar to using something like Docker Swarm or Kubernetes. In brockers case, a service is just an nginx server acting as a reverse proxy for a number of containers of the same type. brocker was designed to be used to run web applications. Due to the fact that you cannot run a container without a service, this is not the best idea for running something like PostgreSQL or Redis. If you wanted to run multiple of the same container for load balancing purposes, you would add each container to the same service.

https://github.com/bpowell/brocker

## dozerlOS
Operating systems have always interested me. The best way to learn about how they work is to build one! I actually started this project way back in 2004. The version of the code in the linked repo is not the original version. When I started out, git wasn't invented yet, and I was still new to programming. Being new means that I didn't know about version control yet. It wasn't until 2009 that I put the code for this project in git. I named the project after my dog Dozer.

dozerlOS sets up the interrupt descriptor table, interrupt service routines, interrupt requests, timer, and use of the keyboard. It requires GRUB to boot. On boot it will display the amount of RAM the system has and then it will allow the user to type things. There is no shell, so anything that is typed with just be displayed on the screen.

One of the hardest parts about developing an operating system is debugging. Yes there are tools like QEMU and Bochs, but it is always more fun to play with real hardware! I started to work on a way to unwind the stack to determine what called what to help debug issues.

https://github.com/bpowell/dozerlos

## vim-android
Vim plugin to help with Android development. This was created back when there was no Android Studio, all you had was Eclipse with ADT plugin. I have been using vim for many years and am not a fan of IDEs. While developing the Android mobile app for Oakland University, a coworker and I built this vim plugin to make Android development in vim possible.

https://github.com/bpowell/vim-android

## Gooogle Music Player
Google Play Music is really awesome, but there is no desktop client for it. No desktop client means that keyboard keys like play/pause/next/previous don't work. If you are playing music and want to change the song, you have to actively hunt down the tab and then click on what ever action you wanted. To me that was a pain in the butt. This project gave me a chance to learn QtWebKit and liblastfm for scrobbling.

https://github.com/bpowell/gm-player
