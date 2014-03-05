---
layout: page
title: "Big Data Genomics &en; bdg-formats"
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
project.

bdg-formats is on [Github](https://github.com/bigdatagenomics/bdg-formats).

### Releases

Currently, the bdg-formats schemas are part of the ADAM 0.6.1 release. ADAM is available for
projects using Maven or SBT through the [Sonatype OSS repository](https://docs.sonatype.org/display/Repository).
We are working to deploy artifacts for non-JVM languages; watch this space for more info!

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

ADAM is available under the [Apache 2](http://www.apache.org/licenses/LICENSE-2.0.html)
open source software (OSS) license. This OSS license is non-viral, and places no restrictions on
users who would like to use or modify the software.