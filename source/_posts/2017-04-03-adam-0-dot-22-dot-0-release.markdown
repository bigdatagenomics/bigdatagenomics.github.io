---
layout: post
title: "ADAM 0.22.0 Released"
date: 2017-04-03 12:00:00 -0700
comments: true
categories: 
- ADAM
- releases
---

ADAM version 0.22.0 has been [released](https://github.com/bigdatagenomics/adam/releases)!

Due to major changes between Spark versions 1.6 and 2.0, we build for combinations of Apache Spark and Scala versions:
[Spark 1.x and Scala 2.10](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.10-0.22.0),
[Spark 1.x and Scala 2.11](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent_2.11-0.22.0),
[Spark 2.x and Scala 2.10](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-spark2_2.10-0.22.0), and
[Spark 2.x and Scala 2.11](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-spark2_2.11-0.22.0).

The focus of this release was performance, including major improvements to BQSR and INDEL realignment.

More than 80 other issues were closed in this release, including bug fixes around VCF validation and paired end FASTQ parsing
and new features such as pipe API support for features.

The full list of changes since version 0.21.0 is below.

<!-- more -->

**Closed issues:**

- Realign all reads at target site, not just reads with no mismatches [\#1469](https://github.com/bigdatagenomics/adam/issues/1469)
- Parallel file merger fails if the output file is smaller than the HDFS block size [\#1467](https://github.com/bigdatagenomics/adam/issues/1467)
- Add new realigner arguments to docs [\#1465](https://github.com/bigdatagenomics/adam/issues/1465)
- Recalibrate method misspelled as recalibateBaseQualities [\#1463](https://github.com/bigdatagenomics/adam/issues/1463)
- FASTQ may try to split GZIPed files [\#1459](https://github.com/bigdatagenomics/adam/issues/1459)
- Update to Hadoop-BAM 7.8.0 [\#1455](https://github.com/bigdatagenomics/adam/issues/1455)
- Publish Markdown and Scaladoc to the interwebs [\#1453](https://github.com/bigdatagenomics/adam/issues/1453)
- Make VariantContextConverter public [\#1451](https://github.com/bigdatagenomics/adam/issues/1451)
- Apply method in FragmentRDD is package private [\#1445](https://github.com/bigdatagenomics/adam/issues/1445)
- Thread pool will block inside of pipe command for streams too large to buffer [\#1442](https://github.com/bigdatagenomics/adam/issues/1442)
- FeatureRDD.apply() does not allow addition of other parameters with defaults in the case class [\#1439](https://github.com/bigdatagenomics/adam/issues/1439)
- Question : Why the number of paired sequence in adam-0.21.0 less than adam-0.19.0? [\#1424](https://github.com/bigdatagenomics/adam/issues/1424)
- loadCoverage missing from Java API [\#1420](https://github.com/bigdatagenomics/adam/issues/1420)
- Estimate contig lengths in SequenceDictionary for BED, GFF3, GTF, and NarrowPeak feature formats [\#1410](https://github.com/bigdatagenomics/adam/issues/1410)
- loadIntervalList FeatureRDD has empty SequenceDictionary [\#1409](https://github.com/bigdatagenomics/adam/issues/1409)
- problem using transform command [\#1406](https://github.com/bigdatagenomics/adam/issues/1406)
- Add coveralls [\#1403](https://github.com/bigdatagenomics/adam/issues/1403)
- INDEL realigner binary search conditional is flipped [\#1402](https://github.com/bigdatagenomics/adam/issues/1402)
- Delete adam-scripts/R [\#1398](https://github.com/bigdatagenomics/adam/issues/1398)
- Data missing when transfroming FASTQ to Adam [\#1393](https://github.com/bigdatagenomics/adam/issues/1393)
- java.io.FileNotFoundException when file exists [\#1385](https://github.com/bigdatagenomics/adam/issues/1385)
- Off-by-1 error in FASTQ InputFormat start positioning code [\#1383](https://github.com/bigdatagenomics/adam/issues/1383)
- Set the wrong value for end for symbolic alts [\#1381](https://github.com/bigdatagenomics/adam/issues/1381)
- RecordGroupDictionary should support `isEmpty` [\#1380](https://github.com/bigdatagenomics/adam/issues/1380)
- Add pipe API in and out formatters for Features [\#1374](https://github.com/bigdatagenomics/adam/issues/1374)
- Increase visibility for SupportedHeaderLines.allHeaderLines [\#1372](https://github.com/bigdatagenomics/adam/issues/1372)
- Bits of VariantContextConverter don't get ValidationStringencied [\#1371](https://github.com/bigdatagenomics/adam/issues/1371)
- Add Markdown docs for Pipe API [\#1368](https://github.com/bigdatagenomics/adam/issues/1368)
- Array[Consensus] not registered [\#1367](https://github.com/bigdatagenomics/adam/issues/1367)
- ValidationStringency in MDTagging should apply to reads on unknown references [\#1365](https://github.com/bigdatagenomics/adam/issues/1365)
- When doing a release, the SNAPSHOT should bump by 0.1.0, not 0.0.1 [\#1364](https://github.com/bigdatagenomics/adam/issues/1364)
- FromKnowns consensus generator fails if no reads overlap a consensus [\#1362](https://github.com/bigdatagenomics/adam/issues/1362)
- Performance tune-up in BQSR [\#1358](https://github.com/bigdatagenomics/adam/issues/1358)
- Increase visibility for ADAMContext.sc and/or getFs... methods [\#1356](https://github.com/bigdatagenomics/adam/issues/1356)
- Pipe API formatters need to be public [\#1354](https://github.com/bigdatagenomics/adam/issues/1354)
- Version 0.21.0: VariantContextConverter fails for 1000G VCF data [\#1353](https://github.com/bigdatagenomics/adam/issues/1353)
- ConsensusModel's can't really be instantiated [\#1352](https://github.com/bigdatagenomics/adam/issues/1352)
- Runtime conflicts in transitive versions of Guava dependency [\#1350](https://github.com/bigdatagenomics/adam/issues/1350)
- Transcript Effects ignored if more than 1 [\#1347](https://github.com/bigdatagenomics/adam/issues/1347)
- Remove "fork" tag from releases [\#1344](https://github.com/bigdatagenomics/adam/issues/1344)
- Refactor isSorted boolean parameters to sorted [\#1341](https://github.com/bigdatagenomics/adam/issues/1341)
- Loading GZipped VCF returns an empty RDD [\#1333](https://github.com/bigdatagenomics/adam/issues/1333)
- Follow up on error messages in build scripts [\#1331](https://github.com/bigdatagenomics/adam/issues/1331)
- Bump Spark 2 build to Spark 2.1.0 [\#1330](https://github.com/bigdatagenomics/adam/issues/1330)
- FeatureRDD instantiation tries to cache the RDD [\#1321](https://github.com/bigdatagenomics/adam/issues/1321)
- Load queryname sorted BAMs as Fragments [\#1303](https://github.com/bigdatagenomics/adam/issues/1303)
- Run Duplicate Marking on Fragments [\#1302](https://github.com/bigdatagenomics/adam/issues/1302)
- GenomicRDD.pipe may hang on failure error codes [\#1282](https://github.com/bigdatagenomics/adam/issues/1282)
- IllegalArgumentException Wrong FS for vcf_head files on HDFS [\#1272](https://github.com/bigdatagenomics/adam/issues/1272)
- java.io.NotSerializableException: org.bdgenomics.formats.avro.AlignmentRecord [\#1240](https://github.com/bigdatagenomics/adam/issues/1240)
- Investigate sorted join in dataset api [\#1223](https://github.com/bigdatagenomics/adam/issues/1223)
- Support looser validation stringency for loading some VCF Integer fields [\#1213](https://github.com/bigdatagenomics/adam/issues/1213)
- Add new feature-overlap command to demonstrate new region joins [\#1194](https://github.com/bigdatagenomics/adam/issues/1194)
- What should our API at the command line look like? [\#1178](https://github.com/bigdatagenomics/adam/issues/1178)
- Split apart partition and join in ShuffleRegionJoin [\#1175](https://github.com/bigdatagenomics/adam/issues/1175)
- Merging files should be multithreaded [\#1164](https://github.com/bigdatagenomics/adam/issues/1164)
- File _rgdict.avro does not exist [\#1150](https://github.com/bigdatagenomics/adam/issues/1150)
- how to collect the .adam files from Spark cluster multiple nodes and some questions about avocado [\#1140](https://github.com/bigdatagenomics/adam/issues/1140)
- JFYI: tiny forked adam-core "0.20.0" release [\#1139](https://github.com/bigdatagenomics/adam/issues/1139)
- Samtools (htslib) integration testing [\#1120](https://github.com/bigdatagenomics/adam/issues/1120)
- AlignmentRecordRDD does not extend GenomicRDD per javac [\#1092](https://github.com/bigdatagenomics/adam/issues/1092)
- Release ADAM version 0.21.0 [\#1088](https://github.com/bigdatagenomics/adam/issues/1088)
- Difference running markdups with and without projection [\#1014](https://github.com/bigdatagenomics/adam/issues/1014)
- ADAM to BAM conversion fails using relative path [\#1012](https://github.com/bigdatagenomics/adam/issues/1012)
- Refactor SequenceDictionary to use Contig instead of SequenceRecord [\#997](https://github.com/bigdatagenomics/adam/issues/997)
- Customize adam-main cli from configuration file [\#918](https://github.com/bigdatagenomics/adam/issues/918)
- genotypeType for genotypes with multiple OtherAlt alleles? [\#897](https://github.com/bigdatagenomics/adam/issues/897)
- How to convert genotype DataFrame to VariantContext DataFrame / RDD [\#886](https://github.com/bigdatagenomics/adam/issues/886)
- Ensure Java API is up-to-date with Scala API [\#855](https://github.com/bigdatagenomics/adam/issues/855)
- Improve parallelism during FASTA output [\#842](https://github.com/bigdatagenomics/adam/issues/842)
- Explicitly validate user args passed to transform enhancement [\#841](https://github.com/bigdatagenomics/adam/issues/841)
- BroadcastRegionJoin fails with unmapped reads [\#821](https://github.com/bigdatagenomics/adam/issues/821)
- Resolve Fragment vs. SingleReadBucket [\#789](https://github.com/bigdatagenomics/adam/issues/789)
- Add profile for skipping test compilation/resolution [\#713](https://github.com/bigdatagenomics/adam/issues/713)
- Next on empty iterator in BroadcastRegionJoin [\#661](https://github.com/bigdatagenomics/adam/issues/661)
- Cleanup code smell in sort work balancing code [\#635](https://github.com/bigdatagenomics/adam/issues/635)
- Remove reliance on MD tags [\#622](https://github.com/bigdatagenomics/adam/issues/622)
- Provide low-impact alternative to `transform -repartition` for reducing partition size [\#594](https://github.com/bigdatagenomics/adam/issues/594)
- Clean up Rich records [\#577](https://github.com/bigdatagenomics/adam/issues/577)
- Create standardized, interpretable exceptions for error reporting [\#420](https://github.com/bigdatagenomics/adam/issues/420)
- Create ADAM Benchmarking suite [\#120](https://github.com/bigdatagenomics/adam/issues/120)

**Merged and closed pull requests:**

- [ADAM-1469] Don't filter on whether reads have mismatches during realignment [\#1470](https://github.com/bigdatagenomics/adam/pull/1470) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1467] Skip `concat` call if there is only one shard. [\#1468](https://github.com/bigdatagenomics/adam/pull/1468) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1465] Updating realigner CLI docs. [\#1466](https://github.com/bigdatagenomics/adam/pull/1466) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1463] Rename recalibateBaseQualities method as recalibrateBaseQualities [\#1464](https://github.com/bigdatagenomics/adam/pull/1464) ([heuermh](https://github.com/heuermh))
- [ADAM-1453] Add hooks to publish ADAM docs from CI flow. [\#1461](https://github.com/bigdatagenomics/adam/pull/1461) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1459] Don't split FASTQ when compressed. [\#1459](https://github.com/bigdatagenomics/adam/pull/1459) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1451] Make VariantContextConverter class and convert methods public [\#1452](https://github.com/bigdatagenomics/adam/pull/1452) ([fnothaft](https://github.com/fnothaft))
- Moving API overview from building apps doc to new source file. [\#1450](https://github.com/bigdatagenomics/adam/pull/1450) ([heuermh](https://github.com/heuermh))
- [ADAM-1424] Adding test for reads dropped in 0.21.0. [\#1448](https://github.com/bigdatagenomics/adam/pull/1448) ([heuermh](https://github.com/heuermh))
- [ADAM-1439] Add inferSequenceDictionary ctr to FeatureRDD. [\#1447](https://github.com/bigdatagenomics/adam/pull/1447) ([heuermh](https://github.com/heuermh))
- [ADAM-1445] Make apply method for FragmentRDD public. [\#1446](https://github.com/bigdatagenomics/adam/pull/1446) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1442] Fix thread pool deadlock in GenomicRDD.pipe [\#1443](https://github.com/bigdatagenomics/adam/pull/1443) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1164] Add parallel file merger. [\#1441](https://github.com/bigdatagenomics/adam/pull/1441) ([fnothaft](https://github.com/fnothaft))
- Dependency version bump + BroadcastRegionJoin fix [\#1440](https://github.com/bigdatagenomics/adam/pull/1440) ([fnothaft](https://github.com/fnothaft))
- added JavaApi for loadCoverage [\#1437](https://github.com/bigdatagenomics/adam/pull/1437) ([akmorrow13](https://github.com/akmorrow13))
- Update versions, etc. in build docs [\#1435](https://github.com/bigdatagenomics/adam/pull/1435) ([heuermh](https://github.com/heuermh))
- Add test sample(verify number of reads in loadAlignments function) and ADAM SNAPSHOT document [\#1433](https://github.com/bigdatagenomics/adam/pull/1433) ([xubo245](https://github.com/xubo245))
- Add cache argument to loadFeatures, additional Feature timers [\#1427](https://github.com/bigdatagenomics/adam/pull/1427) ([heuermh](https://github.com/heuermh))
- feat: speed up 2bit file extract [\#1426](https://github.com/bigdatagenomics/adam/pull/1426) ([Blaok](https://github.com/Blaok))
- BQSR refactor for perf improvements [\#1423](https://github.com/bigdatagenomics/adam/pull/1423) ([fnothaft](https://github.com/fnothaft))
- Add ADAMContext/GenomicRDD/pipe docs [\#1422](https://github.com/bigdatagenomics/adam/pull/1422) ([fnothaft](https://github.com/fnothaft))
- INDEL realigner cleanup [\#1412](https://github.com/bigdatagenomics/adam/pull/1412) ([fnothaft](https://github.com/fnothaft))
- Estimate contig lengths in SequenceDictionary for BED, GFF3, GTF, and NarrowPeak feature formats [\#1411](https://github.com/bigdatagenomics/adam/pull/1411) ([heuermh](https://github.com/heuermh))
- Add coveralls badge to README.md. [\#1408](https://github.com/bigdatagenomics/adam/pull/1408) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1403] Push coverage reports to Coveralls. [\#1404](https://github.com/bigdatagenomics/adam/pull/1404) ([fnothaft](https://github.com/fnothaft))
- Added instrumentation timers around joins. [\#1401](https://github.com/bigdatagenomics/adam/pull/1401) ([fnothaft](https://github.com/fnothaft))
- Add Apache Spark version to --version text [\#1400](https://github.com/bigdatagenomics/adam/pull/1400) ([heuermh](https://github.com/heuermh))
- [ADAM-1398] Delete adam-scripts/R. [\#1399](https://github.com/bigdatagenomics/adam/pull/1399) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1383] Use gt instead of gteq in FASTQ input format line size checks [\#1396](https://github.com/bigdatagenomics/adam/pull/1396) ([fnothaft](https://github.com/fnothaft))
- Maint spark2 2.11 0.21.0 [\#1395](https://github.com/bigdatagenomics/adam/pull/1395) ([A-Tsai](https://github.com/A-Tsai))
- [ADAM-1393] fix missing reads when transforming fastq to adam [\#1394](https://github.com/bigdatagenomics/adam/pull/1394) ([A-Tsai](https://github.com/A-Tsai))
- [ADAM-1380] Adds isEmpty method to RecordGroupDictionary. [\#1392](https://github.com/bigdatagenomics/adam/pull/1392) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1381] Fix Variant end position. [\#1389](https://github.com/bigdatagenomics/adam/pull/1389) ([fnothaft](https://github.com/fnothaft))
- Make javac see that AlignmentRecordRDD extends GenomicRDD [\#1386](https://github.com/bigdatagenomics/adam/pull/1386) ([fnothaft](https://github.com/fnothaft))
- Added ShuffleRegionJoin usage docs [\#1384](https://github.com/bigdatagenomics/adam/pull/1384) ([devin-petersohn](https://github.com/devin-petersohn))
- Misc. INDEL realigner bugfixes [\#1382](https://github.com/bigdatagenomics/adam/pull/1382) ([fnothaft](https://github.com/fnothaft))
- Add pipe API in and out formatters for Features [\#1378](https://github.com/bigdatagenomics/adam/pull/1378) ([heuermh](https://github.com/heuermh))
- [ADAM-1356] Make ADAMContext.getFsAndFiles and related protected visibility [\#1376](https://github.com/bigdatagenomics/adam/pull/1376) ([heuermh](https://github.com/heuermh))
- [ADAM-1372] Increase visibility for DefaultHeaderLines.allHeaderLines [\#1375](https://github.com/bigdatagenomics/adam/pull/1375) ([heuermh](https://github.com/heuermh))
- [ADAM-1371] Wrap ADAM->htsjdk VariantContext conversion with validation stringency. [\#1373](https://github.com/bigdatagenomics/adam/pull/1373) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1367] Register Consensus array for serialization. [\#1369](https://github.com/bigdatagenomics/adam/pull/1369) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1365] Apply validation stringency to reads on missing contigs when MD tagging [\#1366](https://github.com/bigdatagenomics/adam/pull/1366) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1362] Fixing issue where FromKnowns consensus model fails if no reads hit a target. [\#1363](https://github.com/bigdatagenomics/adam/pull/1363) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1352] Clean up consensus model usage. [\#1357](https://github.com/bigdatagenomics/adam/pull/1357) ([fnothaft](https://github.com/fnothaft))
- Increase visibility for InFormatter case classes from package private to public [\#1355](https://github.com/bigdatagenomics/adam/pull/1355) ([heuermh](https://github.com/heuermh))
- Use htsjdk getAttributeAsList for VCF INFO ANN key [\#1348](https://github.com/bigdatagenomics/adam/pull/1348) ([heuermh](https://github.com/heuermh))
- Fixes parsing variant annotations for multi-allelic rows [\#1346](https://github.com/bigdatagenomics/adam/pull/1346) ([majkiw](https://github.com/majkiw))
- Sort pull requests by id [\#1345](https://github.com/bigdatagenomics/adam/pull/1345) ([heuermh](https://github.com/heuermh))
- HBase genotypes backend -revised [\#1335](https://github.com/bigdatagenomics/adam/pull/1335) ([jpdna](https://github.com/jpdna))
- [ADAM-1330] Move to Spark 2.1.0. [\#1332](https://github.com/bigdatagenomics/adam/pull/1332) ([fnothaft](https://github.com/fnothaft))
- Support deduping fragments [\#1309](https://github.com/bigdatagenomics/adam/pull/1309) ([fnothaft](https://github.com/fnothaft))
- [ADAM-1280] Silence CRAM logging in tests. [\#1294](https://github.com/bigdatagenomics/adam/pull/1294) ([fnothaft](https://github.com/fnothaft))
- Added test to try and repro #1282. [\#1292](https://github.com/bigdatagenomics/adam/pull/1292) ([fnothaft](https://github.com/fnothaft))

