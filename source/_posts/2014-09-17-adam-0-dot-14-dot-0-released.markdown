---
layout: post
title: "ADAM 0.14.0 Released"
date: 2014-09-17 14:33:35 -0700
comments: true
categories: 
- ADAM
- Releases
---

ADAM [0.14.0](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-0.14.0) is now available. Special thanks to Arun Ahuja, Michael L Heuer, Uri Laserson, Frank Nothaft, Andy Petrella and Ryan Williams for their contributions to this release!

This release uses the [newly-released Apache Spark 1.1.0](https://spark.apache.org/releases/spark-release-1-1-0.html) which brings operational and performance improvements in Spark core. Two new scripts, `adam-shell` and `adam-submit`, allow you to use ADAM via the Spark shell or the Spark submit script in addition to the ADAM CLI.

The [Hadoop-BAM](http://sourceforge.net/projects/hadoop-bam/) team is now publishing [their artifacts to Maven Central](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22org.seqdoop%22) (yea!) so we no longer rely on snapshot releases. ADAM `0.14.0` uses the `7.0.0` release of Hadoop-BAM.

This release also adds a new Java plugin interface, improves MD tag processing as well as fixes numerous bugs.

We hope that you enjoy this release. Drop by `#adamdev` on freenode.net, [follow us on Twitter](https://twitter.com/bigdatagenomics) or [subscribe to our mailing list](http://bdgenomics.org/mail/) to stay in touch.

<!-- more -->

For more details, see the changelog below:

* ISSUE [376](https://github.com/bigdatagenomics/adam/pull/376): [ADAM-375] Upgrade to Hadoop-BAM 7.0.0.
* ISSUE [378](https://github.com/bigdatagenomics/adam/pull/378): [ADAM-360] Upgrade to Spark 1.1.0.
* ISSUE [379](https://github.com/bigdatagenomics/adam/pull/379): Fix the position of the jar path in the submit.
* ISSUE [383](https://github.com/bigdatagenomics/adam/pull/383): Make Mdtags handle '=' and 'X' cigar operators
* ISSUE [369](https://github.com/bigdatagenomics/adam/pull/369): [ADAM-369] Improve debug output for indel realigner
* ISSUE [377](https://github.com/bigdatagenomics/adam/pull/377): [ADAM-377] Update to Jenkins scripts and README.
* ISSUE [374](https://github.com/bigdatagenomics/adam/pull/374): [ADAM-372][ADAM-371][ADAM-365] Refactoring CLI to simplify and integrate with Spark model better
* ISSUE [370](https://github.com/bigdatagenomics/adam/pull/370): [ADAM-367] Updated alias in README.md
* ISSUE [368](https://github.com/bigdatagenomics/adam/pull/368): erasure, nonexhaustive-match, deprecation warnings
* ISSUE [354](https://github.com/bigdatagenomics/adam/pull/354): [ADAM-353] Fixing issue with SAM/BAM/VCF header attachment when running distributed
* ISSUE [357](https://github.com/bigdatagenomics/adam/pull/357): [ADAM-357] Added Java Plugin hook for ADAM.
* ISSUE [352](https://github.com/bigdatagenomics/adam/pull/352): Fix failing MD tag
* ISSUE [363](https://github.com/bigdatagenomics/adam/pull/363): Adding maven assembly plugin configuration to create tarballs
* ISSUE [364](https://github.com/bigdatagenomics/adam/pull/364): [ADAM-364] Fixing remaining cs.berkeley.edu URLs.
* ISSUE [362](https://github.com/bigdatagenomics/adam/pull/362): Remove mention of uberjar from README
