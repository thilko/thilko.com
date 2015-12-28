---
layout: post
title: "Enforce single responsibility for your commits"
date: 2013-06-28 17:39
comments: true
categories: coding
---
TLDR;
Split your commits into sensible units of work and enforce single responsibility on them.

---

Have you ever thought about your commit messages during your everyday development? No? Have you ever
read through the commit history of your current project and asking yourself what could be in commits
called:

*"fixed strange bug"*, *"merge"* or *"did a lot of stuff and its impossible to describe it"*

Maybe then this article could help you to gain control about commit history and to create a story where
everyone can see how you crafted the piece of code.

## Enforce single responsibility for your messages

A commit history tells you a story about how the system has evolved. This story helps you
to find interesting parts of your application, to hunt down bugs or just to get a feeling how your
collegues are working.

Let´s see an example of a typical commit history:

|**committer:**|**:message**|
|||
|john        |Initial commit.
|john        |Setup of persistence framework
|mary        |Some work on service side
|scott       |Merge
|mary        |Tests
|john        |Fixed typos and introduce checkstyle
|mary        |update person logic for new requirements
|scott       |hunt down some bugs
|john        |refactored crappy methods, it´s all bullshit
...


Now it´s your turn. You as a developer are on bug hunt and looking for the piece of code to blame. Without proper tools
like [git bisect][1] you are lost.

But what about this:

|**committer:**|**:message**|
|||
|john|First working prototype with logon screen
|john|Save the login attempts in db. Hibernate is ready to use.
|mary|You can now change your password
|scott|FEAT-3 New feature requirement implemented, users need to enter a secure pswd
|mary|Forgotten to write tests for last feature, next time I will write them first
|john|Fixed typos and introduce checkstyle
|mary|FEAT-4 Users can login just once a day
|scott|BUG-0815 Usernames with special characters produced errors, they are forbidden now
|john|extracted hasSpecialCharaters() and isAlreadyTaken() in PasswordManager
...

With these messages I´ve got a feeling how my colleagues are working and what features they have implemented. I
even have a reference for feature requests and bugs!

**Try hard in cutting your commits in sensible units of work.** Everytime you wrote a message like

> Eliminated waste AND implemented feature x AND did some configuration stuff

a warning bell should ring!

![climbing](/images/commit_msgs_climbing.jpg)

It´s not just for fun and beauty. **Think about your vcs like a safety rope while you are climbing a
hill.** Everytime you commit you make a safe check point. The higher you climb without making a new one, the deeper
you fall if you had to revert because you got lost in the jungle of a complicated refactoring.

It´s also about **staying focused** on the current task you are working on. How often you find refactoring tasks during
stepping through code while implementing a feature? Write them on a sticky note or add an TODO. Fix your current
task, give your commit an unique name **and do the refactorings right away!**

**Commit messages with a sensible name produces a context** and makes it easier for your reviewer
to understand your changes.

Produce small, sensible commits is like [taking baby steps][2]. **It helps to work with legacy systems** and if you are asked what
have you done you can say: "Just look at my commit history".

**Try hard to let your commits follow the single responsibility principle**

## The hard world of dvcs
We are living in the world of git, mercurial which gives you a complete repository on your
machine. Great because you can make even smaller commit slices and push all commits as bigger unit of work afterwards.
Bad because its leading to a later integration.

**You are not done until your code is pushed AND build by the CI server.** Look for sensible points where you can push your
work in order to **integrate your code as soon as possible.**

[1]: http://git-scm.com/book/en/Git-Tools-Debugging-with-Git
[2]: http://blog.adrianbolboaca.ro/2013/01/the-history-of-taking-baby-steps/
