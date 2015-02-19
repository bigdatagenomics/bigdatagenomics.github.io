---
layout: post
title: "ADAM 0.16.0 Released"
date: 2015-02-18 16:32:31 -0800
comments: true
categories: 
- ADAM
- releases
---

[ADAM 0.16.0](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-0.16.0) is now available.

This release improves the performance of Base Quality Score Recalibration (BQSR) by 3.5x, adds support for multiline FASTQ input, visualization of variants when given VCF input, includes a new RegionJoin implementation that is shuffle-based, and adds new methods for region coverage calculations.

Drop into our Gitter channel to talk with us about this release

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/bigdatagenomics/adam?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

<!-- more -->

Complete list of changes for ADAM `0.16.0`:

* ISSUE [570](https://github.com/bigdatagenomics/adam/pull/570): A few small conversion fixes
* ISSUE [579](https://github.com/bigdatagenomics/adam/pull/579): [ADAM-578] Update end of read when trimming.
* ISSUE [564](https://github.com/bigdatagenomics/adam/pull/564): [ADAM-563] Add warning message when saving Parquet files with incorrect extension
* ISSUE [576](https://github.com/bigdatagenomics/adam/pull/576): Changed hashCode implementations to improve performance of BQSR
* ISSUE [569](https://github.com/bigdatagenomics/adam/pull/569): Typo in the narrowPeak parser
* ISSUE [568](https://github.com/bigdatagenomics/adam/pull/568): Moved the Timers object from bdg-utils back to ADAM
* ISSUE [478](https://github.com/bigdatagenomics/adam/pull/478): Move non-genomics code
* ISSUE [550](https://github.com/bigdatagenomics/adam/pull/550): [ADAM-549] Added documentation for testing and CI for ADAM.
* ISSUE [555](https://github.com/bigdatagenomics/adam/pull/555): Makes maybeLoadVCF private.
* ISSUE [558](https://github.com/bigdatagenomics/adam/pull/558): Makes Features2ADAMSuite use SparkFunSuite
* ISSUE [557](https://github.com/bigdatagenomics/adam/pull/557): Randomize ports and turn off Spark UI to reduce bind exceptions in tests
* ISSUE [552](https://github.com/bigdatagenomics/adam/pull/552): Create test suite for FlagStat
* ISSUE [554](https://github.com/bigdatagenomics/adam/pull/554): privatize ADAMContext.maybeLoad{Bam,Fastq}
* ISSUE [551](https://github.com/bigdatagenomics/adam/pull/551): [ADAM-386] Multiline FASTQ input
* ISSUE [542](https://github.com/bigdatagenomics/adam/pull/542): Variants Visualization
* ISSUE [545](https://github.com/bigdatagenomics/adam/pull/545): [ADAM-543][ADAM-544] Fix issues with ADAM scripts and classpath
* ISSUE [535](https://github.com/bigdatagenomics/adam/pull/535): [ADAM-441] put a check in for Nothing. Throws an IAE if no return type is provided
* ISSUE [546](https://github.com/bigdatagenomics/adam/pull/546): [ADAM-532] Fix wigFix intermittent test failure
* ISSUE [534](https://github.com/bigdatagenomics/adam/pull/534): [ADAM-528][ADAM-533] Adds new RegionJoin impl that is shuffle-based
* ISSUE [531](https://github.com/bigdatagenomics/adam/pull/531): [ADAM-529] Attaching scaladoc to released distribution.
* ISSUE [413](https://github.com/bigdatagenomics/adam/pull/413): [ADAM-409][ADAM-520] Added local wigfix2bed tool
* ISSUE [527](https://github.com/bigdatagenomics/adam/pull/527): [ADAM-526] `VcfAnnotation2ADAM` only counts once
* ISSUE [523](https://github.com/bigdatagenomics/adam/pull/523): don't open non-.adam-extension files as ADAM files
* ISSUE [521](https://github.com/bigdatagenomics/adam/pull/521): quieting wget output
* ISSUE [482](https://github.com/bigdatagenomics/adam/pull/482): [ADAM-462] Coverage region calculation
* ISSUE [515](https://github.com/bigdatagenomics/adam/pull/515): [ADAM-510] fix for bash syntax error; add ADDL_JARS check to adam-submit
