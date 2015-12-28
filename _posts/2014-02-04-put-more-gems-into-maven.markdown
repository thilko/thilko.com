---
layout: post
title: "Put more gems into maven"
date: 2014-02-04 19:59
comments: true
categories: ruby java
---
Recently I was brave enough to publish an artifact to the central maven repository. I didn´t know
anything in the beginning and since I knew the process with ruby gems I thought it couldn´t be much
work. How naive.

# How to release a gem

If you would like to publish a ruby gem you have to:

1. **Register at rubygems.org.**
You will get an api token [there][1] which have to be included in your local configuration files.

2. **Setup a project with bundler.**
Basically you just need a project with a .gemspec file, but [bundler][2] is the de facto standard and
provides a nice [bundle gem][3] command which bootstraps a gem project for you.

3. **Build your gem.**
***gem build*** creates a gem from your gemspec file.

4. Install your gem locally
You could skip this step if you are keen enough to upload your gem to rubygems.org without
integration testing. I wouldn´t recommend it.

5. **Push gem to rubygems.org**
Here you can use [gem push][4] or [bundler][5].

[Jeweler][6] or [hoe][7] provide some more help around this steps but I think I already easy enough.
After pushing, your gem is [publicy available][18].

# How to release a maven artifact
Ok, having this steps in mind I started the journey to publish my first maven project. After some
investigation I found a [general][8] and a [gradle][19] guide. Some [support][20] from sonatype is
available. In short, here are the steps:

1. **Create your maven-like project.**
Personally I prefer [gradle][9], you don´t have to enter the 90´s maven xml hell.

2. **Sign up on the [sonatype repository manager][11].**
There is no way to publish an artifact to maven central by yourself. You will need a trusted
authority. This could be [sonatype][10].

3. **Sign up on the [sonatype ticket system][12].**
Create a JIRA ticket for your project, here is an [example][13].

4. **Wait for ticket completion.**
There is a real(!) person who creates the repositories for you. Afterwards you are able to publish
your first snapshots. For me this has worked quite fast. Just a a couple of minutes.

5. **Create a pom that fulfills the [requirements][14].**
If you are in the luck to use gradle you can use your buildscript to generate the pom´s xml. You
will also need to create a sources and javadoc jar.

6. **Sign your maven package.**
You need to sign your maven package with PGP. I did it with [gpgtools][15]. You will need to publish your
key to a central location as well. There is a really helpful [plugin][16] for gradle.

7. **Publish your first snapshot version.**
You have snapshot, staging and release repositories. The release repositories are synced every 20
minutes with the central maven repository. You can use your first snapshot versions for testing
purposes.

8. **Publish your first staging version**
The staging repository is used to ***promote*** release versions.

9. **Promote staging version**
Use the nexus repository manager to promote the staged artifact as release version. Nexus will
perform various automatic(!) checks if your maven artifact is valid against the required pom
structure. If it´s valid, it is promoted as release.

10. **Release your release**
Comment your JIRA ticket(you remember?) that you promoted your first release. They will check it and
activate the automatic sync to the maven central server. Next time your promoted release is synced
automatically.

Welcome your project in the [java ecosystem][17]! Hooray! Couldn´t be easier... not! Wouldn´t be
nice to make the things more e-a-s-y?

[1]: http://rubygems.org
[2]: http://bundler.io
[3]: http://bundler.io/v1.5/bundle_gem.html
[4]: http://guides.rubygems.org/command-reference/#gem_push
[5]: https://github.com/radar/guides/blob/master/gem-development.md
[6]: https://github.com/technicalpickles/jeweler
[7]: https://github.com/seattlerb/hoe
[8]: http://jroller.com/holy/entry/releasing_a_project_to_maven
[9]: http://www.gradle.org/
[10]: http://www.sonatype.com/
[11]: https://oss.sonatype.org/
[12]: https://issues.sonatype.org/
[13]: https://issues.sonatype.org/browse/OSSRH-141
[14]: https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide#SonatypeOSSMavenRepositoryUsageGuide-6.CentralSyncRequirement
[15]: https://gpgtools.org/
[16]: http://www.gradle.org/docs/current/userguide/signing_plugin.html
[17]: http://search.maven.org/
[18]: https://rubygems.org/gems/ficsr
[19]: http://jedicoder.blogspot.de/2011/11/automated-gradle-project-deployment-to.html
[20]: https://support.sonatype.com/entries/21580432-how-do-i-configure-my-gradle-build-to-publish-artifacts-to-nexus
