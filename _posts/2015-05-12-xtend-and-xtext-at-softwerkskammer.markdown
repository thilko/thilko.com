---
layout: post
title: "XTend and XText at Softwerkskammer"
date: 2015-05-12 20:52:47 +0200
comments: true
categories: softwerkskammer
---
On our [6th meetup][7] of the [softwerkskammer Lübeck][8] we were lucky to have [Sven Efftinge][1] as our guest. He is the project lead for [XText][2], organisator of the [Nordic Coding Event][3] and works at [itemis][4].

![Softwerkskammer](/images/swk_xtend_xtext.jpg)

He presented [XTend][5], a language on the JVM that is build with XText and which compiles to java source code. After a walkthrough he shows with an example how easy it is to build a DSL with XText. The whole presentation was done by showing code! In my opinion the best way and he did a great job in answering questions.

What differenciates XTend from the varios JVM languages like groovy, scala, kotlin etc.?
It does not need it´s own bytecode compile since XTend it translated at compile time to java source code. In that way XTend can be seen as "syntactic sugar" for java - or a domain specific language. (writing java code without the boilerplate)

I think the advantage of XTend lies in it´s lightweight character: you are still writing java, but just in a better way. It´s like [CoffeeScript for JavaScript][6].

![Softwerkskammer](/images/swk_xtend_xtext_discussion.jpg)

It was a great meetup with some really good discussions in the end! Looking forward for the upcoming meeting in June!


[1]: http://blog.efftinge.de/
[2]: http://www.eclipse.org/Xtext/
[3]: http://www.meetup.com/Nordic-Coding/
[4]: http://www.itemis.de/
[5]: http://www.eclipse.org/xtend/
[6]: http://www.infoq.com/news/2012/06/xtend-release-10
[7]: https://www.softwerkskammer.org/activities/6_sw_luebeck
[8]: https://www.softwerkskammer.org/groups/luebeck
