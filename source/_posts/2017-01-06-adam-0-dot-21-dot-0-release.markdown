---
layout: post
title: "ADAM 0.21.0 Released"
date: 2017-01-06 12:00:00 -0700
comments: true
categories: 
- ADAM
- releases
---

ADAM version 0.21.0 has been [released](https://github.com/bigdatagenomics/adam/releases)!

Due to major changes between Spark versions 1.6 and 2.0, we now build for combinations of Apache Spark and Scala versions:
[Spark 1.x and Scala 2.10](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.10-0.21.0),
[Spark 1.x and Scala 2.11](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.11-0.21.0),
[Spark 2.x and Scala 2.10](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-spark2_2.10-0.21.0), and
[Spark 2.x and Scala 2.11](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-spark2_2.11-0.21.0).
The Spark 2.x build-time dependency will be bumped to version 2.1.0 in the next release of ADAM, see issue [\#1330](https://github.com/bigdatagenomics/adam/issues/1330).

One focus of this release was documentation, both at the developer API level, including extensive javadoc and scaladoc
source code comments, and at the user level (e.g. https://github.com/bigdatagenomics/adam/tree/master/docs/source). The
user docs can be compiled to PDF or HTML with pandoc, but to be honest they look better rendered as Markdown on Github.

Another focus was to more closely follow the VCF specification(s) when reading from and writing to VCF.
For this we made significant changes to our variant and variant annotation schema and added support
for version 1.0 of the [VCF INFO 'ANN' key specification](http://snpeff.sourceforge.net/VCFannotationformat_v1.0.pdf).
This work will continue for our genotype and genotype annotation schema in the next version of ADAM.

The full list of changes since version 0.20.0 is below.

<!-- more -->

**Closed issues:**

- Update Markdown docs with ValidationStringency in VCF<->ADAM CLI [\#1342](https://github.com/bigdatagenomics/adam/issues/1342)
- Variant VCFHeaderLine metadata does not handle wildcards properly [\#1339](https://github.com/bigdatagenomics/adam/issues/1339)
- Close called multiple times on VCF header stream [\#1337](https://github.com/bigdatagenomics/adam/issues/1337)
- BroadcastRegionJoin has serialization failures [\#1334](https://github.com/bigdatagenomics/adam/issues/1334)
- adam-cli uses git-commit-id-plugin which breaks release? [\#1322](https://github.com/bigdatagenomics/adam/issues/1322)
- move_to_xyz scripts should have interlocks... [\#1317](https://github.com/bigdatagenomics/adam/issues/1317)
- Lineage for partitionAndJoin in ShuffleRegionJoin causes StackOverflow Errors [\#1308](https://github.com/bigdatagenomics/adam/issues/1308)
- Add move_to_spark_1.sh script and update README to mention [\#1307](https://github.com/bigdatagenomics/adam/issues/1307)
- adam-submit transform fails with Exception in thread "main" java.lang.IncompatibleClassChangeError: Implementing class [\#1306](https://github.com/bigdatagenomics/adam/issues/1306)
- private ADAMContext constructor? [\#1296](https://github.com/bigdatagenomics/adam/issues/1296)
- AlignmentRecord.mateAlignmentEnd never set [\#1290](https://github.com/bigdatagenomics/adam/issues/1290)
- how to submit my own driver class via adam-submit? [\#1289](https://github.com/bigdatagenomics/adam/issues/1289)
- ReferenceRegion on Genotype seems busted? [\#1286](https://github.com/bigdatagenomics/adam/issues/1286)
- Clarify strandedness in ReferenceRegion apply methods [\#1285](https://github.com/bigdatagenomics/adam/issues/1285)
- Parquet and CRAM debug logging during unit tests [\#1280](https://github.com/bigdatagenomics/adam/issues/1280)
- Add more ANN field parsing unit tests [\#1273](https://github.com/bigdatagenomics/adam/issues/1273)
- loadVariantAnnotations returns empty RDD [\#1271](https://github.com/bigdatagenomics/adam/issues/1271)
- Implement joinVariantAnnotations with region join [\#1259](https://github.com/bigdatagenomics/adam/issues/1259)
- Count how many chromosome in the range of the kmer [\#1249](https://github.com/bigdatagenomics/adam/issues/1249)
- ADAM minor release to support htsjdk 2.7.0? [\#1248](https://github.com/bigdatagenomics/adam/issues/1248)
- how to config kryo.registrator programmatically [\#1245](https://github.com/bigdatagenomics/adam/issues/1245)
- Does the nested record Flattener drop Maps/Arrays? [\#1244](https://github.com/bigdatagenomics/adam/issues/1244)
- Dead-ish code cleanup in `org.bdgenomics.adam.utils` [\#1242](https://github.com/bigdatagenomics/adam/issues/1242)
- java.io.FileNotFoundException for old adam file after upgrade to adam0.20 [\#1240](https://github.com/bigdatagenomics/adam/issues/1240)
- please add maven-source-plugin into the pom file [\#1239](https://github.com/bigdatagenomics/adam/issues/1239)
- Assembly jar doesn't get rebuilt on CLI changes [\#1238](https://github.com/bigdatagenomics/adam/issues/1238)
- how to compare with the last the column for the same chromosome name? [\#1237](https://github.com/bigdatagenomics/adam/issues/1237)
- Need a way for users to add VCF header lines [\#1233](https://github.com/bigdatagenomics/adam/issues/1233)
- Enhancements to VCF save [\#1232](https://github.com/bigdatagenomics/adam/issues/1232)
- Must we split multi-allelic sites in our Genotype model? [\#1231](https://github.com/bigdatagenomics/adam/issues/1231)
- Can't override default -collapse in reads2coverage [\#1228](https://github.com/bigdatagenomics/adam/issues/1228)
- Reads2coverage NPEs on unmapped reads [\#1227](https://github.com/bigdatagenomics/adam/issues/1227)
- Strand bias doesn't get exported [\#1226](https://github.com/bigdatagenomics/adam/issues/1226)
- Move ADAMFunSuite helper functions upstream to SparkFunSuite [\#1225](https://github.com/bigdatagenomics/adam/issues/1225)
- broadcast join using interval tree [\#1224](https://github.com/bigdatagenomics/adam/issues/1224)
- Instrumentation is lost in ShuffleRegionJoin [\#1222](https://github.com/bigdatagenomics/adam/issues/1222)
- Bump Spark, Scala, Hadoop dependency versions [\#1221](https://github.com/bigdatagenomics/adam/issues/1221)
- GenomicRDD shuffle region join passes partition count to partition size [\#1220](https://github.com/bigdatagenomics/adam/issues/1220)
- Scala compile errors downstream of Spark 2 Scala 2.11 artifacts [\#1218](https://github.com/bigdatagenomics/adam/issues/1218)
- Javac error: incompatible types: SparkContext cannot be converted to ADAMContext [\#1217](https://github.com/bigdatagenomics/adam/issues/1217)
- Release 0.20.0 artifacts failed Sonatype Nexus validation [\#1212](https://github.com/bigdatagenomics/adam/issues/1212)
- Release script failed for 0.20.0 release [\#1211](https://github.com/bigdatagenomics/adam/issues/1211)
- gVCF - can't load multi-allelic sites [\#1202](https://github.com/bigdatagenomics/adam/issues/1202)
- Allow open-ended intervals in loadIndexedBam [\#1196](https://github.com/bigdatagenomics/adam/issues/1196)
- Interval tree join in ADAM [\#1171](https://github.com/bigdatagenomics/adam/issues/1171)
- spark-submit throw exception in spark-standalone using .adam which transformed from .vcf [\#1121](https://github.com/bigdatagenomics/adam/issues/1121)
- BroadcastRegionJoin is not a broadcast join [\#1110](https://github.com/bigdatagenomics/adam/issues/1110)
- Improve test coverage of VariantContextConverter [\#1107](https://github.com/bigdatagenomics/adam/issues/1107)
- Variant dbsnp rs id tracking in vcf2adam and ADAM2Vcf [\#1103](https://github.com/bigdatagenomics/adam/issues/1103)
- Document core ADAM transform methods [\#1085](https://github.com/bigdatagenomics/adam/issues/1085)
- Document deploying ADAM on Toil [\#1084](https://github.com/bigdatagenomics/adam/issues/1084)
- Clean up packages [\#1083](https://github.com/bigdatagenomics/adam/issues/1083)
- VariantCallingAnnotations is getting populated with INFO fields [\#1063](https://github.com/bigdatagenomics/adam/issues/1063)
- How to load DatabaseVariantAnnotation information ? [\#1049](https://github.com/bigdatagenomics/adam/issues/1049)
- Release ADAM version 0.20.0 [\#1048](https://github.com/bigdatagenomics/adam/issues/1048)
- Support VCF annotation ANN field in vcf2adam and adam2vcf [\#1044](https://github.com/bigdatagenomics/adam/issues/1044)
- How to create a rich(er) VariantContext RDD? Reconstruct VCF INFO fields. [\#878](https://github.com/bigdatagenomics/adam/issues/878)
- Add biologist targeted section to the README [\#497](https://github.com/bigdatagenomics/adam/issues/497)
- Update usage docs running for EC2 and CDH [\#493](https://github.com/bigdatagenomics/adam/issues/493)
- Add docs about building downstream apps on top of ADAM [\#291](https://github.com/bigdatagenomics/adam/issues/291)
- Variant filter representation [\#194](https://github.com/bigdatagenomics/adam/issues/194)

**Merged and closed pull requests:**

- [ADAM-1342] Update CLI docs after #1288 merged. [\#1343](https://github.com/bigdatagenomics/adam/pull/1343) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1339] Use glob-safe method to load VCF header metadata for Parquet [\#1340](https://github.com/bigdatagenomics/adam/pull/1340) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1337] Remove os.{flush,close} calls after writing VCF header. [\#1338](https://github.com/bigdatagenomics/adam/pull/1338) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1334] Clean up serialization issues in Broadcast region join. [\#1336](https://github.com/bigdatagenomics/adam/pull/1336) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1307] move_to_spark_2 fails after moving to scala 2.11. [\#1329](https://github.com/bigdatagenomics/adam/pull/1329) ([fnothaft](https://github.com/fnothaft))
- unroll/optimize some JavaConversions [\#1326](https://github.com/bigdatagenomics/adam/pull/1326) ([ryan-williams](https://github.com/ryan-williams))
- clean up *Join type-params/scaldocs [\#1325](https://github.com/bigdatagenomics/adam/pull/1325) ([ryan-williams](https://github.com/ryan-williams))
- [ADAM-1322] Skip git commit plugin if .git is missing. [\#1323](https://github.com/bigdatagenomics/adam/pull/1323) ([fnothaft](https://github.com/fnothaft))
- Supports access to indexed fa and fasta files [\#1320](https://github.com/bigdatagenomics/adam/pull/1320) ([akmorrow13](https://github.com/akmorrow13))
- Add interlocks for move_to_xyz scripts. [\#1319](https://github.com/bigdatagenomics/adam/pull/1319) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1307] Add script for moving to Spark 1. [\#1318](https://github.com/bigdatagenomics/adam/pull/1318) ([fnothaft](https://github.com/fnothaft))
- Update move_to_spark_2.sh [\#1316](https://github.com/bigdatagenomics/adam/pull/1316) ([creggian](https://github.com/creggian))
- [ADAM-1308] Fix stack overflow in join with custom iterator impl. [\#1315](https://github.com/bigdatagenomics/adam/pull/1315) ([fnothaft](https://github.com/fnothaft))
- Why Adam? section added to README.md [\#1310](https://github.com/bigdatagenomics/adam/pull/1310) ([tverbeiren](https://github.com/tverbeiren)) 
- Add docs about using ADAM's Kryo registrator from another Kryo registrator. [\#1305](https://github.com/bigdatagenomics/adam/pull/1305) ([fnothaft](https://github.com/fnothaft))
- Add docs about building downstream applications [\#1304](https://github.com/bigdatagenomics/adam/pull/1304) ([heuermh](https://github.com/heuermh))
- [ADAM-493] Add ADAM-on-Spark-on-YARN docs. [\#1301](https://github.com/bigdatagenomics/adam/pull/1301) ([fnothaft](https://github.com/fnothaft))
- Code style fixes [\#1299](https://github.com/bigdatagenomics/adam/pull/1299) ([heuermh](https://github.com/heuermh))
- Make ADAMContext and JavaADAMContext constructors public [\#1298](https://github.com/bigdatagenomics/adam/pull/1298) ([heuermh](https://github.com/heuermh))
- Remove back reference between VariantAnnotation and Variant [\#1297](https://github.com/bigdatagenomics/adam/pull/1297) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1280] Silence CRAM logging in tests. [\#1294](https://github.com/bigdatagenomics/adam/pull/1294) ([fnothaft](https://github.com/fnothaft))
- HBase as a separate repo [\#1293](https://github.com/bigdatagenomics/adam/pull/1293) ([jpdna](https://github.com/jpdna))
- Reference region cleanup [\#1291](https://github.com/bigdatagenomics/adam/pull/1291) ([fnothaft](https://github.com/fnothaft))
- Clean rewrite of VariantContextConverter [\#1288](https://github.com/bigdatagenomics/adam/pull/1288) ([fnothaft](https://github.com/fnothaft))
- add function:filterByOverlappingRegions [\#1287](https://github.com/bigdatagenomics/adam/pull/1287) ([liamlee](https://github.com/liamlee)) 
- Populate fields on VariantAnnotation [\#1283](https://github.com/bigdatagenomics/adam/pull/1283) ([heuermh](https://github.com/heuermh))
- Add VCF headers for fields in Variant and VariantAnnotation records [\#1281](https://github.com/bigdatagenomics/adam/pull/1281) ([heuermh](https://github.com/heuermh))
- CGCloud deploy docs [\#1279](https://github.com/bigdatagenomics/adam/pull/1279) ([jpdna](https://github.com/jpdna))
- some style nits [\#1278](https://github.com/bigdatagenomics/adam/pull/1278) ([ryan-williams](https://github.com/ryan-williams))
- use ParsedLoci in loadIndexedBam [\#1277](https://github.com/bigdatagenomics/adam/pull/1277) ([ryan-williams](https://github.com/ryan-williams))
- Increasing unit test coverage for VariantContextConverter [\#1276](https://github.com/bigdatagenomics/adam/pull/1276) ([heuermh](https://github.com/heuermh))
- Expose FeatureRDD to public [\#1275](https://github.com/bigdatagenomics/adam/pull/1275) ([Georgehe4](https://github.com/Georgehe4)) 
- Clean up CLI operation categories and names, and add documentation for CLI [\#1274](https://github.com/bigdatagenomics/adam/pull/1274) ([fnothaft](https://github.com/fnothaft))
- Rename org.bdgenomics.adam.rdd.variation package to o.b.a.rdd.variant [\#1270](https://github.com/bigdatagenomics/adam/pull/1270) ([heuermh](https://github.com/heuermh))
- use testFile in some tests [\#1268](https://github.com/bigdatagenomics/adam/pull/1268) ([ryan-williams](https://github.com/ryan-williams))
- [ADAM-1083] Cleaning up `org.bdgenomics.adam.models`. [\#1267](https://github.com/bigdatagenomics/adam/pull/1267) ([fnothaft](https://github.com/fnothaft))
- make py file py3-forward-compatible [\#1266](https://github.com/bigdatagenomics/adam/pull/1266) ([ryan-williams](https://github.com/ryan-williams))
- rm accidentally-added file [\#1265](https://github.com/bigdatagenomics/adam/pull/1265) ([fnothaft](https://github.com/fnothaft))
- Finishing up the cleanup on org.bdgenomics.adam.rdd. [\#1264](https://github.com/bigdatagenomics/adam/pull/1264) ([fnothaft](https://github.com/fnothaft))
- Clean up `org.bdgenomics.adam.rich` package. [\#1263](https://github.com/bigdatagenomics/adam/pull/1263) ([fnothaft](https://github.com/fnothaft))
- Add docs for transform pipeline, ADAM-on-Toil [\#1262](https://github.com/bigdatagenomics/adam/pull/1262) ([fnothaft](https://github.com/fnothaft))
- updates for bdg utils 0.2.9-SNAPSHOT [\#1261](https://github.com/bigdatagenomics/adam/pull/1261) ([akmorrow13](https://github.com/akmorrow13))
- [ADAM-1233] Expose header lines in Variant-related GenomicRDDs [\#1260](https://github.com/bigdatagenomics/adam/pull/1260) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1221] Bump Spark/Hadoop versions. [\#1258](https://github.com/bigdatagenomics/adam/pull/1258) ([fnothaft](https://github.com/fnothaft))
- Rename org.bdgenomics.adam.rdd.features package to o.b.a.rdd.feature [\#1256](https://github.com/bigdatagenomics/adam/pull/1256) ([heuermh](https://github.com/heuermh))
- Clean up documentation in `org.bdgenomics.adam.projection`. [\#1255](https://github.com/bigdatagenomics/adam/pull/1255) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1221] Bump Spark/Hadoop versions. [\#1254](https://github.com/bigdatagenomics/adam/pull/1254) ([fnothaft](https://github.com/fnothaft))
- Misc shuffle join fixes. [\#1253](https://github.com/bigdatagenomics/adam/pull/1253) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1196] Add support for open ReferenceRegions. [\#1252](https://github.com/bigdatagenomics/adam/pull/1252) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1225] Move helper functions from ADAMFunSuite to SparkFunSuite. [\#1251](https://github.com/bigdatagenomics/adam/pull/1251) ([fnothaft](https://github.com/fnothaft))
- Merge VariantAnnotation and DatabaseVariantAnnotation records [\#1250](https://github.com/bigdatagenomics/adam/pull/1250) ([heuermh](https://github.com/heuermh))
- Miscellaneous VCF fixes [\#1247](https://github.com/bigdatagenomics/adam/pull/1247) ([fnothaft](https://github.com/fnothaft))
- HBase backend for Genotypes [\#1246](https://github.com/bigdatagenomics/adam/pull/1246) ([jpdna](https://github.com/jpdna))
- [ADAM-1242] Clean up dead code in org.bdgenomics.adam.util. [\#1243](https://github.com/bigdatagenomics/adam/pull/1243) ([fnothaft](https://github.com/fnothaft))
- Small cleanup of "replacing uses of deprecated class SAMFileReader" [\#1236](https://github.com/bigdatagenomics/adam/pull/1236) ([fnothaft](https://github.com/fnothaft))
- replacing uses of deprecated class SAMFileReader [\#1235](https://github.com/bigdatagenomics/adam/pull/1235) ([lbergelson](https://github.com/lbergelson))
- [ADAM-1224] Replace BroadcastRegionJoin with tree based algo. [\#1234](https://github.com/bigdatagenomics/adam/pull/1234) ([fnothaft](https://github.com/fnothaft))
- Fix reads2coverage issues [\#1230](https://github.com/bigdatagenomics/adam/pull/1230) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1212] Add empty assembly object, allows Maven build to create sources and javadoc artifacts [\#1215](https://github.com/bigdatagenomics/adam/pull/1215) ([heuermh](https://github.com/heuermh))
- [ADAM-1211] Fix call to move_to_scala_2.sh, reorder Spark 2.x Scala 2.10 and 2.10 sections [\#1214](https://github.com/bigdatagenomics/adam/pull/1214) ([heuermh](https://github.com/heuermh))
- demonstrate multi-allelic gVCF failure - test added [\#1205](https://github.com/bigdatagenomics/adam/pull/1205) ([jpdna](https://github.com/jpdna))
- Merge VariantAnnotation and DatabaseVariantAnnotation records [\#1144](https://github.com/bigdatagenomics/adam/pull/1144) ([heuermh](https://github.com/heuermh))
- Upgrade to bdg-formats-0.10.0 [\#1135](https://github.com/bigdatagenomics/adam/pull/1135) ([fnothaft](https://github.com/fnothaft))

