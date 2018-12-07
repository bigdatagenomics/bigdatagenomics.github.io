---
layout: post
title: "ADAM 0.25.0 and Cannoli 0.3.0 Released"
date: 2018-12-01
comments: true
categories: 
---

ADAM [version 0.25.0](https://github.com/bigdatagenomics/adam/releases) and
Cannoli [version 0.3.0](https://github.com/bigdatagenomics/cannoli/releases) have been released!

Since the 0.24.0 release of ADAM, more then 40 issues have been closed, including bug fixes around
indexed reads and attributes in VCF. New features include additional filter by methods and multi-sample
coverage. The ADAM Python APIs now support Python 3.

Based on feedback from the [2018 GCCBOSC bioinformatics community conference](https://www.open-bio.org/wiki/BOSC_2018),
at [2018 GCCBOSC CollaborationFest](https://galaxyproject.org/events/gccbosc2018/collaboration/) the Cannoli API
was refactored to greatly improve interactive use in `cannoli-shell` (a Scala REPL based on Spark Shell, similar
to `adam-shell`) and notebooks such as [Jupyter](https://jupyter.org/), [Zeppelin](https://zeppelin.apache.org/),
and [Spark Notebook](http://spark-notebook.io/).

For example, here is an entire variant calling pipeline, based on bwa, ADAM, and Freebayes

{% codeblock %}
import org.bdgenomics.adam.rdd.ADAMContext._
import org.bdgenomics.cannoli.cli._
import org.bdgenomics.cannoli.cli.Cannoli._

val sample = "sample"
val reference = "ref.fa"

val reads = sc.loadPairedFastqAsFragments(sample + "_1.fq", sample + "_2.fq")

val bwaArgs = new BwaArgs()
bwaArgs.sample = sample
bwaArgs.indexPath = reference

val alignments = reads.alignWithBwa(bwaArgs)
val sorted = alignments.sortReadsByReferencePositionAndIndex()
val markdup = sorted.markDuplicates()

val freebayesArgs = new FreebayesArgs()
freebayesArgs.referencePath = reference

val variantContexts = markdup.callVariantsWithFreebayes(freebayesArgs)

variantContexts.saveAsVcf(sample + ".freebayes.vcf.bgzf")
{% endcodeblock %}


Changes since Previous Releases
===============================

The full list of changes to ADAM since version 0.24.0 and Cannoli since version 0.2.0 are below.

<!-- more -->

### ADAM version 0.25.0 ###

**Closed issues:**

 - Expand illumina metadata regex to include "N" character  [\#2079](https://github.com/bigdatagenomics/adam/issues/2079)
 - Remove support for Hadoop 2.6 [\#2073](https://github.com/bigdatagenomics/adam/issues/2073)
 - NumberFormatException: For input string: "nan" in VCF [\#2068](https://github.com/bigdatagenomics/adam/issues/2068)
 - Support Spark 2.3.2 [\#2062](https://github.com/bigdatagenomics/adam/issues/2062)
 - Arrays should be passed to HTSJDK in the JVM primitive type [\#2059](https://github.com/bigdatagenomics/adam/issues/2059)
 - toCoverage() function for alignments does not distinguish samples [\#2049](https://github.com/bigdatagenomics/adam/issues/2049)
 - Building from adam-core module directory fails to generate Scala code for sql package [\#2047](https://github.com/bigdatagenomics/adam/issues/2047)
 - Data Sets [\#2043](https://github.com/bigdatagenomics/adam/issues/2043)
 - saveAsBed writes missing score values as '.' instead of '0' [\#2039](https://github.com/bigdatagenomics/adam/issues/2039)
 - Fix GFF3 parser to handle trailing FASTA [\#2037](https://github.com/bigdatagenomics/adam/issues/2037)
 - Add StorageLevel as an optional parameter to loadPairedFastq [\#2032](https://github.com/bigdatagenomics/adam/issues/2032)
 - Error: File name too long when building on encrypted file system [\#2031](https://github.com/bigdatagenomics/adam/issues/2031)
 - Fail to transform a VCF  file containing multiple genome data (Muliple sample) [\#2029](https://github.com/bigdatagenomics/adam/issues/2029)
 - Dataset and RDD constructors are missing from CoverageRDD [\#2027](https://github.com/bigdatagenomics/adam/issues/2027)
 - How to create a single RDD[Genotype] object out of multiple VCF files? [\#2025](https://github.com/bigdatagenomics/adam/issues/2025)
 - ReadTheDocs github banner is broken [\#2020](https://github.com/bigdatagenomics/adam/issues/2020)
 - -realign_indels throws serialization error with instrumentation enabled [\#2007](https://github.com/bigdatagenomics/adam/issues/2007)
 - Support 0 length FASTQ reads [\#2006](https://github.com/bigdatagenomics/adam/issues/2006)
 - Speed of Reading into ADAM RDDs from S3 [\#2003](https://github.com/bigdatagenomics/adam/issues/2003)
 - Support Python 3 [\#1999](https://github.com/bigdatagenomics/adam/issues/1999)
 - Unordered list of region join types in doc is missing nested levels [\#1997](https://github.com/bigdatagenomics/adam/issues/1997)
 - Add VariantContextRDD.saveAsPartitionedParquet, ADAMContext.loadPartitionedParquetVariantContexts [\#1996](https://github.com/bigdatagenomics/adam/issues/1996)
 - VCF annotation question [\#1994](https://github.com/bigdatagenomics/adam/issues/1994)
 - Fastq reader clips long reads at 10,000 bp [\#1992](https://github.com/bigdatagenomics/adam/issues/1992)
 - adam-submit Error: Number of executors must be a positive number on EMR 5.13.0/Spark 2.3.0 [\#1991](https://github.com/bigdatagenomics/adam/issues/1991)
 - Test against Spark 2.3.1, Parquet 1.8.3 [\#1989](https://github.com/bigdatagenomics/adam/issues/1989)
 - END does not get set when writing a gVCF [\#1988](https://github.com/bigdatagenomics/adam/issues/1988)
 - Support saving single files to filesystems that don't implement getScheme [\#1984](https://github.com/bigdatagenomics/adam/issues/1984)
 - Add additional filter by convenience methods [\#1978](https://github.com/bigdatagenomics/adam/issues/1978)
 - Limiting FragmentRDD pipe paralellism [\#1977](https://github.com/bigdatagenomics/adam/issues/1977)
 - Consider javadoc.io for API documentation linking [\#1976](https://github.com/bigdatagenomics/adam/issues/1976)
 - FASTQ Reader leaks connections [\#1974](https://github.com/bigdatagenomics/adam/issues/1974)
 - Update bioconda recipe for version 0.24.0 [\#1971](https://github.com/bigdatagenomics/adam/issues/1971)
 - Update homebrew formula at brewsci/homebrew-bio for version 0.24.0 [\#1970](https://github.com/bigdatagenomics/adam/issues/1970)
 - loadPartitionedParquetAlignments fails with Reference.all [\#1967](https://github.com/bigdatagenomics/adam/issues/1967)
 - Caused by: java.lang.VerifyError: class com.fasterxml.jackson.module.scala.ser.ScalaIteratorSerializer overrides final method withResolved [\#1953](https://github.com/bigdatagenomics/adam/issues/1953)
 - FASTQ input format needs to support index sequences [\#1697](https://github.com/bigdatagenomics/adam/issues/1697)
 - Changelog must be edited and committed manually during release process [\#936](https://github.com/bigdatagenomics/adam/issues/936)

**Merged and closed pull requests:**

 - added pyspark mock modules for API documentation [\#2084](https://github.com/bigdatagenomics/adam/pull/2084) ([akmorrow13](https://github.com/akmorrow13))
 - Added mock python modules for API python documentation [\#2082](https://github.com/bigdatagenomics/adam/pull/2082) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-2079] Expand illumina metadata regex to include "N" character [\#2081](https://github.com/bigdatagenomics/adam/pull/2081) ([pauldwolfe](https://github.com/pauldwolfe))
 - ADAM-2079 Added "N" to regexs for illumina metadata [\#2080](https://github.com/bigdatagenomics/adam/pull/2080) ([pauldwolfe](https://github.com/pauldwolfe))
 - Update docs with new template and documentation [\#2078](https://github.com/bigdatagenomics/adam/pull/2078) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1992] Make maximum FASTQ read length configurable. [\#2077](https://github.com/bigdatagenomics/adam/pull/2077) ([heuermh](https://github.com/heuermh))
 -  [ADAM-2059] Properly pass back primitive typed arrays to HTSJDK. [\#2075](https://github.com/bigdatagenomics/adam/pull/2075) ([heuermh](https://github.com/heuermh))
 - Update dependency versions, including htsjdk to 2.16.1 and guava to 27.0-jre [\#2072](https://github.com/bigdatagenomics/adam/pull/2072) ([heuermh](https://github.com/heuermh))
 - [ADAM-1999] Support Python 3 [\#2070](https://github.com/bigdatagenomics/adam/pull/2070) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-2068] Prevent NumberFormatException for nan vs NaN in VCF files. [\#2069](https://github.com/bigdatagenomics/adam/pull/2069) ([heuermh](https://github.com/heuermh))
 - Update python MAKE file [\#2067](https://github.com/bigdatagenomics/adam/pull/2067) ([Georgehe4](https://github.com/Georgehe4))
 - Update python MAKE file [\#2066](https://github.com/bigdatagenomics/adam/pull/2066) ([Georgehe4](https://github.com/Georgehe4))
 - Update jenkins script to test python 3.6 [\#2060](https://github.com/bigdatagenomics/adam/pull/2060) ([Georgehe4](https://github.com/Georgehe4))
 - [ADAM-2062] Update Spark version to 2.3.2 [\#2055](https://github.com/bigdatagenomics/adam/pull/2055) ([heuermh](https://github.com/heuermh))
 - Clean up fields and doc in fragment. [\#2054](https://github.com/bigdatagenomics/adam/pull/2054) ([heuermh](https://github.com/heuermh))
 - [ADAM-2037] Support GFF3 files containing FASTA formatted sequences. [\#2053](https://github.com/bigdatagenomics/adam/pull/2053) ([heuermh](https://github.com/heuermh))
 - modified CoverageRDD and FeatureRDD to extend MultisampleGenomicDataset [\#2051](https://github.com/bigdatagenomics/adam/pull/2051) ([akmorrow13](https://github.com/akmorrow13))
 - Multi-sample coverage [\#2050](https://github.com/bigdatagenomics/adam/pull/2050) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-2047] Use source directory relative to project.basedir for adam codegen. [\#2048](https://github.com/bigdatagenomics/adam/pull/2048) ([heuermh](https://github.com/heuermh))
 - [ADAM-2039] Adding support for writing BED format per UCSC definition [\#2042](https://github.com/bigdatagenomics/adam/pull/2042) ([heuermh](https://github.com/heuermh))
 - Update Jenkins Spark version to 2.2.2 [\#2035](https://github.com/bigdatagenomics/adam/pull/2035) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-2032] Add StorageLevel as an optional parameter to loadPairedFastq [\#2033](https://github.com/bigdatagenomics/adam/pull/2033) ([heuermh](https://github.com/heuermh))
 - [ADAM-2027] Add RDD and Dataset constructors to CoverageRDD. [\#2028](https://github.com/bigdatagenomics/adam/pull/2028) ([heuermh](https://github.com/heuermh))
 - Allow for export of query name sorted SAM files [\#2026](https://github.com/bigdatagenomics/adam/pull/2026) ([karenfeng](https://github.com/karenfeng))
 - [ADAM-2020] Fix ReadTheDocs Github banner. [\#2021](https://github.com/bigdatagenomics/adam/pull/2021) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1988] Add copyVariantEndToAttribute method to support gVCF END attribute … [\#2017](https://github.com/bigdatagenomics/adam/pull/2017) ([heuermh](https://github.com/heuermh))
 - [ADAM-936] Use github-changes-maven-plugin to update CHANGES.md. [\#2014](https://github.com/bigdatagenomics/adam/pull/2014) ([heuermh](https://github.com/heuermh))
 - [ADAM-1992] Make maximum FASTQ read length configurable. [\#2011](https://github.com/bigdatagenomics/adam/pull/2011) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1697] Expand Illumina metadata regex to cover interleaved index sequences. [\#2010](https://github.com/bigdatagenomics/adam/pull/2010) ([heuermh](https://github.com/heuermh))
 - [ADAM-2007] Make IndelRealignmentTarget implement Serializable. [\#2009](https://github.com/bigdatagenomics/adam/pull/2009) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-2006] Support loading 0-length reads as FASTQ. [\#2008](https://github.com/bigdatagenomics/adam/pull/2008) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1697] Expand Illumina metadata regex to cover index sequences [\#2004](https://github.com/bigdatagenomics/adam/pull/2004) ([pauldwolfe](https://github.com/pauldwolfe))
 - [ADAM-1996] Load and save VariantContexts as partitioned Parquet. [\#2001](https://github.com/bigdatagenomics/adam/pull/2001) ([heuermh](https://github.com/heuermh))
 - [ADAM-1997] Nest list of region join types in joins doc. [\#1998](https://github.com/bigdatagenomics/adam/pull/1998) ([heuermh](https://github.com/heuermh))
 - [ADAM-1877] Add filterToReferenceName(s) to SequenceDictionary. [\#1995](https://github.com/bigdatagenomics/adam/pull/1995) ([heuermh](https://github.com/heuermh))
 - [ADAM-1984] Support file systems that don't set the scheme. [\#1985](https://github.com/bigdatagenomics/adam/pull/1985) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1978] Add additional filter by convenience methods. [\#1983](https://github.com/bigdatagenomics/adam/pull/1983) ([heuermh](https://github.com/heuermh))
 - Adding printAttribute methods for alignment records, features, and samples. [\#1982](https://github.com/bigdatagenomics/adam/pull/1982) ([heuermh](https://github.com/heuermh))
 -  Fix partitioning code to use Long instead of Int [\#1980](https://github.com/bigdatagenomics/adam/pull/1980) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1976] Adding core API documentation link and badge. [\#1979](https://github.com/bigdatagenomics/adam/pull/1979) ([heuermh](https://github.com/heuermh))
 - [ADAM-1974] Close unclosed stream in FastqInputFormat. [\#1975](https://github.com/bigdatagenomics/adam/pull/1975) ([fnothaft](https://github.com/fnothaft))
 - Set defaults to schemas [\#1972](https://github.com/bigdatagenomics/adam/pull/1972) ([ffinfo](https://github.com/ffinfo))
 - Add loadPairedFastqAsFragments method. [\#1866](https://github.com/bigdatagenomics/adam/pull/1866) ([heuermh](https://github.com/heuermh))
 - Adding loadPairedFastqAsFragments method [\#1828](https://github.com/bigdatagenomics/adam/pull/1828) ([ffinfo](https://github.com/ffinfo))

 
### Cannoli Version 0.3.0 ###

**Closed issues:**

 - Add implicit methods that attach to source RDD [\#131](https://github.com/bigdatagenomics/cannoli/issues/131)
 - Flip function and command line class names around [\#130](https://github.com/bigdatagenomics/cannoli/issues/130)
 - Add API documentation link and badge [\#128](https://github.com/bigdatagenomics/cannoli/issues/128)
 - Add homebrew formula at brewsci/homebrew-bio [\#124](https://github.com/bigdatagenomics/cannoli/issues/124)
 - Add bioconda recipe [\#123](https://github.com/bigdatagenomics/cannoli/issues/123)
 - Support validation stringency in out formatters [\#122](https://github.com/bigdatagenomics/cannoli/issues/122)
 - Add Ensembl Variant Effect Predictor (VEP) for variant annotation [\#112](https://github.com/bigdatagenomics/cannoli/issues/112)
 - Add Minimap2 for alignment [\#111](https://github.com/bigdatagenomics/cannoli/issues/111)

**Merged and closed pull requests:**

 - Update release script for changelog. [\#143](https://github.com/bigdatagenomics/cannoli/pull/143) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-141] Update ADAM dependency to 0.25.0. [\#142](https://github.com/bigdatagenomics/cannoli/pull/142) ([heuermh](https://github.com/heuermh))
 - Update default docker image for bowtie2. [\#140](https://github.com/bigdatagenomics/cannoli/pull/140) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-138] Update Cannoli per latest ADAM snapshot changes. [\#139](https://github.com/bigdatagenomics/cannoli/pull/139) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-131] Add implicits on Cannoli function source data sets. [\#133](https://github.com/bigdatagenomics/cannoli/pull/133) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-130] Extract function classes to core package. [\#132](https://github.com/bigdatagenomics/cannoli/pull/132) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-128] Adding API documentation link and badge. [\#129](https://github.com/bigdatagenomics/cannoli/pull/129) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-112]  Adding Ensembl Variant Effect Predictor (VEP) for variant annotation [\#127](https://github.com/bigdatagenomics/cannoli/pull/127) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-122] Support validation stringency in out formatters. [\#126](https://github.com/bigdatagenomics/cannoli/pull/126) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-111] Adding Minimap2 for alignment. [\#119](https://github.com/bigdatagenomics/cannoli/pull/119) ([heuermh](https://github.com/heuermh))
