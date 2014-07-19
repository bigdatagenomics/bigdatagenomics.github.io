---
layout: page
title: "Big Data Genomics &mdash; bdg-formats"
date: 2014-03-04 19:46
comments: true
sharing: true
footer: true
---

### bdg-formats: Schemas for genomic data

The bdg-formats project provides schemas for describing common genomic data, such as reads, reference
oriented data, variants, genotypes, and assemblies. The schemas developed by this project use
[Apache Avro](http://avro.apache.org), which allows use across most common programming languages
and platforms. These schemas form the core data structures used in the [ADAM](/projects/adam/)
project. bdg-formats is on [Github](https://github.com/bigdatagenomics/bdg-formats).

### Releases

The latest stable release of the bdg-formats is 0.1.0. Java artifacts are available for projects using Maven or SBT
through Maven Central. Snapshots are available from the [Sonatype OSS repository](https://docs.sonatype.org/display/Repository).
The dependency for the data formats is:

```
<dependency>
  <groupId>org.bdgenomics.bdg-formats</groupId>
  <artifactId>bdg-formats</artifactId>
  <version>0.1.0</version>
</dependency>
```

The changelist per release can be found [here](https://github.com/bigdatagenomics/bdg-services/blob/master/CHANGES.md).
We hope to make C/C++ and Python artifacts available soon.

### Support

The bdg-formats parallel the development of ADAM. For support, please contact the
[ADAM developer mailing list](/mail/). Additionally, we track issues and feature enhancement
requests through our [Github issue tracker](https://github.com/bigdatagenomics/bdg-formats/issues).

### Citing

An early version of the bdg-formats schemas was been described in the UC Berkeley EECS
[technical report](http://www.eecs.berkeley.edu/Pubs/TechRpts/2013/EECS-2013-207.html) for the
ADAM project. The Bibtex for this reference is:

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

The bdg-formats are available under the [Apache 2](http://www.apache.org/licenses/LICENSE-2.0.html)
open source software (OSS) license. This OSS license is non-viral, and places no restrictions on
users who would like to use or modify the software.