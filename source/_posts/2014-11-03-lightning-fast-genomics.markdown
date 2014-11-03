---
layout: post
title: "Lightning Fast Genomics"
date: 2014-11-03 08:24:15 -0800
comments: true
categories:
- ADAM
- talks
---

[Andy Petrella](https://twitter.com/noootsab) and [Xavier Tordoir](https://twitter.com/xtordoir) gave a talk, *[Scalable Genomics with ADAM](http://scala.io/talks.html#/#SVK-108)*, at [Scala.IO](http://scala.io/) in Paris, France.

> We are at a time where biotech allow us to get personal genomes for $1000. Tremendous progress since the 70s in DNA sequencing have been done, e.g. more samples in an experiment, more genomic coverages at higher speeds. Genomic analysis standards that have been developed over the years weren't designed with scalability and adaptability in mind. In this talk, we’ll present a game changing technology in this area, ADAM, initiated by the AMPLab at Berkeley. ADAM is framework based on Apache Spark and the Parquet storage. We’ll see how it can speed up a sequence reconstruction to a factor 150.

Andy and Xavier's talk included a demo: using Spark's MLlib to do population stratification across 1000 Genomes in just a few minutes in the cloud using Amazon Web Services (AWS). Their talk highlights the advantages of building on open-source technologies, like Apache [Spark](http://spark.apache.org) and [Parquet](http://parquet.io), designed for performance and scale.

Andy also modified the [Scala Notebook](https://github.com/Bridgewater/scala-notebook) to create [Spark Notebook](https://github.com/andypetrella/spark-notebook) which enables visualization and reproducible analysis on Apache Spark inside a web browser. A great addition to the Spark ecosystem!

<iframe src="//www.slideshare.net/slideshow/embed_code/40715122" width="850" height="710" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//fr.slideshare.net/noootsab/lightning-fast-genomics-with-spark-adam-and-scala" title="Lightning fast genomics with Spark, Adam and Scala" target="_blank">Lightning fast genomics with Spark, Adam and Scala</a> </strong> from <strong><a href="//www.slideshare.net/noootsab" target="_blank">noootsab</a></strong> </div>
