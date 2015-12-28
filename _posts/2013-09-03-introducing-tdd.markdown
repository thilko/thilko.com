---
layout: post
title: "Introducing TDD"
date: 2013-09-03 20:27
comments: true
categories: testing
---
TDD is nothing new. Kent Beck presented his [book][3] 2002, long before scrum and agile become mainstream. Although nothing 
really new and proved as increasing software quality I found TDD as one of the most controversly discussed
topics in work as software developer. Why?

Developed with and without TDD I could now say that TDD is my preferred working mode. I am deeply conviced that TDD
is the best mode to write software that should be maintainable.

But how to convice developer about TDD? Some thoughts:

##Start small##
I like the ideas of [Growing object oriented systems guided by test][1]. But first steps with TDD could be
daunting if you start TDD with automated acceptance tests that require a more sophisticated infrastructure
than simple jenkins instance. You don´t need that much technical infrastructure, just start with rspec,
JUnit right away.

How simpler you start the greater the change that more developers get a glimpse of the benefits of TDD and you
can continue to improve your testing with integration and acceptance tests.

##Deal with resistance##
Nothing comes for free. Maybe you have the luck and all developers in your team get adopted in a second. Normally
thats not the case. Cultural habits, fear of change or simply missing knowledge makes the job of distributing
TDD even harder.

Try to convice by evidence. Do not dictate TDD if the people don´t like it. Try to get the developer into a situation
where they realize the benifits by themselves.

##Facilitate dojos##
Dojos are a great way to experiment with TDD in a sandbox. You don´t have to cope with typical system problems
which could be used as an excuse not to do TDD. Start with small katas in order to concentrate on the TDD workflow.

##Put the fun into programming##
Lonesome programming in a cubicle is the opposite of fun. Start with pair programming and use [pairing games][2] to get a
new sense of programming. It also helps a lot to get a common sense for your coding style.


[1]: http://www.growing-object-oriented-software.com/
[2]: http://www.blaulabs.de/2011/11/02/play-with-your-pair/
[3]: http://www.amazon.de/Driven-Development-Example-Addison-Wesley-Signature/dp/0321146530
