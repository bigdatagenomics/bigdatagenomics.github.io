---
layout: post
title: "ADAM 0.15.0 Released"
date: 2014-11-26 10:27:00 -0800
comments: true
categories: 
- ADAM
- Releases
---

We're proud to announce the [release of ADAM 0.15.0](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-0.15.0)!

This release includes important memory and performance improvements, better documentation, new features and many bug fixes.

We have upgraded from Parquet `1.4.3` to `1.6.0` in order to dramatically reduce our memory footprint. For string columns with dictionary encoding, the amount of memory used will now be proportional to the number of dictionary entries instead of the number of records materialized. Parquet 1.6.0 also provides improved column statistics and the ability to store custom metadata. We will use these features in subsequent ADAM releases to improve random access performance. Note that ADAM `0.14.0` had a serious memory regression so upgrading to `0.15.0` as soon as possible is recommended.

We are unhappy with the quality of the documentation we have been providing ADAM users and are working to improve it. With this release, all documentation has been centralized into the `./docs` directory and we're using `pandoc` to convert the Markdown source into both PDF and HTML formats. We are committed to improving the content of the docs over time and welcome your pull requests!

This release includes [binary distributions](https://repo1.maven.org/maven2/org/bdgenomics/adam/adam-distribution/0.15.0/) to make it easier for you to get up and running with ADAM. We do not include any Spark or Hadoop artifacts in order to prevent versioning conflicts. For application developers, we have also changed our Spark and Hadoop dependencies to `provided`. This means that you can more easily running on ADAM using your preferred Spark and Hadoop version and configuration. We want to make deployment as easier as possible.

This release includes numerous features and bug fixes that are detailed below:

<!-- more -->

* ISSUE [509](https://github.com/bigdatagenomics/adam/pull/509): Add a 'distribution' module to create assemblies
* ISSUE [508](https://github.com/bigdatagenomics/adam/pull/508): Upgrade from Parquet 1.4.3 to 1.6.0rc4
* ISSUE [498](https://github.com/bigdatagenomics/adam/pull/498): [ADAM-496] Changes VCF to flat ADAM command name and usage
* ISSUE [500](https://github.com/bigdatagenomics/adam/pull/500): [ADAM-495] Require SPARK_HOME for adam-submit
* ISSUE [501](https://github.com/bigdatagenomics/adam/pull/501): [ADAM-499] Add -onlyvariants option to vcf2adam
* ISSUE [507](https://github.com/bigdatagenomics/adam/pull/507): [ADAM-505] Removed `adam-local` from docs
* ISSUE [504](https://github.com/bigdatagenomics/adam/pull/504): [ADAM-502] Add missing Long implicit to ColumnReaderInput
* ISSUE [503](https://github.com/bigdatagenomics/adam/pull/503): [ADAM-473] Make RecordCondition and FieldCondition public
* ISSUE [494](https://github.com/bigdatagenomics/adam/pull/494): Fix foreach block for vcf ingest
* ISSUE [492](https://github.com/bigdatagenomics/adam/pull/492): Documentation cleanup and style improvements
* ISSUE [481](https://github.com/bigdatagenomics/adam/pull/481): [ADAM-480] Switch assembly to single goal.
* ISSUE [487](https://github.com/bigdatagenomics/adam/pull/487): [ADAM-486] Add port option to viz command.
* ISSUE [469](https://github.com/bigdatagenomics/adam/pull/469): [ADAM-461] Fix ReferenceRegion and ReferencePosition impl
* ISSUE [440](https://github.com/bigdatagenomics/adam/pull/440): [ADAM-439] Fix ADAM to account for BDG-FORMATS-35: Avro uses Strings
* ISSUE [470](https://github.com/bigdatagenomics/adam/pull/470): added ReferenceMapping for Genotype, filterByOverlappingRegion for GenotypeRDDFunctions
* ISSUE [468](https://github.com/bigdatagenomics/adam/pull/468): refactor RDD loading; explicitly load alignments
* ISSUE [474](https://github.com/bigdatagenomics/adam/pull/474): Consolidate documentation into a single location in source.
* ISSUE [471](https://github.com/bigdatagenomics/adam/pull/471): Fixed typo on MAVEN_OPTS quotation mark
* ISSUE [467](https://github.com/bigdatagenomics/adam/pull/467): [ADAM-436] Optionally output original qualities to fastq
* ISSUE [451](https://github.com/bigdatagenomics/adam/pull/451): add `adam view` command, analogous to `samtools view`
* ISSUE [466](https://github.com/bigdatagenomics/adam/pull/466): working examples on .sam included in repo
* ISSUE [458](https://github.com/bigdatagenomics/adam/pull/458): Remove unused val from Reads2Ref
* ISSUE [438](https://github.com/bigdatagenomics/adam/pull/438): Add ability to save paired-FASTQ files
* ISSUE [457](https://github.com/bigdatagenomics/adam/pull/457): A few random Predicate-related cleanups
* ISSUE [459](https://github.com/bigdatagenomics/adam/pull/459): a few tweaks to scripts/jenkins-test
* ISSUE [460](https://github.com/bigdatagenomics/adam/pull/460): Project only the sequence when kmer/qmer counting
* ISSUE [450](https://github.com/bigdatagenomics/adam/pull/450): Refactor some file writing and reading logic
* ISSUE [455](https://github.com/bigdatagenomics/adam/pull/455): [ADAM-454] Add serializers for Avro objects which don't have serializers
* ISSUE [447](https://github.com/bigdatagenomics/adam/pull/447): Update the contribution guidelines
* ISSUE [453](https://github.com/bigdatagenomics/adam/pull/453): Better null handling for isSameContig utility
* ISSUE [417](https://github.com/bigdatagenomics/adam/pull/417): Stores original position and original cigar during realignment.
* ISSUE [449](https://github.com/bigdatagenomics/adam/pull/449): read “OQ” attr from structured SAMRecord field
* ISSUE [446](https://github.com/bigdatagenomics/adam/pull/446): Revert "[ADAM-237] Migrate to Chill serialization libraries."
* ISSUE [437](https://github.com/bigdatagenomics/adam/pull/437): random nits
* ISSUE [434](https://github.com/bigdatagenomics/adam/pull/434): Few transform tweaks
* ISSUE [435](https://github.com/bigdatagenomics/adam/pull/435): [ADAM-403] Remove seqDict from RegionJoin
* ISSUE [431](https://github.com/bigdatagenomics/adam/pull/431): A few tweaks, typo corrections, and random cleanups
* ISSUE [430](https://github.com/bigdatagenomics/adam/pull/430): [ADAM-429] adam-submit now handles args correctly.
* ISSUE [427](https://github.com/bigdatagenomics/adam/pull/427): Fixes for indel realigner issues
* ISSUE [418](https://github.com/bigdatagenomics/adam/pull/418): [ADAM-416] Removing 'ADAM' prefix
* ISSUE [404](https://github.com/bigdatagenomics/adam/pull/404): [ADAM-327] Adding gene, transcript, and exon models.
* ISSUE [414](https://github.com/bigdatagenomics/adam/pull/414): Fix error in `adam-local` alias
* ISSUE [415](https://github.com/bigdatagenomics/adam/pull/415): Update README.md to reflect Spark 1.1
* ISSUE [412](https://github.com/bigdatagenomics/adam/pull/412): [ADAM-411] Updated usage aliases in README. Fixes #411.
* ISSUE [408](https://github.com/bigdatagenomics/adam/pull/408): [ADAM-405] Add FASTQ output.
* ISSUE [385](https://github.com/bigdatagenomics/adam/pull/385): [ADAM-384] Adds import from FASTQ.
* ISSUE [400](https://github.com/bigdatagenomics/adam/pull/400): [ADAM-399] Fix link to schemas.
* ISSUE [396](https://github.com/bigdatagenomics/adam/pull/396): [ADAM-388] Sets Kryo serialization with --conf args
* ISSUE [394](https://github.com/bigdatagenomics/adam/pull/394): [ADAM-393] Adds knobs to SparkContext creation in SparkFunSuite
* ISSUE [391](https://github.com/bigdatagenomics/adam/pull/391): [ADAM-237] Migrate to Chill serialization libraries.
* ISSUE [380](https://github.com/bigdatagenomics/adam/pull/380): Rewrite of MarkDuplicates which seems to improve performance
* ISSUE [387](https://github.com/bigdatagenomics/adam/pull/387): fix some deprecation warnings

