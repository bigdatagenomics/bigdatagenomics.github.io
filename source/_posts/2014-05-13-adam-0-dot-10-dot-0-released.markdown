---
layout: post
title: "ADAM 0.10.0 Released"
date: 2014-05-13 15:18:06 -0700
comments: true
categories: 
---

ADAM [0.10.0](https://github.com/bigdatagenomics/adam/releases) is now available.

## Parquet 1.4.3 upgrade

As developers, you need to know about a small (but significant) change to way Parquet projections work now. In the past, any record fields that were not in the projections were returned as null values. With our upgrade to Parquet 1.4.3, fields that are not in the your projections will be set to their default value (if one exists).

## BQSR Performance and Concordance

This release adds the cycle covariate to BQSR. Our BQSR implementation can process the 1000g NA12878 High-Coverage Whole Genome in under 45 minutes using 100 EC2 nodes. Our concordance with GATK is high (>99.9%) for the data we've tested. We'll publish the scripts we use for concordance testing in an upcoming release to allow broader, automated testing. Additionally, ADAM now processes the OQ tag to make comparing original quality scores easier.

## Lots of other bug fixes and features...

* ISSUE [242](https://github.com/bigdatagenomics/adam/pull/242): Upgrade to Parquet 1.4.3
* ISSUE [241](https://github.com/bigdatagenomics/adam/pull/241): Fixes to FASTA code to properly handle indices.
* ISSUE [239](https://github.com/bigdatagenomics/adam/pull/239): Make ADAMVCFOutputFormat public
* ISSUE [233](https://github.com/bigdatagenomics/adam/pull/233): Build up reference information during cigar processing
* ISSUE [234](https://github.com/bigdatagenomics/adam/pull/234): Predicate to filter conversion
* ISSUE [235](https://github.com/bigdatagenomics/adam/pull/235): Remove unused contiglength field
* ISSUE [232](https://github.com/bigdatagenomics/adam/pull/232): Add `-pretty` and `-o` to the `print` command
* ISSUE [230](https://github.com/bigdatagenomics/adam/pull/230): Remove duplicate mdtag field
* ISSUE [231](https://github.com/bigdatagenomics/adam/pull/231): Helper scripts to run an ADAM Console.
* ISSUE [226](https://github.com/bigdatagenomics/adam/pull/226): Fix ReferenceRegion from ADAMRecord
* ISSUE [225](https://github.com/bigdatagenomics/adam/pull/225): Change Some to Option to check for unmapped reads
* ISSUE [223](https://github.com/bigdatagenomics/adam/pull/223): Use SparkConf object to configure SparkContext
* ISSUE [217](https://github.com/bigdatagenomics/adam/pull/217): Stop using reference IDs and use reference names instead
* ISSUE [220](https://github.com/bigdatagenomics/adam/pull/220): Update SAM to ADAM conversion
* ISSUE [213](https://github.com/bigdatagenomics/adam/pull/213): BQSR updates

