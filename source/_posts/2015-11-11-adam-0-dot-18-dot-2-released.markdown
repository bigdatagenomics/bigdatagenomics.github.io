---
layout: post
title: "ADAM 0.18.2 Released"
date: 2015-11-11 12:00:00 -0700
comments: true
categories: 
- ADAM
- releases
---


A few ADAM releases have been made since the last announcement; we'll attempt to catch up here.


The most recent is a version [0.18.2 bugfix release](https://github.com/bigdatagenomics/adam/releases), built for both [Scala 2.10](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.10-0.18.2) and [Scala 2.11](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.11-0.18.2).  It fixes [a minor issue](https://github.com/bigdatagenomics/adam/pull/873) with the binary distribution artifact from version 0.18.1.


Prior to version 0.18.2, we made significant changes to support version 0.6.0 of the Big Data Genomics [Avro data formats](https://github.com/bigdatagenomics/bdg-formats/releases/tag/bdg-formats-0.6.0).  We also improved performance on core transforms (markdups, indel realignment, bqsr) by using finer grained projection.  Some issues in 2bitfile when dealing with gaps and masked regions were fixed.  Round-trip transformations from native formats (e.g., FASTA, FASTQ, SAM, BAM) to ADAM and back have been improved.  We made extending ADAM more straightforward.


ADAM now runs on a wide range of Apache Spark (1.2.1 up to and including the most recent, 1.5.1) and Apache Hadoop (currently 1.0.4, 2.3.0 and 2.6.0) versions.  This is verified by a compatibility matrix of Spark, Hadoop, and Scala version builds in our [continuous integration system](https://amplab.cs.berkeley.edu/jenkins/view/Big%20Data%20Genomics/).


The full list of changes since version 0.17.0 is below.


<!-- more -->

### Version 0.18.2 ###
* ISSUE [877](https://github.com/bigdatagenomics/adam/pull/877): Minor fix to commit script to support https.
* ISSUE [876](https://github.com/bigdatagenomics/adam/pull/876): Separate command line argument words by underscores
* ISSUE [875](https://github.com/bigdatagenomics/adam/pull/875): P Operator parsing for MDTag
* ISSUE [873](https://github.com/bigdatagenomics/adam/pull/873): [ADAM-872] Modify regex to capture release and SNAPSHOT jars but not javadoc or sources jars
* ISSUE [866](https://github.com/bigdatagenomics/adam/pull/866): [ADAM-864] Don't force shuffle if reducing partition count.
* ISSUE [856](https://github.com/bigdatagenomics/adam/pull/856): export valid fastq
* ISSUE [847](https://github.com/bigdatagenomics/adam/pull/847): Updating build dependency versions to latest minor versions

### Version 0.18.1 ###
* ISSUE [870](https://github.com/bigdatagenomics/adam/pull/870): [ADAM-867] add pull requests missing from 0.18.0 release to CHANGES.md
* ISSUE [869](https://github.com/bigdatagenomics/adam/pull/869): [ADAM-868] make release branch and tag names consistent
* ISSUE [862](https://github.com/bigdatagenomics/adam/pull/862): [ADAM-861] use -d to check for repo assembly dir

### Version 0.18.0 ###
* ISSUE [860](https://github.com/bigdatagenomics/adam/pull/860): New release and pr-commit scripts
* ISSUE [859](https://github.com/bigdatagenomics/adam/pull/859): [ADAM-857] Corrected handling of env vars in bin scripts
* ISSUE [854](https://github.com/bigdatagenomics/adam/pull/854): [ADAM-853] allow main class in adam-submit to be specified
* ISSUE [852](https://github.com/bigdatagenomics/adam/pull/852): [ADAM-851] Slienced Parquet logging.
* ISSUE [850](https://github.com/bigdatagenomics/adam/pull/850): [ADAM-848] TwoBitFile now support nBlocks and maskBlocks
* ISSUE [846](https://github.com/bigdatagenomics/adam/pull/846): Updating maven build plugin dependency versions
* ISSUE [845](https://github.com/bigdatagenomics/adam/pull/845): [ADAM-780] Make DecadentRead package private.
* ISSUE [844](https://github.com/bigdatagenomics/adam/pull/844): [ADAM-843] Aggressively project out metadata fields.
* ISSUE [840](https://github.com/bigdatagenomics/adam/pull/840): fix flagstat output file encoding
* ISSUE [839](https://github.com/bigdatagenomics/adam/pull/839): let flagstat write to file
* ISSUE [831](https://github.com/bigdatagenomics/adam/pull/831): Support loading paired fastqs
* ISSUE [830](https://github.com/bigdatagenomics/adam/pull/830): better validation when saving paired fastqs
* ISSUE [829](https://github.com/bigdatagenomics/adam/pull/829): fix `Long != null` warnings
* ISSUE [819](https://github.com/bigdatagenomics/adam/pull/819): Implement custom ReferenceRegion hashcode
* ISSUE [816](https://github.com/bigdatagenomics/adam/pull/816): [ADAM-793] adding command to convert ADAM nucleotide contig fragments to FASTA files
* ISSUE [815](https://github.com/bigdatagenomics/adam/pull/815): Upgrade to bdg-formats:0.6.0, add Fragment datatype converters
* ISSUE [814](https://github.com/bigdatagenomics/adam/pull/814): [ADAM-812] fix for javadoc errors on JDK8
* ISSUE [813](https://github.com/bigdatagenomics/adam/pull/813): [ADAM-808] build an assembly cli jar with maven shade plugin
* ISSUE [810](https://github.com/bigdatagenomics/adam/pull/810): [ADAM-807] workaround for ktoso/maven-git-commit-id-plugin#61
* ISSUE [809](https://github.com/bigdatagenomics/adam/pull/809): [ADAM-785] Add support for all numeric array (TYPE=B) tags
* ISSUE [806](https://github.com/bigdatagenomics/adam/pull/806): [ADAM-755] updating utils dependency version to 0.2.3
* ISSUE [805](https://github.com/bigdatagenomics/adam/pull/805): Better transform error when file doesn't exist
* ISSUE [803](https://github.com/bigdatagenomics/adam/pull/803): fix unmapped-read sorting
* ISSUE [802](https://github.com/bigdatagenomics/adam/pull/802): stop writing contig names as md5 sums
* ISSUE [798](https://github.com/bigdatagenomics/adam/pull/798): fix SAM-attr conversion bug; int[]'s not byte[]'s
* ISSUE [790](https://github.com/bigdatagenomics/adam/pull/790): optionally add MDTags to reads with `transform`
* ISSUE [782](https://github.com/bigdatagenomics/adam/pull/782): Fix SAM Attribute parser for numeric array tags
* ISSUE [773](https://github.com/bigdatagenomics/adam/pull/773): [ADAM-772] fix some bash var quoting
* ISSUE [765](https://github.com/bigdatagenomics/adam/pull/765): [ADAM-752] Build for many combos of Spark/Hadoop versions.
* ISSUE [764](https://github.com/bigdatagenomics/adam/pull/764): More involved README restructuring
* ISSUE [762](https://github.com/bigdatagenomics/adam/pull/762): [ADAM-132] allowing list of commands to be injected into adam-cli ADAMMain

### Version 0.17.1 ###
* ISSUE [784](https://github.com/bigdatagenomics/adam/pull/784): [ADAM-783] Write @SQ header lines in sorted order.
* ISSUE [792](https://github.com/bigdatagenomics/adam/pull/792): [ADAM-791] Add repartition parameter to Fasta2ADAM.
* ISSUE [781](https://github.com/bigdatagenomics/adam/pull/781): [ADAM-777] Add validation stringency flag for BQSR.
* ISSUE [757](https://github.com/bigdatagenomics/adam/pull/757): We should print a warning message if the user has ADAM_OPTS set.
* ISSUE [770](https://github.com/bigdatagenomics/adam/pull/770): [ADAM-769] Fix serialization issue in known indel consensus model.
* ISSUE [763](https://github.com/bigdatagenomics/adam/pull/763): Clean up README links, other nits
* ISSUE [749](https://github.com/bigdatagenomics/adam/pull/749): Remove adam-cli jar from classpath during adam-submit
* ISSUE [754](https://github.com/bigdatagenomics/adam/pull/754): Bump ADAM to Spark 1.4
* ISSUE [753](https://github.com/bigdatagenomics/adam/pull/753): Bump Spark to 1.4
* ISSUE [748](https://github.com/bigdatagenomics/adam/pull/748): Fix for mdtag issues with insertions
* ISSUE [746](https://github.com/bigdatagenomics/adam/pull/746): Upgrade to Parquet 1.8.1.
* ISSUE [744](https://github.com/bigdatagenomics/adam/pull/744): [ADAM-743] exclude conflicting jackson dependencies
* ISSUE [737](https://github.com/bigdatagenomics/adam/pull/737): Reverse complement negative strand reads in fastq output
* ISSUE [731](https://github.com/bigdatagenomics/adam/pull/731): Fixed bug preventing use of TLEN attribute
* ISSUE [730](https://github.com/bigdatagenomics/adam/pull/730): [ADAM-729] Stuff TLEN into attributes.
* ISSUE [728](https://github.com/bigdatagenomics/adam/pull/728): [ADAM-709] Remove FeatureHierarchy and FeatureHierarchySuite
* ISSUE [719](https://github.com/bigdatagenomics/adam/pull/719): [ADAM-718] Use filesystem path to get underlying file system.
* ISSUE [712](https://github.com/bigdatagenomics/adam/pull/712): unify header-setting between BAM/SAM and VCF
* ISSUE [696](https://github.com/bigdatagenomics/adam/pull/696): include SequenceRecords from second-in-pair reads
* ISSUE [698](https://github.com/bigdatagenomics/adam/pull/698): class-ify ShuffleRegionJoin, force setting seqdict
* ISSUE [706](https://github.com/bigdatagenomics/adam/pull/706): restore clause guarding pruneCache check
* ISSUE [705](https://github.com/bigdatagenomics/adam/pull/705): GeneFeatureRDDFunctions → FeatureRDDFunctions
