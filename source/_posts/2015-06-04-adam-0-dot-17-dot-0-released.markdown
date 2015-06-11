---
layout: post
title: "ADAM 0.17.0 Released"
date: 2015-06-04 10:49:26 -0700
comments: true
categories: 
- ADAM
- releases
---

The 0.17.0 release of ADAM includes a [release for Scala 2.10](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.10-0.17.0) and a [release for Scala 2.11](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.11-0.17.0). We've been working to cleanup APIs and simplify ADAM for developers. Code that isn't useful has been removed. Code that belongs in other downstream or upstream projects has been moved. Parquet and HTSJDK has been upgraded.

There are also some new features, e.g. you can now now `transform` all the SAM/BAM files in a directory by specifying the directory and there's a new `flatten` command that allows you to flatten the schema of ADAM data to process in Impala, Hive, SparkSQL, etc; there are also many bug fixes.

<!-- more -->

For more details, see the following pull requests:

* ISSUE [691](https://github.com/bigdatagenomics/adam/pull/691): fix BAM/SAM header setting when writing on cluster
* ISSUE [688](https://github.com/bigdatagenomics/adam/pull/688): make adamLoad public
* ISSUE [694](https://github.com/bigdatagenomics/adam/pull/694): Fix parent reference in distribution module
* ISSUE [684](https://github.com/bigdatagenomics/adam/pull/684): a few region-join nits
* ISSUE [682](https://github.com/bigdatagenomics/adam/pull/682): [ADAM-681] Remove menacing error message about reqd .adam extension
* ISSUE [680](https://github.com/bigdatagenomics/adam/pull/680): [ADAM-674] Delete Bam2ADAM.
* ISSUE [678](https://github.com/bigdatagenomics/adam/pull/678): upgrade to bdg utils 0.2.1
* ISSUE [668](https://github.com/bigdatagenomics/adam/pull/668): [ADAM-597] Move correction out of ADAM and into a downstream project.
* ISSUE [671](https://github.com/bigdatagenomics/adam/pull/671): Bug fix in ReferenceUtils.unionReferenceSet
* ISSUE [667](https://github.com/bigdatagenomics/adam/pull/667): [ADAM-666] Clean up key not found error in partitioner code.
* ISSUE [656](https://github.com/bigdatagenomics/adam/pull/656): Update Vcf2ADAM.scala
* ISSUE [652](https://github.com/bigdatagenomics/adam/pull/652): added filterByOverlappingRegion in GeneFeatureRDDFunctions
* ISSUE [650](https://github.com/bigdatagenomics/adam/pull/650): [ADAM-649] Support transform of all BAM/SAM files in a directory.
* ISSUE [647](https://github.com/bigdatagenomics/adam/pull/647): [ADAM-646] Special case reads with '*' quality during BQSR.
* ISSUE [645](https://github.com/bigdatagenomics/adam/pull/645): [ADAM-634] Create a local ParquetLister for testing purposes.
* ISSUE [633](https://github.com/bigdatagenomics/adam/pull/633): [Adam] Tests for SAMRecordConverter.scala
* ISSUE [641](https://github.com/bigdatagenomics/adam/pull/641): [ADAM-640] Fix incorrect exclusion for org.seqdoop.htsjdk.
* ISSUE [632](https://github.com/bigdatagenomics/adam/pull/632): [ADAM-631] Allow VCF conversion to sort on output after coalescing.
* ISSUE [628](https://github.com/bigdatagenomics/adam/pull/628): [ADAM-627] Makes ReferenceFile trait extend Serializable.
* ISSUE [637](https://github.com/bigdatagenomics/adam/pull/637): check for mac brew alternate spark install structure
* ISSUE [624](https://github.com/bigdatagenomics/adam/pull/624): Conceptual fix for duplicate marking and sorting stragglers
* ISSUE [629](https://github.com/bigdatagenomics/adam/pull/629): [ADAM-604] Remove normalization code.
* ISSUE [630](https://github.com/bigdatagenomics/adam/pull/630): Add flatten command.
* ISSUE [619](https://github.com/bigdatagenomics/adam/pull/619): [ADAM-540] Move to new HTSJDK release; should support Java 8.
* ISSUE [626](https://github.com/bigdatagenomics/adam/pull/626): [ADAM-625] Enable globbing for BAM.
* ISSUE [621](https://github.com/bigdatagenomics/adam/pull/621): Removes the predicates package.
* ISSUE [620](https://github.com/bigdatagenomics/adam/pull/620): [ADAM-600] Adding RegionJoin trait.
* ISSUE [616](https://github.com/bigdatagenomics/adam/pull/616): [ADAM-565] Upgrade to Parquet filter2 API.
* ISSUE [613](https://github.com/bigdatagenomics/adam/pull/613): [ADAM-612] Point to proper k-mer counters.
* ISSUE [588](https://github.com/bigdatagenomics/adam/pull/588): [ADAM-587] Clean up loading checks.
* ISSUE [592](https://github.com/bigdatagenomics/adam/pull/592): [ADAM-513] Remove ReferenceMappable trait.
* ISSUE [606](https://github.com/bigdatagenomics/adam/pull/606): [ADAM-605] Remove visualization code.
* ISSUE [596](https://github.com/bigdatagenomics/adam/pull/596): [ADAM-595] Delete the 'comparisons' code.
* ISSUE [590](https://github.com/bigdatagenomics/adam/pull/590): [ADAM-589] Removed pileup code.
* ISSUE [586](https://github.com/bigdatagenomics/adam/pull/586): [ADAM-452] Fixes SM attribute on ADAM to BAM conversion.
* ISSUE [584](https://github.com/bigdatagenomics/adam/pull/584): [ADAM-583] Add k-mer counting functionality for nucleotide contig fragments
