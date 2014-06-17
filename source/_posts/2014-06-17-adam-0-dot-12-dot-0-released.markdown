---
layout: post
title: "ADAM 0.12.0 Released"
date: 2014-06-17 15:35:12 -0700
comments: true
categories: 
---

ADAM [0.12.0](https://github.com/bigdatagenomics/adam/releases) is now available!

This release includes new Parquet utilities for reading/writing Parquet directly on S3, eliminating
the need to transfer data from S3 to HDFS for processing. This release also upgrades
ADAM to Spark 1.0 and provides new schema definitions, bug fixes and features:

* ISSUE [264](https://github.com/bigdatagenomics/adam/pull/264): Parquet-related Utility Classes
* ISSUE [259](https://github.com/bigdatagenomics/adam/pull/259): ADAMFlatGenotype is a smaller, flat version of a genotype schema
* ISSUE [266](https://github.com/bigdatagenomics/adam/pull/266): Removed extra command 'BuildInformation'
* ISSUE [263](https://github.com/bigdatagenomics/adam/pull/263): Added AdamContext.referenceLengthFromCigar
* ISSUE [260](https://github.com/bigdatagenomics/adam/pull/260): Modifying conversion code to resolve #112.
* ISSUE [258](https://github.com/bigdatagenomics/adam/pull/258): Adding an 'args' parameter to the plugin framework.
* ISSUE [262](https://github.com/bigdatagenomics/adam/pull/262): Adding reference assembly name to ADAMContig.
* ISSUE [256](https://github.com/bigdatagenomics/adam/pull/256): Upgrading to Spark 1.0
* ISSUE [257](https://github.com/bigdatagenomics/adam/pull/257): Adds toString method for sequence dictionary.
* ISSUE [255](https://github.com/bigdatagenomics/adam/pull/255): Add equals, canEqual, and hashCode methods to MdTag class
