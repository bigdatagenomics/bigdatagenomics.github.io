---
layout: page
title: "Big Data Genomics &en; ADAM"
date: 2014-03-04 19:46
comments: true
sharing: true
footer: true
---

### ADAM: Data Alignment/Map

ADAM provides both an application programming interface (API) and a command line interface (CLI)
for manipulating genomic data on a computing cluster. ADAM operates on data stored inside of
[Parquet](http://www.parquet.io) with the [bdg-formats](/projects/bdg-formats/) schemas,
using [Apache Spark](http://spark.apache.org/), and provides scalable performance on clusters
larger than 100 machines.

ADAM is on [Github](https://github.com/bigdatagenomics/adam). Quick start guides are available
for [running ADAM on EC2](https://github.com/bigdatagenomics/adam/wiki/Running-ADAM-on-EC2), and
for [building ADAM for specific CDH releases](https://github.com/bigdatagenomics/adam/wiki/Running-ADAM-on-CDH-4-or-5).

### Releases

The latest available release of ADAM is 0.6.1. ADAM is available for projects using Maven or SBT
through the [Sonatype OSS repository](https://docs.sonatype.org/display/Repository).

### Support

For support using ADAM, please contact the [ADAM developer mailing list](/mail/). Additionally, we
track issues and feature enhancement requests through our
[Github issue tracker](https://github.com/bigdatagenomics/adam/issues).

### Citing

ADAM has been described in a UC Berkeley EECS [technical report](http://www.eecs.berkeley.edu/Pubs/TechRpts/2013/EECS-2013-207.html).
The Bibtex for this reference is:

```
@techreport{Massie:EECS-2013-207,
    Author = {Massie, Matt and Nothaft, Frank and Hartl, Christopher and Kozanitis, Christos and Schumacher, André and Joseph, Anthony D. and Patterson, David A.},
    Title = {ADAM: Genomics Formats and Processing Patterns for Cloud Scale Computing},
    Institution = {EECS Department, University of California, Berkeley},
    Year = {2013},
    Month = {Dec},
    URL = {http://www.eecs.berkeley.edu/Pubs/TechRpts/2013/EECS-2013-207.html},
    Number = {UCB/EECS-2013-207}
}
```

### Licensing

ADAM is available under the [Apache 2](http://www.apache.org/licenses/LICENSE-2.0.html)
open source software (OSS) license. This OSS license is non-viral, and places no restrictions on
users who would like to use or modify the software.