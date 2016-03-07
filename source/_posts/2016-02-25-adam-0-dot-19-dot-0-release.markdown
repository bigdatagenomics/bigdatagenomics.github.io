---
layout: post
title: "ADAM 0.19.0 Released"
date: 2016-02-25 12:00:00 -0700
comments: true
categories: 
- ADAM
- releases
---

ADAM version 0.19.0 has been [released](https://github.com/bigdatagenomics/adam/releases), built for both [Scala 2.10](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.10-0.19.0) and [Scala 2.11](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.11-0.19.0).


The 0.19.0 release contains various concordance fixes and performance improvements for accessing read metadata.  Schema changes, including a bump to version 0.7.0 of the Big Data Genomics [Avro data formats](https://github.com/bigdatagenomics/bdg-formats/releases/tag/bdg-formats-0.7.0), were made to support the read metadata performance improvements.  Additionally, the performance of exporting a single BAM file was improved, and this was made to be guaranteed correct for sorted data.


ADAM now targets Apache Spark 1.5.2 and Apache Hadoop 2.6.0 as the default build environment.  ADAM and applications built on ADAM should run on a wide range of Apache Spark (1.3.1 up to and including the most recent, 1.6.0) and Apache Hadoop (currently 2.3.0 and 2.6.0) versions.  A compatibility matrix of Spark, Hadoop, and Scala version builds in our [continuous integration system](https://amplab.cs.berkeley.edu/jenkins/view/Big%20Data%20Genomics/) verifies this.  Please note, as of this release, support for Apache Spark 1.2.x and Apache Hadoop 1.0.x [has been dropped](https://github.com/bigdatagenomics/adam/issues/958).


The full list of changes since version 0.18.2 is below.


<!-- more -->

**Closed issues:**

- Update bdg-utils dependency version to 0.2.4 [\#960](https://github.com/bigdatagenomics/adam/issues/960)
- Drop support for Spark version 1.2.1, Hadoop version 1.0.x [\#958](https://github.com/bigdatagenomics/adam/issues/958)
- Exception occurs when running tests on master [\#956](https://github.com/bigdatagenomics/adam/issues/956)
- Flagstat results still don't match samtools flagstat [\#946](https://github.com/bigdatagenomics/adam/issues/946)
- readInFragment value is not properly read from parquet file into RDD\[AlignmentRecord\] [\#942](https://github.com/bigdatagenomics/adam/issues/942)
- adam2vcf -sort\_on\_save flag broken [\#940](https://github.com/bigdatagenomics/adam/issues/940)
- Transform -limit\_projection requires .sam.seqdict file [\#937](https://github.com/bigdatagenomics/adam/issues/937)
- MarkDuplicates fails if library name is not set [\#934](https://github.com/bigdatagenomics/adam/issues/934)
- fastqtobam or sam [\#928](https://github.com/bigdatagenomics/adam/issues/928)
- Vcf2Adam uses SB field instead of FS field for fisher exact test for strand bias [\#923](https://github.com/bigdatagenomics/adam/issues/923)
- Add back limit\_projection on Transform [\#920](https://github.com/bigdatagenomics/adam/issues/920)
- BAM header is not getting set on partition 0 with headerless BAM output format [\#916](https://github.com/bigdatagenomics/adam/issues/916)
- Add numParts apply method to GenomicRegionPartitioner [\#914](https://github.com/bigdatagenomics/adam/issues/914)
- Add Spark version 1.6.x to Jenkins build matrix [\#913](https://github.com/bigdatagenomics/adam/issues/913)
- Target Spark 1.5.2 as default Spark version [\#911](https://github.com/bigdatagenomics/adam/issues/911)
- Move to bdg-formats 0.7.0 [\#905](https://github.com/bigdatagenomics/adam/issues/905)
- secondOfPair and firstOfPair flag is missing in the newest 0.18 adam transformed results from BAM [\#903](https://github.com/bigdatagenomics/adam/issues/903)
- Future pull request [\#900](https://github.com/bigdatagenomics/adam/issues/900)
- error in vcf2adam [\#899](https://github.com/bigdatagenomics/adam/issues/899)
- Importing directory of VCFs seems to fail [\#898](https://github.com/bigdatagenomics/adam/issues/898)
- How to filter genotypeRDD on sample names? org.apache.spark.SparkException: Task not serializable? [\#891](https://github.com/bigdatagenomics/adam/issues/891)
- Add Spark version 1.5.x to Jenkins build matrix [\#889](https://github.com/bigdatagenomics/adam/issues/889)
- Transform DAG causes stages to recompute [\#883](https://github.com/bigdatagenomics/adam/issues/883)
- adam-submit buildinfo is confused [\#880](https://github.com/bigdatagenomics/adam/issues/880)
- move\_to\_scala\_2.11 and maven-javadoc-plugin [\#863](https://github.com/bigdatagenomics/adam/issues/863)
- NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable [\#837](https://github.com/bigdatagenomics/adam/issues/837)
- Fix record oriented shuffle [\#599](https://github.com/bigdatagenomics/adam/issues/599)
- Avro.GenericData error with ADAM 0.12.0 on reading from ADAM file [\#290](https://github.com/bigdatagenomics/adam/issues/290)

**Merged and closed pull requests:**

- \[ADAM-960\] Updating bdg-utils dependency version to 0.2.4 [\#961](https://github.com/bigdatagenomics/adam/pull/961) ([heuermh](https://github.com/heuermh))
- \[ADAM-946\] Fixes to FlagStat for Samtools concordance issue [\#954](https://github.com/bigdatagenomics/adam/pull/954) ([jpdna](https://github.com/jpdna))
- Fix for travis build, replace reads2ref with reads2fragments [\#950](https://github.com/bigdatagenomics/adam/pull/950) ([heuermh](https://github.com/heuermh))
- \[ADAM-940\] Fix adam2vcf -sort_on_save flag [\#949](https://github.com/bigdatagenomics/adam/pull/949) ([massie](https://github.com/massie))
- Remove BuildInformation and extraneous git-commit-id-plugin configuration [\#948](https://github.com/bigdatagenomics/adam/pull/948) ([heuermh](https://github.com/heuermh))
- Update readme for spark 1.5.2 and hadoop 2.6.0 [\#944](https://github.com/bigdatagenomics/adam/pull/944) ([heuermh](https://github.com/heuermh))
- \[ADAM-942\] Replace first/secondInRead with readInFragment [\#943](https://github.com/bigdatagenomics/adam/pull/943) ([heuermh](https://github.com/heuermh))
- \[ADAM-937\] Adding check for aligned read predicate or limit projection flags and non-parquet input path [\#938](https://github.com/bigdatagenomics/adam/pull/938) ([heuermh](https://github.com/heuermh))
- \[ADAM-934\] Properly handle unset library name during duplicate marking [\#935](https://github.com/bigdatagenomics/adam/pull/935) ([fnothaft](https://github.com/fnothaft))
- \[ADAM-911\] Move to Spark 1.5.2 and Hadoop 2.6.0 as default versions. [\#932](https://github.com/bigdatagenomics/adam/pull/932) ([fnothaft](https://github.com/fnothaft))
- added start and end values to Interval Trait. Used for IntervalRDD [\#931](https://github.com/bigdatagenomics/adam/pull/931) ([akmorrow13](https://github.com/akmorrow13))
- Removing buildinfo command [\#929](https://github.com/bigdatagenomics/adam/pull/929) ([heuermh](https://github.com/heuermh))
- Removing symbolic test resource links, read from test classpath instead [\#927](https://github.com/bigdatagenomics/adam/pull/927) ([heuermh](https://github.com/heuermh))
- Changed fisher strand bias field for VCF2Adam from SB to FS [\#924](https://github.com/bigdatagenomics/adam/pull/924) ([andrewmchen](https://github.com/andrewmchen))
- \[ADAM-920\] Limit tag/orig qual flags in Transform. [\#921](https://github.com/bigdatagenomics/adam/pull/921) ([fnothaft](https://github.com/fnothaft))
- Change the README to use adam-shell -i instead of pasting [\#919](https://github.com/bigdatagenomics/adam/pull/919) ([andrewmchen](https://github.com/andrewmchen))
- \[ADAM-916\] New strategy for writing header. [\#917](https://github.com/bigdatagenomics/adam/pull/917) ([fnothaft](https://github.com/fnothaft))
- \[ADAM-914\] Create a GenomicRegionPartitioner given a partition count. [\#915](https://github.com/bigdatagenomics/adam/pull/915) ([fnothaft](https://github.com/fnothaft))
- Squashed \#907 and ran format-sources [\#908](https://github.com/bigdatagenomics/adam/pull/908) ([fnothaft](https://github.com/fnothaft))
- Various small fixes [\#907](https://github.com/bigdatagenomics/adam/pull/908) ([huitseeker](https://github.com/huitseeker))
- ADAM-599, 905: Move to bdg-formats:0.7.0 and migrate metadata [\#906](https://github.com/bigdatagenomics/adam/pull/906) ([fnothaft](https://github.com/fnothaft))
- Rewrote the getType method to handle all ploidy levels [\#904](https://github.com/bigdatagenomics/adam/pull/904) ([NeillGibson](https://github.com/NeillGibson))
- Single file save from \#733, rebased [\#901](https://github.com/bigdatagenomics/adam/pull/901) ([fnothaft](https://github.com/fnothaft))
- Added is\* genotype methods from HTS-JDK Genotype to RichGenotype [\#895](https://github.com/bigdatagenomics/adam/pull/895) ([NeillGibson](https://github.com/NeillGibson))
- \[ADAM-891\] Mark SparkContext as @transient. [\#894](https://github.com/bigdatagenomics/adam/pull/894) ([fnothaft](https://github.com/fnothaft))
- Update README URLs based on HTTP redirects [\#892](https://github.com/bigdatagenomics/adam/pull/892) ([ReadmeCritic](https://github.com/ReadmeCritic))
- adding --version command line option [\#888](https://github.com/bigdatagenomics/adam/pull/888) ([heuermh](https://github.com/heuermh))
- Add exception in move_to_scala_2.11.sh for maven-javadoc-plugin [\#887](https://github.com/bigdatagenomics/adam/pull/887) ([heuermh](https://github.com/heuermh))
- Fix tightlist bug in Pandoc [\#885](https://github.com/bigdatagenomics/adam/pull/885) ([massie](https://github.com/massie))
- \[ADAM-883\] Add caching to Transform pipeline. [\#884](https://github.com/bigdatagenomics/adam/pull/884) ([fnothaft](https://github.com/fnothaft))
