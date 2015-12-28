---
layout: post
title: "Why deleting code is important"
date: 2013-02-19 21:30
comments: true
categories: 
---
On my last day at [blau][1] I got a nice little tag because I was member of team "delme". Originally "delme" was a prefix for database columns that will be deleted in the next iteration. It evolved to our team name since we deleted a lot of code from old systems.


![team delme tag](/images/team_delme.png)


We had a great time, migrating legacy systems and producing cool features in an agile environment. Seeing features going live was really a joy, but to delete thousands lines of (crappy or unused) code was a pleasure as well.

We constantly seeking for code to cleanup, grooming through the legacy and our own codebase. Yes, we made no excuse for our own code. I think it is nearly impossible to create perfect code at the first shot. It is more important to work constantly on your codebase.

###Why is deleting code so important?###

* it reduces complexity
* you remove unused features and clutter
* you follow the [boy scout role][3]
* it prevents to get more mess in the system ([legend of the broken window][4])
* it makes used features more clear
* it reduces total cost of ownership

I think deleting code is more than a good habit. It is part of all the refactoring techniques that a developer should do every day. It means that you TAKING CARE OF YOUR CODE.

Normally it isn´t so easy peel out features from existing systems since they are so woven into it. But its definitely worth.

In the end deleting (unnecessary, unsued and crappy) code is as much important as writing clean and concise one. Don´t be too polite with your own code, but remeber the goodness of tests that provides a safety net for all refactorings.

> Every deleted line of code is good code


[1]: http://www.blau.de
[2]: https://twitter.com/phoet
[3]: http://www.informit.com/articles/article.aspx?p=1235624&seqNum=6
[4]: http://en.wikipedia.org/wiki/Software_entropy

