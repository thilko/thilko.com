---
layout: post
title: "Generate your api doc with gradle"
date: 2014-01-12 21:12
comments: true
categories: documentation gradle spring rest
---
A [good documentation][15] is essential for your (public) api and makes it pleasant to use. But maintaining api documentation
manually is hard, tedious and error prone. Documentation is less likely to be updated if it´s far away from code. So why not
generating the documentation if the relevant information is already at your fingertips: in the code?

It´s nothing new and there are a couple of projects out there. Focussing on the java world you will find
[swagger][1], [springdoclet][2], [restdoclet][3], [jsondoc][4] and many more.

Evaluating some of the tools for a project I am working on I saw some drawbacks of the existing tools. Let´s
have a look.

Swagger
=========
Supports many different languages and I like the [swagger UI][14]. Personally I think that swagger is a bit to complex. Suppose you would
like to generate your rest api documentation for spring at build time with gradle. AFAIK you will need
[swagger-springmvc][6] and [swagger-gradle-plugin][7]. Maybe. To be honest, I think this usecase
isn´t supported yet. But if you want to generate your documentation for a non spring project at runtime,
swagger may be appropriate for you.

springdoclet and restdoclet
======
Generates the doc at compile time which would be a better solution for me, but unfortunately it was a bit
quite in those projects the last time. And finally restdoclet has [no gradle integration][5], which is a key
feature for me.

Both projects using [java doclets][8] to gather compile time informations about annotations. Personally I
think that an [annotation processor][9] is a better alternative.

jsondoc
=======
[Nice layout][16], even example requests are supported. But unfortunately you have to use jsondoc specific
annotations on top of your spring annotations.

To sum up: I just would like to have a simple plugin **for gradle** which produces a **useable html
documentation** for my **springmvc app** at **build time**. No intermediate format, no runtime dependencies. Thats it.

So I wrote [gradle-springdoc-plugin][10], maybe just another documentation generator. The project is quite young,
so don´t expect too much yet.

Used technologies
=======
* Completly written in groovy
* Produces html documentation with twitter bootstrap
* Utilize an anotation processor to get information about spring mvc annotations
* To be used as gradle plugin
* CI with [TravisCI][11]


Things I have learnt
=======
* It takes fun!
* You can [invoke the java compiler at runtime][12] - I tested the processor though
* Proper groovy (not just java with a .groovy suffix) takes fun and feels a bit like ruby
* An annotation processor is good to get compile time information about annotations
* Awkward old java api´s could be a bit more pleasant with groovy
* It´s simple to publish a project to maven central (NOT!) - but that´s another blog post

Here is an [example page][13].

[1]: http://developers.helloreverb.com/swagger/
[2]: http://scottfrederick.github.io/springdoclet/
[3]: http://ig-group.github.io/RESTdoclet/
[4]: http://jsondoc.org/
[5]: https://github.com/IG-Group/RESTdoclet/issues/10
[6]: https://github.com/martypitt/swagger-springmvc
[7]: https://github.com/victorgit/swagger-gradle-plugin
[8]: http://docs.oracle.com/javase/7/docs/technotes/guides/javadoc/doclet/overview.html
[9]: http://docs.oracle.com/javase/7/docs/technotes/guides/apt/
[10]: https://github.com/thilko/gradle-springdoc-plugin
[11]: https://travis-ci.org/thilko/gradle-springdoc-plugin
[12]: http://docs.oracle.com/javase/7/docs/api/javax/tools/JavaCompiler.html
[13]: http://www.thilko.com/springdoc/index.html
[14]: http://swagger.wordnik.com/
[15]: http://developer.github.com/v3/
[16]: http://jsondoc.eu01.aws.af.cm/jsondoc.jsp
