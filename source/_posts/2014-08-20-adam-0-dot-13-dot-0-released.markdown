---
layout: post
title: "ADAM 0.13.0 Released"
date: 2014-08-20 14:04:41 -0700
comments: true
categories: 
- ADAM
- Releases
---

ADAM [0.13.0](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-0.13.0) is now available!

This release includes [genome visualization](https://github.com/bigdatagenomics/adam/pull/346) to view aligned reads and coverage 
information over a reference region. You simply run e.g. `adam viz myreads.adam chr1` from the ADAM source directory and open
your favorite web browser to http://localhost:8080/ to view your data. 

This release also includes a number of features and bug fixes including upgrading to Spark 1.0.1.

<!-- more -->

* ISSUE [343](https://github.com/bigdatagenomics/adam/pull/343): Allow retrying on failure for HTTPRangedByteAccess
* ISSUE [349](https://github.com/bigdatagenomics/adam/pull/349): Fix for a NullPointerException when hostname is null in Task Metrics
* ISSUE [347](https://github.com/bigdatagenomics/adam/pull/347): Bug fix for genome browser
* ISSUE [346](https://github.com/bigdatagenomics/adam/pull/346): Genome visualization
* ISSUE [342](https://github.com/bigdatagenomics/adam/pull/342): [ADAM-309] Update to bdg-formats 0.2.0
* ISSUE [333](https://github.com/bigdatagenomics/adam/pull/333): [ADAM-332] Upgrades ADAM to Spark 1.0.1.
* ISSUE [341](https://github.com/bigdatagenomics/adam/pull/341): [ADAM-340] Adding the TrackedLayout trait and implementation.
* ISSUE [337](https://github.com/bigdatagenomics/adam/pull/337): [ADAM-335] Updated README.md to reflect migration to appassembler.
* ISSUE [311](https://github.com/bigdatagenomics/adam/pull/311): Adding several simple normalizations.
* ISSUE [330](https://github.com/bigdatagenomics/adam/pull/330): Make mismatch and deletes positions accessible
* ISSUE [334](https://github.com/bigdatagenomics/adam/pull/334): Moving code coverage into a profile
* ISSUE [329](https://github.com/bigdatagenomics/adam/pull/329): Add count of mismatches to mdtag
* ISSUE [328](https://github.com/bigdatagenomics/adam/pull/328): [ADAM-326] Adding a 5-second retry on the HttpRangedByteAccess test.
* ISSUE [325](https://github.com/bigdatagenomics/adam/pull/325): Adding documentation for commit/issue nomenclature and rebasing

This summer, we also quietly pushed out a [0.12.1](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-0.12.1) release
that included a number of features (e.g. Parquet and indexed Parquet Spark RDDs, k-mer/q-mer counting, fixed depth prefix tries) and bug fixes:

* ISSUE [308](https://github.com/bigdatagenomics/adam/pull/308): Fixing the 'index 0' bug in features2adam
* ISSUE [306](https://github.com/bigdatagenomics/adam/pull/306): Adding code for lifting over between sequences and the reference genome.
* ISSUE [320](https://github.com/bigdatagenomics/adam/pull/320): Remove extraneous implicit methods in ReferenceMappingContext
* ISSUE [314](https://github.com/bigdatagenomics/adam/pull/314): Updates to indel realigner to improve performance and accuracy.
* ISSUE [319](https://github.com/bigdatagenomics/adam/pull/319): Adding scripts for publishing scaladoc.
* ISSUE [315](https://github.com/bigdatagenomics/adam/pull/315): Added table of (wall-clock) stage durations when print_metrics is used
* ISSUE [312](https://github.com/bigdatagenomics/adam/pull/312): Fixing sources jar
* ISSUE [313](https://github.com/bigdatagenomics/adam/pull/313): Making the CredentialsProperties file optional
* ISSUE [267](https://github.com/bigdatagenomics/adam/pull/267): Parquet and indexed Parquet RDD implementations, and indices.
* ISSUE [301](https://github.com/bigdatagenomics/adam/pull/301): Add Beacon's AlleleCount
* ISSUE [293](https://github.com/bigdatagenomics/adam/pull/293): Add aggregation and display of metrics obtained from Spark
* ISSUE [295](https://github.com/bigdatagenomics/adam/pull/295): Fix broken link to ADAM specification for storing reads.
* ISSUE [292](https://github.com/bigdatagenomics/adam/pull/292): Cleaning up scaladoc generation warnings.
* ISSUE [289](https://github.com/bigdatagenomics/adam/pull/289): Modifying interleaved fastq format to be hadoop version independent.
* ISSUE [288](https://github.com/bigdatagenomics/adam/pull/288): Add ADAMFeature to Kryo registrator
* ISSUE [286](https://github.com/bigdatagenomics/adam/pull/286): Removing some debug printout that was left in.
* ISSUE [287](https://github.com/bigdatagenomics/adam/pull/287): Cleaning hadoop dependencies
* ISSUE [285](https://github.com/bigdatagenomics/adam/pull/285): Refactoring read groups to increase the amount of data stored.
* ISSUE [284](https://github.com/bigdatagenomics/adam/pull/284): Cleaning up build warnings.
* ISSUE [280](https://github.com/bigdatagenomics/adam/pull/280): Move to bdg-formats
* ISSUE [283](https://github.com/bigdatagenomics/adam/pull/283): Fix reference name comment
* ISSUE [282](https://github.com/bigdatagenomics/adam/pull/282): Minor cleanup on interleaved FASTQ input format.
* ISSUE [277](https://github.com/bigdatagenomics/adam/pull/277): Implemented HTTPRangedByteAccess.
* ISSUE [274](https://github.com/bigdatagenomics/adam/pull/274): Added clarifying note to `ADAMVariantContext`
* ISSUE [279](https://github.com/bigdatagenomics/adam/pull/279): Simplify format-source
* ISSUE [278](https://github.com/bigdatagenomics/adam/pull/278): Use maven license plugin to ensure source has correct license
* ISSUE [268](https://github.com/bigdatagenomics/adam/pull/268): Adding fixed depth prefix trie implementation
* ISSUE [273](https://github.com/bigdatagenomics/adam/pull/273): Fixes issue in reference models where strings are not sanitized on collection from avro.
* ISSUE [272](https://github.com/bigdatagenomics/adam/pull/272): Created command categories
* ISSUE [269](https://github.com/bigdatagenomics/adam/pull/269): Adding k-mer and q-mer counting.
* ISSUE [271](https://github.com/bigdatagenomics/adam/pull/271): Consolidate Parquet logging configuration

