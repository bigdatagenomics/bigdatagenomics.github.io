---
layout: post
title: "Developing Big Data Genomics: A Screencast"
date: 2014-05-15 12:00:00 -0500
comments: true
categories:
---

<iframe id="ytplayer" type="text/html" width="640" height="390" src="http://www.youtube.com/embed/BCoIXqUfFkU?autoplay=0&origin=http://bdgenomics.org" frameborder="0"></iframe>

<br/><br/>

This short screencast is meant to get someone new to Scala, IntelliJ, and the Big Data Genomics stack up and running with a configured development environment suitable for working with or on projects like ADAM and Avocado.

We'll walk you through downloading the appropriate JDK, IntelliJ IDE, and plugings. Then we will set up the project (using ADAM as the example), generating sources, packaging the application, and building the project. Finally, we cover running tests, as well as some basic exploration and code navigation using the IDE.

---

Note, if you have trouble using `mvn package` in the command line, you may want to add the following to your `.bashrc`, or at least export these environment variables before running `mvn package`:

    export MAVEN_OPTS="-Xmx512m -XX:MaxPermSize=128M"
    export JAVA_HOME=`/usr/libexec/java_home -v 1.7`

### Links

* 00:00:14  [Oracle JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)
* 00:00:28  [IntelliJ IDE](http://www.jetbrains.com/idea/download/)
* 00:00:43  [ADAM Github Repository](https://github.com/bigdatagenomics/adam)
