---
layout: post
title: "ADAM 0.24.0 and Cannoli 0.2.0 Released"
date: 2018-03-28
comments: true
categories: 
---

ADAM [version 0.24.0](https://github.com/bigdatagenomics/adam/releases) and
Cannoli [version 0.2.0](https://github.com/bigdatagenomics/cannoli/releases) have been released!

As of version 0.24.0, support for Spark version 1.x and Scala 2.10.x has been dropped. ADAM and
Cannoli currently build against Spark version 2.3.0 and Scala version 2.11.12.

### ADAM Version 0.24.0 ###

Major new features in ADAM version 0.24.0 include Spark SQL support across all genomic data
types and access to the ADAM region join API through Python and R. The ADAM Python and R APIs are
now feature complete relative to ADAM's Java API. ADAM version 0.24.0 also introduces
Hive-style partitioning by genomic range for Parquet-backed Datasets. This greatly improves
performance for genomic range based queries.

For example, to prepare the data, load a BAM file and write to Parquet and partitioned Parquet
{% codeblock %}
import org.bdgenomics.adam.rdd.ADAMContext._
val alignments = sc.loadAlignments("sample.bam")
alignments.saveAsParquet("sample.alignments.adam")
alignments.saveAsPartitionedParquet("sample.alignments.partitioned.adam")
{% endcodeblock %}

then to compare performance, load from Parquet, filter reads to those overlapping the MHC and KIR
genomic regions, and then filter by MAPQ > 20
{% codeblock %}
import org.bdgenomics.adam.models._
import org.bdgenomics.adam.rdd.ADAMContext._

val mhc = ReferenceRegion.fromGenomicRange("6", 28477796L, 33448354L)
val kir = ReferenceRegion.fromGenomicRange("19", 55228187L, 55383188L)

val alignments = sc.loadParquetAlignments("sample.alignments.adam")
  .filterByOverlappingRegions(Seq(mhc, kir))

alignments.dataset.filter("mapq > 20").count()
{% endcodeblock %}

or use a pushdown predicate to filter reads first by MAPQ > 20 and then to those overlapping the MHC
and KIR genomic regions
{% codeblock %}
import org.apache.parquet.filter2.dsl.Dsl._
import org.apache.parquet.filter2.predicate.FilterPredicate

import org.bdgenomics.adam.models._
import org.bdgenomics.adam.rdd.ADAMContext._

val mhc = ReferenceRegion.fromGenomicRange("6", 28477796L, 33448354L)
val kir = ReferenceRegion.fromGenomicRange("19", 55228187L, 55383188L)
val mapq: FilterPredicate = (IntColumn("mapq") > 20)

val alignments = sc.loadParquetAlignments("sample.alignments.adam", optPredicate = Some(mapq))
  .filterByOverlappingRegions(Seq(mhc, kir))

alignments.dataset.count()
{% endcodeblock %}

or load only those reads overlapping the MHC and KIR genomic regions from partitioned Parquet
and then filter by MAPQ > 20
{% codeblock %}
import org.bdgenomics.adam.models._
import org.bdgenomics.adam.rdd.ADAMContext._

val mhc = ReferenceRegion.fromGenomicRange("6", 28477796L, 33448354L)
val kir = ReferenceRegion.fromGenomicRange("19", 55228187L, 55383188L)

val alignments = sc.loadPartitionedParquetAlignments("sample.alignments.partitioned.adam", Seq(mhc, kir))

alignments.dataset.filter("mapq > 20").count()
{% endcodeblock %}


Performance metrics for the above examples, times in seconds:

NA12878_S1, 1,582,771,014 alignments
|                                                  | 16 executors | 32 executors |
|--------------------------------------------------|--------------|--------------|
| Load Parquet and filter                          |           86 |           79 |
| Load Parquet with pushdown predicate and filter  |           57 |           45 |
| Load partitioned Parquet and filter              |           44 |           34 |


Similar performance metrics for variants initially loaded from VCF:

gnomad.exomes.r2.0.2.sites, 17,009,588 variants
|                                                  | 16 executors | 32 executors |
|--------------------------------------------------|--------------|--------------|
| Load Parquet and filter                          |           28 |           24 |
| Load Parquet with pushdown predicate and filter  |           26 |           24 |
| Load partitioned Parquet and filter              |           30 |           30 |

gnomad.genomes.r2.0.0.2.sites, 160,938,997 variants
|                                                  | 16 executors | 32 executors |
|--------------------------------------------------|--------------|--------------|
| Load Parquet and filter                          |           60 |           47 |
| Load Parquet with pushdown predicate and filter  |           57 |           51 |
| Load partitioned Parquet and filter              |           32 |           22 |


### Cannoli Version 0.2.0 ###

With version 0.2.0, Cannoli now provides a functional API for interactive use in
`cannoli-shell` (a Scala REPL based on Spark Shell, similar to `adam-shell`) and
notebooks such as [Jupyter](https://jupyter.org/), [Zeppelin](https://zeppelin.apache.org/),
and [Spark Notebook](http://spark-notebook.io/). This API allows for multiple
Cannoli-wrapped bioinformatics tools as processes in a larger Spark-based workflow
without having to write out to disk intermediately.

{% codeblock %}
$ cannoli-shell

scala> import org.bdgenomics.adam.rdd.ADAMContext._
scala> import org.bdgenomics.cannoli.cli._

scala> val reads = sc.loadInterleavedFastqAsFragments("sample.ifq")
reads: RDDBoundFragmentRDD with 0 reference sequences, 0 read groups,
and 0 processing steps

scala> val bwaArgs = new BwaFnArgs()
scala> bwaArgs.sample = "sample"
scala> bwaArgs.indexPath = "ref.fa"
scala> bwaArgs.useDocker = true

scala> val alignments = new BwaFn(bwaArgs, sc).apply(reads)
alignments: RDDBoundAlignmentRecordRDD with 86 reference sequences,
1 read groups, and 12 processing steps

scala> val sorted = alignments.sortReadsByReferencePositionAndIndex()
markdup: RDDBoundAlignmentRecordRDD with 86 reference sequences,
1 read groups, and 12 processing steps

scala> val markdup = sorted.markDuplicates()
markdup: RDDBoundAlignmentRecordRDD with 86 reference sequences,
1 read groups, and 12 processing steps

scala> val freebayesArgs = new FreebayesFnArgs()
scala> freebayesArgs.referencePath = "ref.fa"
scala> freebayesArgs.useDocker = true

scala> val genotypes = new FreebayesFn(freebayesArgs, sc).apply(markdup).toGenotypes
genotypes: ParquetUnboundGenotypeRDD with 24 reference sequences and 1 samples

scala> genotypes.dataset.count()
res1: Long = 42057

scala> genotypes.dataset.filter("variant.quality > 20").count()
res1: Long = 39873
{% endcodeblock %}


Changes since Previous Releases
===============================

The full list of changes to ADAM since version 0.23.0 and Cannoli since version 0.1.0 are below.

<!-- more -->

### ADAM Version 0.24.0 ###

**Closed issues:**

 - Phred values from 156–254 do not round trip properly between log space [\#1964](https://github.com/bigdatagenomics/adam/issues/1964)
 - Support VCF lines with positions at 0 [\#1959](https://github.com/bigdatagenomics/adam/issues/1959)
 - Don't initialize non-ref values to Int.MinValue [\#1957](https://github.com/bigdatagenomics/adam/issues/1957)
 - Support downsampling in recalibration [\#1955](https://github.com/bigdatagenomics/adam/issues/1955)
 - Cannot waive validation stringency for INFO Number=.,Type=Flag fields [\#1939](https://github.com/bigdatagenomics/adam/issues/1939)
 - Clip phred scores below Int.MaxValue [\#1934](https://github.com/bigdatagenomics/adam/issues/1934)
 - ADAMContext.getFsAndFilesWithFilter should throw exception if paths null or empty [\#1932](https://github.com/bigdatagenomics/adam/issues/1932)
 - Bump to Spark 2.3.0 [\#1931](https://github.com/bigdatagenomics/adam/issues/1931)
 - util.FileExtensions should be public for use downstream in Cannoli [\#1927](https://github.com/bigdatagenomics/adam/issues/1927)
 - Reduce logging level for ADAMKryoRegistrator [\#1925](https://github.com/bigdatagenomics/adam/issues/1925)
 - Revisit performance implications of commit 1eed8e8 [\#1923](https://github.com/bigdatagenomics/adam/issues/1923)
 - add akmorrow13 to PyPl for bdgenomics.adam [\#1919](https://github.com/bigdatagenomics/adam/issues/1919)
 - Read the Docs build failing with TypeError: super() argument 1 must be type, not None [\#1917](https://github.com/bigdatagenomics/adam/issues/1917)
 - Bump Hadoop-BAM dependency to 7.9.2. [\#1915](https://github.com/bigdatagenomics/adam/issues/1915)
 - cannot run pyadam from adam distribution 0.23.0 [\#1914](https://github.com/bigdatagenomics/adam/issues/1914)
 - adam2fasta/q are missing asSingleFile, disableFastConcat [\#1912](https://github.com/bigdatagenomics/adam/issues/1912)
 - Pipe API doesn't properly handle multiple arguments and spaces [\#1909](https://github.com/bigdatagenomics/adam/issues/1909)
 - Bump to HTSJDK 2.13.2 [\#1907](https://github.com/bigdatagenomics/adam/issues/1907)
 - S3A error: HTTP request: Timeout waiting for connection from pool [\#1906](https://github.com/bigdatagenomics/adam/issues/1906)
 - InputStream passed to VCFHeaderReader does not get closed [\#1900](https://github.com/bigdatagenomics/adam/issues/1900)
 - Support INFO fields set to missing [\#1898](https://github.com/bigdatagenomics/adam/issues/1898)
 - CLI to transfer between cloud storage and HDFS [\#1896](https://github.com/bigdatagenomics/adam/issues/1896)
 - Jenkins does not run python or R tests [\#1889](https://github.com/bigdatagenomics/adam/issues/1889)
 - pyadam throws application option error [\#1886](https://github.com/bigdatagenomics/adam/issues/1886)
 - ReferenceRegion in python does not exist [\#1884](https://github.com/bigdatagenomics/adam/issues/1884)
 - Caching GenomicRDD in pyspark [\#1883](https://github.com/bigdatagenomics/adam/issues/1883)
 - adam-submit aborts if ADAM_HOME is set [\#1882](https://github.com/bigdatagenomics/adam/issues/1882)
 - Allow piped commands to timeout [\#1875](https://github.com/bigdatagenomics/adam/issues/1875)
 - loadVcf does not dedupe sample ID [\#1874](https://github.com/bigdatagenomics/adam/issues/1874)
 - Add coverage command for reporting read coverage [\#1873](https://github.com/bigdatagenomics/adam/issues/1873)
 - Only python 2?  [\#1871](https://github.com/bigdatagenomics/adam/issues/1871)
 - Support VariantContextRDD from SQL [\#1867](https://github.com/bigdatagenomics/adam/issues/1867)
 - Cannot find `find-adam-assembly.sh` in bioconda build [\#1862](https://github.com/bigdatagenomics/adam/issues/1862)
 - `_jvm.java.lang.Class.forName` does not work for certain configurations [\#1858](https://github.com/bigdatagenomics/adam/issues/1858)
 - Formatting error in CHANGES.md [\#1857](https://github.com/bigdatagenomics/adam/issues/1857)
 - Various improvements to readthedocs documentation [\#1853](https://github.com/bigdatagenomics/adam/issues/1853)
 - add filterByOverlappingRegion(query: ReferenceRegion) to R and python APIs [\#1852](https://github.com/bigdatagenomics/adam/issues/1852)
 - Support adding VCF header lines from Python [\#1840](https://github.com/bigdatagenomics/adam/issues/1840)
 - Support loadIndexedBam from Python [\#1836](https://github.com/bigdatagenomics/adam/issues/1836)
 - Add link to awesome list of applications that extend ADAM [\#1832](https://github.com/bigdatagenomics/adam/issues/1832)
 - loadIndexed bam lazily throws Exception if index does not exist [\#1830](https://github.com/bigdatagenomics/adam/issues/1830)
 - OAuth credentials for Github in Coveralls configuration are no longer valid [\#1829](https://github.com/bigdatagenomics/adam/issues/1829)
 - base counts per position [\#1825](https://github.com/bigdatagenomics/adam/issues/1825)
 - Issues loading BAM files in Google FS [\#1816](https://github.com/bigdatagenomics/adam/issues/1816)
 - Error when writing a vcf file to Parquet [\#1810](https://github.com/bigdatagenomics/adam/issues/1810)
 - transformAlignments cannot repartition files [\#1808](https://github.com/bigdatagenomics/adam/issues/1808)
 - GenotypeRDD should support `toVariants` method [\#1806](https://github.com/bigdatagenomics/adam/issues/1806)
 - Add support for python and R in Homebrew formula [\#1796](https://github.com/bigdatagenomics/adam/issues/1796)
 - Add `transformVariantContexts` or similar to cli [\#1793](https://github.com/bigdatagenomics/adam/issues/1793)
 - Issue while using Sorting option [\#1791](https://github.com/bigdatagenomics/adam/issues/1791)
 - Issue with adam2vcf [\#1787](https://github.com/bigdatagenomics/adam/issues/1787)
 - Remove explicit `<compile>` scopes from submodule POMs [\#1786](https://github.com/bigdatagenomics/adam/issues/1786)
 - java.nio.file.ProviderNotFoundException (Provider "s3" not found) [\#1732](https://github.com/bigdatagenomics/adam/issues/1732)
 - Accessing GenomicRDD join functions in python [\#1728](https://github.com/bigdatagenomics/adam/issues/1728)
 - ArrayIndexOutOfBoundsException in PhredUtils$.phredToSuccessProbability [\#1714](https://github.com/bigdatagenomics/adam/issues/1714)
 - Add ability to specify region bounds to pipe command [\#1707](https://github.com/bigdatagenomics/adam/issues/1707)
 - Unable to run pyadam, SQLException: Failed to start database 'metastore_db' [\#1666](https://github.com/bigdatagenomics/adam/issues/1666)
 - SAMFormatException: Unrecognized tag type: ^@ [\#1657](https://github.com/bigdatagenomics/adam/issues/1657)
 - IndexOutOfBoundsException in BAMInputFormat.getSplits [\#1656](https://github.com/bigdatagenomics/adam/issues/1656)
 - overlaps considers that Strand.FORWARD cannot overlap with Strand.INDEPENDENT [\#1650](https://github.com/bigdatagenomics/adam/issues/1650)
 - migration converters [\#1629](https://github.com/bigdatagenomics/adam/issues/1629)
 - RFC: Removing Spark 1.x, Scala 2.10 support in 0.24.0 release [\#1597](https://github.com/bigdatagenomics/adam/issues/1597)
 - Eliminate unused ConcreteADAMRDDFunctions class [\#1580](https://github.com/bigdatagenomics/adam/issues/1580)
 - Add set theory/statistics packages to ADAM [\#1533](https://github.com/bigdatagenomics/adam/issues/1533)
 - Evaluate Apache Carbondata INDEXED column store file format for genomics [\#1527](https://github.com/bigdatagenomics/adam/issues/1527)
 - Stranded vs unstranded in getReferenceRegions() for features [\#1513](https://github.com/bigdatagenomics/adam/issues/1513)
 - Question:How to tranform a line of sam to AlignmentRecord? [\#1425](https://github.com/bigdatagenomics/adam/issues/1425)
 - Excessive compilation warnings about multiple scala libraries [\#695](https://github.com/bigdatagenomics/adam/issues/695)
 - Support Hive-style partitioning [\#651](https://github.com/bigdatagenomics/adam/issues/651)

**Merged and closed pull requests:**

 - [ADAM-1964] Lower point where phred conversions are done using log code. [\#1965](https://github.com/bigdatagenomics/adam/pull/1965) ([fnothaft](https://github.com/fnothaft))
 - Add utility methods for adam-shell. [\#1958](https://github.com/bigdatagenomics/adam/pull/1958) ([heuermh](https://github.com/heuermh))
 - [ADAM-1955] Add support for downsampling during recalibration table generation [\#1963](https://github.com/bigdatagenomics/adam/pull/1963) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1957] Don't initialize missing likelihoods to MinValue. [\#1961](https://github.com/bigdatagenomics/adam/pull/1961) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1959] Support VCF rows at position 0. [\#1960](https://github.com/bigdatagenomics/adam/pull/1960) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-651] Implement Hive-style partitioning by genomic range of Parquet backed datasets [\#1948](https://github.com/bigdatagenomics/adam/pull/1948) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1914] Python profile needs to be specified for egg to be in distribution. [\#1946](https://github.com/bigdatagenomics/adam/pull/1946) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1917] Delete dependency on fulltoc. [\#1944](https://github.com/bigdatagenomics/adam/pull/1944) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1917] Try 3: fix Sphinx fulltoc. [\#1943](https://github.com/bigdatagenomics/adam/pull/1943) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1917] Set Sphinx version in requirements.txt. [\#1942](https://github.com/bigdatagenomics/adam/pull/1942) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1917] Set minimal Sphinx version for Readthedocs build. [\#1941](https://github.com/bigdatagenomics/adam/pull/1941) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1939] Allow validation stringency to waive off FLAG arrays. [\#1940](https://github.com/bigdatagenomics/adam/pull/1940) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1915] Bump to Hadoop-BAM 7.9.2. [\#1938](https://github.com/bigdatagenomics/adam/pull/1938) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1934] Clip phred values to 3233, instead of Int.MaxValue. [\#1936](https://github.com/bigdatagenomics/adam/pull/1936) ([fnothaft](https://github.com/fnothaft))
 - Ignore VCF INFO fields with number=G when stringency=LENIENT [\#1935](https://github.com/bigdatagenomics/adam/pull/1935) ([jpdna](https://github.com/jpdna))
 - [ADAM-1931] Bump to Spark 2.3.0. [\#1933](https://github.com/bigdatagenomics/adam/pull/1933) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1840] Support adding VCF header lines from Python. [\#1930](https://github.com/bigdatagenomics/adam/pull/1930) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1927] Increase visibility for util.FileExtensions for use downstream. [\#1929](https://github.com/bigdatagenomics/adam/pull/1929) ([heuermh](https://github.com/heuermh))
 - [ADAM-1925] Reduce logging level for ADAMKryoRegistrator. [\#1928](https://github.com/bigdatagenomics/adam/pull/1928) ([heuermh](https://github.com/heuermh))
 - [ADAM-1923] Revert 1eed8e8 [\#1926](https://github.com/bigdatagenomics/adam/pull/1926) ([fnothaft](https://github.com/fnothaft))
 - Use SparkFiles.getRootDirectory in local mode. [\#1924](https://github.com/bigdatagenomics/adam/pull/1924) ([heuermh](https://github.com/heuermh))
 - [ADAM-651] Implement Hive-style partitioning by genomic range of Parquet backed datasets [\#1922](https://github.com/bigdatagenomics/adam/pull/1922) ([jpdna](https://github.com/jpdna))
 - Make Spark SQL APIs supported across all types [\#1921](https://github.com/bigdatagenomics/adam/pull/1921) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1909] Refactor pipe cmd parameter from String to Seq[String]. [\#1920](https://github.com/bigdatagenomics/adam/pull/1920) ([heuermh](https://github.com/heuermh))
 - Add Google Cloud documentation [\#1918](https://github.com/bigdatagenomics/adam/pull/1918) ([Georgehe4](https://github.com/Georgehe4))
 - [ADAM-1917] Load sphinxcontrib.fulltoc with imp.load_sources. [\#1916](https://github.com/bigdatagenomics/adam/pull/1916) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1912] Add asSingleFile, disableFastConcat to adam2fasta/q. [\#1913](https://github.com/bigdatagenomics/adam/pull/1913) ([heuermh](https://github.com/heuermh))
 - [ADAM-651] Hive-style partitioning of parquet files by genomic position [\#1911](https://github.com/bigdatagenomics/adam/pull/1911) ([jpdna](https://github.com/jpdna))
 - Minor unit test/style fixes. [\#1910](https://github.com/bigdatagenomics/adam/pull/1910) ([heuermh](https://github.com/heuermh))
 - [ADAM-1907] Bump to HTSJDK 2.13.2. [\#1908](https://github.com/bigdatagenomics/adam/pull/1908) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1882] Don't abort adam-submit if ADAM_HOME is set. [\#1905](https://github.com/bigdatagenomics/adam/pull/1905) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1806] Add toVariants conversion from GenotypeRDD. [\#1904](https://github.com/bigdatagenomics/adam/pull/1904) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1882] Return true if ADAM_HOME is set, not exit 0. [\#1903](https://github.com/bigdatagenomics/adam/pull/1903) ([heuermh](https://github.com/heuermh))
 - [ADAM-1900] Close stream after reading VCF header. [\#1901](https://github.com/bigdatagenomics/adam/pull/1901) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1898] Support converting INFO fields set to empty ('.'). [\#1899](https://github.com/bigdatagenomics/adam/pull/1899) ([fnothaft](https://github.com/fnothaft))
 - Add Kryo registration for two classes required for Spark 2.3.0. [\#1897](https://github.com/bigdatagenomics/adam/pull/1897) ([jpdna](https://github.com/jpdna))
 - [ADAM-1853] Various improvements to readthedocs documentation. [\#1893](https://github.com/bigdatagenomics/adam/pull/1893) ([heuermh](https://github.com/heuermh))
 - [ADAM-1889][ADAM-1884] updated ReferenceRegion in python [\#1892](https://github.com/bigdatagenomics/adam/pull/1892) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1889] Run R/Python tests. [\#1890](https://github.com/bigdatagenomics/adam/pull/1890) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1886] fix for pyadam to recognize >1 egg file [\#1887](https://github.com/bigdatagenomics/adam/pull/1887) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1883] Python and R caching [\#1885](https://github.com/bigdatagenomics/adam/pull/1885) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1875] Add ability to timeout a piped command. [\#1881](https://github.com/bigdatagenomics/adam/pull/1881) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1871] Fix print call that broke python 3 support. [\#1880](https://github.com/bigdatagenomics/adam/pull/1880) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1832] Use awesome list style and link to bigdatagenomics/awesome-adam. [\#1879](https://github.com/bigdatagenomics/adam/pull/1879) ([heuermh](https://github.com/heuermh))
 - [ADAM-651] Hive-style partitioning of parquet files by genomic position [\#1878](https://github.com/bigdatagenomics/adam/pull/1878) ([jpdna](https://github.com/jpdna))
 - [ADAM-1874] Dedupe samples when loading VCFs. [\#1876](https://github.com/bigdatagenomics/adam/pull/1876) ([fnothaft](https://github.com/fnothaft))
 - Fixes Coverage python API and adds tests [\#1870](https://github.com/bigdatagenomics/adam/pull/1870) ([akmorrow13](https://github.com/akmorrow13))
 - added filterByOverlappingRegion for python [\#1869](https://github.com/bigdatagenomics/adam/pull/1869) ([akmorrow13](https://github.com/akmorrow13))
 - Add command line option for populating nested variant.annotation field in Genotype records. [\#1865](https://github.com/bigdatagenomics/adam/pull/1865) ([heuermh](https://github.com/heuermh))
 - Hive partitioned(v4) rebased [\#1864](https://github.com/bigdatagenomics/adam/pull/1864) ([jpdna](https://github.com/jpdna))
 - [ADAM-1597] Move to Scala 2.11 and Spark 2.x. [\#1861](https://github.com/bigdatagenomics/adam/pull/1861) ([heuermh](https://github.com/heuermh))
 - [ADAM-1857] Fix formatting error due to forward slashes. [\#1860](https://github.com/bigdatagenomics/adam/pull/1860) ([heuermh](https://github.com/heuermh))
 - [ADAM-1858] Use getattr instead of Class.forName from python API. [\#1859](https://github.com/bigdatagenomics/adam/pull/1859) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1836] Adds loadIndexedBam API to Python and Java. [\#1837](https://github.com/bigdatagenomics/adam/pull/1837) ([fnothaft](https://github.com/fnothaft))
 - Added check for bam index files in loadIndexedBam [\#1831](https://github.com/bigdatagenomics/adam/pull/1831) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1793] Adding vcf2adam and adam2vcf that handle separate variant and genotype data. [\#1794](https://github.com/bigdatagenomics/adam/pull/1794) ([heuermh](https://github.com/heuermh))
 - added adam notebook [\#1778](https://github.com/bigdatagenomics/adam/pull/1778) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1666] SQLContext creation fix for Spark 2.x [\#1777](https://github.com/bigdatagenomics/adam/pull/1777) ([akmorrow13](https://github.com/akmorrow13))
 - Add optional accumulator for VCF header lines to VCFOutFormatter. [\#1727](https://github.com/bigdatagenomics/adam/pull/1727) ([heuermh](https://github.com/heuermh))
 - add hive style partitioning for contigName [\#1620](https://github.com/bigdatagenomics/adam/pull/1620) ([jpdna](https://github.com/jpdna))
 - Add loadReadsFromSamString function into ADAMContext [\#1434](https://github.com/bigdatagenomics/adam/pull/1434) ([xubo245](https://github.com/xubo245))
 
 
### Cannoli Version 0.2.0 ###

**Closed issues:**

 - Update ADAM dependency version to 0.24.0. [\#118](https://github.com/bigdatagenomics/cannoli/issues/118)
 - Javadoc error and warnings [\#115](https://github.com/bigdatagenomics/cannoli/issues/115)
 - Update pipe method calls due to latest ADAM 0.24.0 snapshot [\#114](https://github.com/bigdatagenomics/cannoli/issues/114)
 - Split commands with subcommands into separate Cannoli CLI classes [\#110](https://github.com/bigdatagenomics/cannoli/issues/110)
 - Jenkins build failing due to upstream changes. [\#108](https://github.com/bigdatagenomics/cannoli/issues/108)
 - Provide functions for use in cannoli-shell or notebooks. [\#104](https://github.com/bigdatagenomics/cannoli/issues/104)
 - Error running BWA with Docker [\#103](https://github.com/bigdatagenomics/cannoli/issues/103)
 - Allow use of Singularity instead of Docker [\#98](https://github.com/bigdatagenomics/cannoli/issues/98)
 - Bump ADAM dependency version to 0.24.0-SNAPSHOT. [\#95](https://github.com/bigdatagenomics/cannoli/issues/95)
 - Drop support for Scala 2.10 and Spark 1.x. [\#94](https://github.com/bigdatagenomics/cannoli/issues/94)
 - Tidy up FreeBayes [\#67](https://github.com/bigdatagenomics/cannoli/issues/67)
 - Support loading reference files from HDFS/other file system [\#50](https://github.com/bigdatagenomics/cannoli/issues/50)
 - Attributes from freebayes header missing from variants and genotypes [\#43](https://github.com/bigdatagenomics/cannoli/issues/43)
 - Factor out docker/mapping code [\#34](https://github.com/bigdatagenomics/cannoli/issues/34)
 - Add wrappers for GMAP and GSNAP aligners [\#29](https://github.com/bigdatagenomics/cannoli/issues/29)
 - Jenkins failures due to missing publish_scaladoc.sh [\#21](https://github.com/bigdatagenomics/cannoli/issues/21)

**Merged and closed pull requests:**

 - [CANNOLI-118] Update ADAM dependency version to 0.24.0. [\#121](https://github.com/bigdatagenomics/cannoli/pull/121) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-110] Split commands with subcommands into separate Cannoli CLI classes. [\#117](https://github.com/bigdatagenomics/cannoli/pull/117) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-115] Fix javadoc error and warnings. [\#116](https://github.com/bigdatagenomics/cannoli/pull/116) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-108] Command argument to pipe is now Seq[String]. [\#109](https://github.com/bigdatagenomics/cannoli/pull/109) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-98] Adding container builder. [\#107](https://github.com/bigdatagenomics/cannoli/pull/107) ([heuermh](https://github.com/heuermh))
 - Allow Singularity to run containers [\#106](https://github.com/bigdatagenomics/cannoli/pull/106) ([jpdna](https://github.com/jpdna))
 - [CANNOLI-95] Bump ADAM dependency version to 0.24.0-SNAPSHOT [\#102](https://github.com/bigdatagenomics/cannoli/pull/102) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-94] Dropping support for Scala 2.10 and Spark 1.x. [\#101](https://github.com/bigdatagenomics/cannoli/pull/101) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-94][CANNOLI-95] Drop support for Scala 2.10 and Spark 1.x. [\#100](https://github.com/bigdatagenomics/cannoli/pull/100) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-43] Use accumulator for VCF header lines. [\#72](https://github.com/bigdatagenomics/cannoli/pull/72) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-104] Provide functions for use in cannoli-shell or notebooks. [\#69](https://github.com/bigdatagenomics/cannoli/pull/69) ([heuermh](https://github.com/heuermh))
 - Add CannoliCommand and CannoliAlignerCommand. [\#54](https://github.com/bigdatagenomics/cannoli/pull/54) ([heuermh](https://github.com/heuermh))
 - [CANNOLI-29] Add minimal GMAP and GSNAP wrappers. [\#32](https://github.com/bigdatagenomics/cannoli/pull/32) ([heuermh](https://github.com/heuermh))
