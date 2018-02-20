---
title: "Git Workflow"
date: 2018-02-16T21:25:42-05:00
draft: false
---

## You Got Your New Task, Now What?

Before code is even written, an issue has to be created in the project’s code repository. The title of the issue should be rather short, but descriptive. Important details should be added to the description. Make sure to assign yourself to the issue if you are the one doing the work. If the project you are working on uses milestones and labels, be sure to apply those as well. 

Committing directly to the master branch is a very bad idea. Even if there is only one person on the project, it is still a good idea to create a branch for each feature, bug, etc that is being worked on. The master branch should always be stable. By committing random small commits to master, it could no longer be stable. Branch names are based on the issue number that was created to track progress of the work and two or three letters from the project’s name. For example, if the project was named My Details and the issue number was 5, a good branch name would be MD-5. MD referrers to the project name and 5 is the issue number. This way if there are multiple people working on the same project, it is possible to quickly look at all branches and see what issues are being worked on.

Permissioning is one way to stop people from committing directly to a branch. The master branch can be protected so that only masters or owners of the project can directly commit to it. When working with many student developers it is a good idea to lock the master branch down. Student developers will not be able to push any commits without creating a merge request for review and testing. They also will not be able to merge any merge requests that are open.

Now that our branch is created, commits can now be made. The commits must follow a few standards. If the commit message is very long, it has to be broken apart. There should be a subject line (no more than 72 characters), a new line, then the body of the message. The subject line should be very short and say what the commit actually does. At the end of the subject line it is good practice to put a #issueNumber. This makes it so the commit is brought up in the issue, so you can see all commits for an issue. The commit body must describe why the change is being made and any side effects it might have.

Now that the feature, bug, etc has been fixed, it is time to submit a merge request. When submitting a merge request make sure to assign it to someone else to take a look at. Having more eyes on an issue can help detect problems before they go into production. This also means that the branch should not be merged unless it works as intended. The master branch should always be stable that can be pushed to production at any point. In the description of the merge request put in ‘Closes #issueNumber’. Once the merge request has been merged, the issue that it references will automatically be closed. Be sure to check the box for remove branch when the merge request is accepted. Once a branch has been merged, it should never be worked on again. This keeps things more clean by removing stale branches that are no longer needed and to not confuse people who may say that why is this branch being worked on again, it was already merged.

If the branch requires more changes before it is merged, it can be edited to stop anyone from merging it. When editing the merge request, prepend to the title `WIP:`. WIP stands for Work In Progress. If a merge request has at the start of the title `WIP:` it cannot be merged. Once it is ready to be merged, the `WIP:` can be taken out and the branch then can be merged. This feature can be used if the work has been completed, but it is not going into production yet. Remember that the master branch should always be stable and can be deployed to production.

Once something has been pushed to production or is about to be pushed into production, it is a good idea to tag the project. Tagging means to give a human readable name to a commit hash. If the project is now at version 1.2.5 when it gets pushed to production, it would be a good idea to tag the commit hash of the master branch as v1.2.5. This way if there is an issue with version 1.2.6 and a few hundred commits have been added between 1.25 and 1.2.6, version v1.2.5 can be checked out and pushed to production. The tag v1.2.5 is based on [semantic versioning]( https://semver.org/). 


## Pitfalls To Avoid

**DO NOT STORE PASSWORDS IN GIT.** When commiting changes to a repository, do not commit passwords. Once a commit has been pushed to a remote repository, it is accessible to anyone that has access. Passwords should be stored in configuration files that live outside of the code base for this reason. To ensure that passwords are not committed accidentally it is good practice to add these configuration files to the project gitignore file. In which, the file will be ignored for all purposes related to using Git.

**DO NOT USE A GUI TO USE GIT.** Using any of the git GUI tools can cause many issues. Some of which include; accidentally committing files that should not be committed, committing code to the wrong branch, introducing special characters at the end of all lines. It is much easier to fix any issue on the command line. Git is a very powerful tool, it is worthwhile to learn how to use it properly.

**DO NOT USE GIT PUSH OR GIT PULL WITHOUT SPECIFYING A BRANCH NAME.** When using the git pull or git push commands, always specify the branch that is being targeted. On older versions of git, if no branch is specified, it defaults to the master branch, even if that is not the current branch. 


## Git Ignore Files

Git has the ability to ignore files that should not be committed. When using a tool like gradle or maven, there is a directory created that has the compiled artifact in it. Some development environments, like IntelliJ or Eclipse, create hidden folders containing their configurations.  All of these files should not be added to the repository. A .gitignore file can be created to make git ignore all of these files. A good resource for finding what files to ignore can be found here: https://github.com/github/gitignore


## Advanced Git Topics

Sometimes the code that is being written is an excellent candidate to be contributed back to the main open source project. It doesn’t make sense to make a commit on the internal project’s repository then make another commit on the open source version. The open source project may also have new additions that others have contributed, like security updates, that must be applied locally. This is where git remote repositories comes into play. The open source repository can be added as an upstream remote repository. The name upstream should always be used to define the open source project.

Submodules are a way to include other projects as a code dependency. They are essentially a git repository inside another git repository. Making changes inside of the submodule does not mean that the outer project will get those changes. Changes have to be committed inside the submodule and then the outer project needs to be updated to reference the latest commit hash of the submodule. Submodules do not work very well with older versions of git. It is highly recommended to update to a later version of git if working with submodules. 


## Resources

* Learn git online: https://try.github.io/levels/1/challenges/1
* Getting Started With Git: https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control
* Fixing Mistakes In Git: https://www.linux.com/learn/fixing-mistakes-git
* Atlassian Git Tutorials: https://www.atlassian.com/git/tutorials
