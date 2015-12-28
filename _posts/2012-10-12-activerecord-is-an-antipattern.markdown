---
layout: post
title: "ActiveRecord is an antipattern"
date: 2012-12-10 21:26
comments: true
categories: 
---
Recently I read a very good book about [SQL Antipatterns][1]. What I did expected was a whole bunch of good advices about how to write bad sql and how to avoid it. I was surprised to find the ActiveRecord in the application specific antipatterns section and it inspired me to some thoughts that I would like to dump here.

But first of all, a small review of the book: It is divided in 4 chapters:

### Logical Database Design Patterns
This chapter contains guidelines how to solve logical design problems in your DB. The most inspiring one for me was how to save a tree in a relational DB. I always created parent-child associations but now I know that there are a lot of (better) [choices][9].

### Physical Database Design Pattern
Typical problems with rounding, wrong indexes and cases where you reference a file by name in the database and store it elsewhere(its called phantom files). In the latter case you should just store the file as BLOB.

### Query antipatterns
Do not use queries with wildcards in it, since you they aren´t refactoring save. Do not use like expression for complex search queries and avoid complex queries which try to solve evereything. Use multiple ones instead. Interesting stuff if you have to write your SQL for yourself. Mentioned [lucene][2]/[solr][3] as search engine alternatives.

### Application development antipatterns
It´s about readable passwords in the db(does anyone really do this?), SQL Injection and other stuff that was not really interesting except the last part about the ActiveRecord antipattern.

### Why?
Maybe nothing new for some out there, but for me it was great to recap the reasons why ActiveRecord is an antipattern:

* it leads to an [anemonic design][4] because you get CRUD methods on the public interface
* objects are difficult to test, you need the complete framework or mock a shared context
* it violates the single responsibilty principle

Since ActiveRecord is one of the core concepts of Rails, it is easy to get fat controllers with to much logic in it.

But even hibernate has issues: typically you call some repository for CRUD actions.  This is better, since you have a shallow object, less dependencies and no magic stuff from the upper class.

But what about situations were calling a repository inside the domain object could lead to more intelligent entity objects?([DDD likes that][8])

In this case you would need to inject services into the entity classes. There are [several approaches][5], but none of them were really acceptable for me (and [others][6]).

AspectJ? Why? What you get is a technical burden and a quite more complicated test setup, because you need the framework in the test setup as well.

Hibernate Interceptors? Maybe simpler than aspects, but why solving this design issue with more complicated technical stuff? Why not just following the plain old [KISS][7] principle?

BTW that´s the reason why I can´t agree with some points of DDD since they are too theoretical, not pragmatic, and introduces [just a lot of new layers][10] into the system.

[1]: http://pragprog.com/book/bksqla/sql-antipatterns
[2]: http://lucene.apache.org/core/
[3]: http://lucene.apache.org/solr/
[4]: http://martinfowler.com/bliki/AnemicDomainModel.html
[5]: http://jblewitt.com/blog/?p=129
[6]: http://techblog.bozho.net/?p=180
[7]: http://en.wikipedia.org/wiki/KISS_principle
[8]: http://gorodinski.com/blog/2012/04/14/services-in-domain-driven-design-ddd/
[9]: http://stackoverflow.com/questions/192220/what-is-the-most-efficient-elegant-way-to-parse-a-flat-table-into-a-tree/192462#192462
[10]: http://in.relation.to/Bloggers/StillConfusedAboutRepositories
