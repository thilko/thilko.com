---
layout: post
title: "Global day of code retreat 2013"
date: 2013-12-15 14:02
comments: true
categories: coderetreat agile coding testing
---
On 14th of december we had a great [code retreat][1] in Luebeck! Luebeck was one of more
than [165 cities][2] that were celebrating the [global day of code retreat][7]. Held at
[cloudsters][3] and supported by [draeger][4], we had several interesting sessions and various video conferences
with other locations.

{% img /images/gdcr13.jpg 800 600 %}

## Session 1: Plain old TDD
After a breakfast we started the first session with plain old TDD. Focus was on getting in touch with
the problem ([game of life][10]). Since all participants already had experiences with TDD, this session ran
smoothly.

## Session 2: Pairing games
In the second session we applied some pairing techniques: i.e. evil pair, ping ping and timeboxed.
Evil pair was one of the favorite games, I assume it is related to the fun factor.

## Session 3: TDD as if you meant it
After two a bit more warming up sessions, we got our hands dirty with [TDD as if you meant it][11] now. 
It worked surpisingly well, although this technique is not the easiest one.

It was impressive to see what happens if you focus on really small steps and skip any upfront design 
activities.

{% img /images/gdcr13_session.jpg 800 600 %}

Our lunch was delivered by [Fresh and Fair][5], quite tasty!

## Session 4: Switching pairs
A bit tired we moved to our next session. To warm up again the pairs got the exercise to switch
the laptop every ten minutes and takeover the code from another pair. Lessons learned: it does not
take months to create legacy code!

## Session 5: Object calisthenics
Now we streched us a bit with object calisthenics. Key idea is to apply several
restrictions in order to enforce better or even other programming styles.

Since I saw quite simliar solutions the sessions before we applied the following constraints:

* no conditionals(if/else, switch/case, ternary operator)
* no loops
* no primitive data types(including arrays)

## Session 6: Baby steps
In our last session we took some [baby steps][12]. The time limit was 2 minutes in which a red test 
must be written and fixed. Otherwise all changes had to be reverted. Refactoring is done in a new 2 minutes
cycle. Quite challenging too, and we quickly realized that we often make to many steps at once.

Exhausted but satisfied with a day full of cells we moved to the [christmas market][6] around the corner 
and relaxed with some good wine and new ideas.

For me it was the first time as a [facilitator][8] and I have to admit that the job is challenging as well.

Thx all origanizers and participants for the great day! Thx [Sina][9] for hosting!

[1]: http://coderetreat.org/
[2]: http://globalday.coderetreat.org/gdcr_2013_timeline.pdf
[3]: http://luebeck.cloudsters.net/
[4]: http://www.draeger.com/sites/de_de/Pages/Krankenhaus/Welcome.aspx
[5]: http://www.fresh-und-fair.de/
[6]: http://www.luebecker-weihnachtsmarkt.de/
[7]: http://globalday.coderetreat.org/
[8]: http://coderetreat.org/facilitating
[9]: https://twitter.com/_theSinster
[10]: http://en.wikipedia.org/wiki/Conway's_Game_of_Life
[11]: http://coderetreat.org/facilitating/activities/tdd-as-if-you-meant-it
[12]: http://blog.adrianbolboaca.ro/2013/01/the-history-of-taking-baby-steps/
