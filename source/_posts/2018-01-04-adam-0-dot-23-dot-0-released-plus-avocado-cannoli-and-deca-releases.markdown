---
layout: post
title: "ADAM 0.23.0 Released (+ Avocado and DECA releases)"
date: 2018-01-04 09:47:53 -0800
comments: true
categories: 
---

We are excited to announce the availability of the ADAM 0.23.0 release, along
with releases of Avocado germline variant caller (release 0.1.0) and the DECA
copy number variant caller (release 0.2.0). These releases contain an extensive
number of feature additions, performance improvements, and bug patches, with
over 375 issues closed and pull requests merged or closed since the last ADAM
release.

Some of the highlights include:

* A validated, high-performance end-to-end alignment/variant calling pipeline
  using ADAM, Cannoli, and Avocado.
* Support for manipulating data using Spark SQL.
* R and Python APIs for ADAM, including the ability to get a working deployment
  of ADAM simply by running `pip install bdgenomics.adam`.

With this release, we have also moved our documentation to Read The Docs:

* [Read the Docs for ADAM](http://adam.readthedocs.io/en/latest/)
* [Read the Docs for Avocado](http://bdg-avocado.readthedocs.io/en/latest/)
* [Read the Docs for DECA](http://bdg-deca.readthedocs.io/en/latest/)

This documentation describes how to deploy our tools on a variety of platforms,
including a local cluster, cloud computing, and through the
[Toil](https://github.com/bd2kgenomics/toil) workflow manager. We already have
a `pip` installable Toil workflow for calling copy number variants with DECA,
which is packaged as part of the
[bdgenomics.workflows](http://bdg-workflows.readthedocs.io/en/latest/) library.

This release is the last release of ADAM that supports Spark 1.x and Scala 2.10.
The upcoming release of ADAM will only support Spark 2.x and Scala 2.11. Avocado
and DECA have already dropped support for Spark 1.x.

Over the upcoming few weeks, we are working on a release of
[Cannoli](https://github.com/bigdatagenomics/cannoli), as well as Toil workflows
for running the ADAM/Avocado/Cannoli variant calling pipeline, and a preprint
describing the pipeline in more depth. We also are working on a release of the
[Mango](https://github.com/bigdatagenomics/mango) visualization tool, which uses
ADAM as a backend for interactively visualizing large genomics datasets. Stay
tuned for more info!

Variant Calling with Cannoli, ADAM, Avocado, and DECA
=====================================================

With the collection of tools we have released, you can run highly rapid and
accurate variant calling entirely in Apache Spark. While we have introduced
Avocado and DECA earlier in this post, we haven't talked about Cannoli yet.
Cannoli---Italian for "a little pipe"---uses ADAM's [pipe API](http://adam.readthedocs.io/en/adam-parent_2.11-0.23.0/api/pipes/)
to parallelize commonly used genomics tools. Currently, Cannoli supports
aligning reads with Bowtie, Bowtie2, and BWA; calling variants with FreeBayes;
and annotating variant effects with SnpEff. We are working on support for many
more tools, as you can see in our [issue tracker](https://github.com/bigdatagenomics/cannoli/issues).
Please let us know if you are interested in any specific tool---or even
better---in helping us add support for a specific tool. ADAM's pipe API makes
it extremely easy to parallelize an existing single node genomic analysis tool,
and most tools can be implemented on top of the pipe API in less than 10 lines
of code. For example, here's how you could launch BWA using ADAM's Pipe API in
Python:

{% img center /images/pipe.png 750 %}

By using Cannoli, we can accelerate alignment with BWA to take approximately
10--15 minutes when running on a 1,024 core cluster.

We can couple this rapid alignment pipeline with the fast preprocessing stages in
ADAM and the variant calling stages in Avocado to call variants on a 60x coverage WGS
dataset in approximately 45 minutes on a 1,024 core cluster. Avocado can be used to
call variants on a single sample, or to jointly call variants using a [gVCF-based
workflow](http://bdg-avocado.readthedocs.io/en/latest/workflows/joint.html). When
running on 1,024 cores, we were able to jointly genotype more than 10TB of gVCFs
within approximately 6 hours. Avocado has >99% accuracy when genotyping SNPs,
and >96% accuracy when genotyping INDELs. Detailed benchmarking results can be
found in [Chapter 8 of this thesis](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2017/EECS-2017-204.pdf).
Avocado is two times faster than the GATK4's Spark-based implementation of the
HaplotypeCaller, although it is worth pointing out that this is an unfair
comparison, as the HaplotypeCaller performs local reassembly, while Avocado does
not.

One interesting comparison is between the duplicate marking and BQSR tools in
ADAM and in the GATK4. In both cases, ADAM's implementation is faster than the
GATK4's equivalent implementation. 

{% img center /images/speedup-md.png %}

{% img center /images/speedup-bqsr.png %}

We have work-in-progress towards a Spark SQL-based implementation of duplicate
marking, which will provide an additional >20% performance improvement. We hope to
introduce this new duplicate marker in the 0.24.0 release of ADAM.

Manipulating Data using Spark SQL
=================================

Since Apache Spark 1.6, there has been a major push in the Spark project to
rearchitect Spark around the Catalyst query optimizer and the Tungsten code
execution engine. These two engines are hidden behind Spark SQL's DataFrame
and Dataset APIs, which provide a SQL-like interface for manipulating data
using Spark. Unlike Spark's Resilient Distributed Dataset (RDD) API, the
DataFrame API allows the Catalyst query optimizer to examine the function that
the user is running. Catalyst can then rewrite the query so that it runs in a
more efficient manner, and can implement the query using the Tungsten engine
with performance that approaches native performance. This can provide
order-of-magnitude performance improvements for some queries, and it also
provides users with uniform query performance across Scala, Java, SQL, Python,
and R. 

Although Spark SQL was introduced in 2015, we were not able to take advantage
of Spark SQL in ADAM until recently. While ADAM has always described genomics
data using a set of schemas, the library we used to represent these schemas
([Apache Avro](https://avro.apache.org)) was not compatible with Spark SQL. To
resolve this, we updated our core [`GenomicRDD` interfaces](http://adam.readthedocs.io/en/adam-parent_2.11-0.23.0/api/genomicRdd/)
to transparently convert between Spark's RDD and DataFrame/Dataset APIs. We
describe the architecture we use for converting between these two representations
[here](http://adam.readthedocs.io/en/adam-parent_2.11-0.23.0/api/genomicRdd/#transforming-genomicrdds-via-spark-sql).
With the Spark SQL query interfaces built into `GenomicRDD`s, you can begin
running SQL queries on genomic data in fewer than 5 lines of code:

{% codeblock %}
$ adam-shell 

Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.2.1
      /_/
         
Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_152)

scala> import org.bdgenomics.adam.rdd.ADAMContext._
import org.bdgenomics.adam.rdd.ADAMContext._

scala> val reads = sc.loadAlignments("adam-core/src/test/resources/small.sam")
reads: org.bdgenomics.adam.rdd.read.AlignmentRecordRDD = RDDBoundAlignmentRecordRDD with 2 reference sequences, 0 read groups, and 2 processing steps

scala> reads.transformDataset(_.filter("readMapped=true")).dataset.show
+--------------+----------+---------+-----------+---------+----+--------------------+--------------------+----+-----+--------+---------------------+-------------------+----------+----------+----------+----------+-------------------------+-------------+------------------+------------------+----------------+------------------+----------------------+--------------------+--------+--------------------+---------------+-----------------+------------------+--------------+------------------+
|readInFragment|contigName|    start|oldPosition|      end|mapq|            readName|            sequence|qual|cigar|oldCigar|basesTrimmedFromStart|basesTrimmedFromEnd|readPaired|properPair|readMapped|mateMapped|failedVendorQualityChecks|duplicateRead|readNegativeStrand|mateNegativeStrand|primaryAlignment|secondaryAlignment|supplementaryAlignment|mismatchingPositions|origQual|          attributes|recordGroupName|recordGroupSample|mateAlignmentStart|mateContigName|inferredInsertSize|
+--------------+----------+---------+-----------+---------+----+--------------------+--------------------+----+-----+--------+---------------------+-------------------+----------+----------+----------+----------+-------------------------+-------------+------------------+------------------+----------------+------------------+----------------------+--------------------+--------+--------------------+---------------+-----------------+------------------+--------------+------------------+
|             0|         1| 26472783|       null| 26472858|  60|simread:1:2647278...|GTATAAGAGCAGCCTTA...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|240997787|       null|240997862|  60|simread:1:2409977...|CTTTATTTTTATTTTTA...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:39	AS:i:75	N...|           null|             null|              null|          null|              null|
|             0|         1|189606653|       null|189606728|  60|simread:1:1896066...|TGTATCTTCCTCCCCTG...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|207027738|       null|207027813|  60|simread:1:2070277...|TTTAATAAATGTTGATT...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1| 14397233|       null| 14397308|  60|simread:1:1439723...|TAAAATGCCCCCATCTT...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|240344442|       null|240344517|  24|simread:1:2403444...|TACAGGCACCCACCATC...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:61	AS:i:75	N...|           null|             null|              null|          null|              null|
|             0|         1|153978724|       null|153978799|  60|simread:1:1539787...|GCTCACTGCAGCCTCAA...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|237728409|       null|237728484|  28|simread:1:2377284...|TTTCTTTTTCTTTCTTT...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:59	AS:i:75	N...|           null|             null|              null|          null|              null|
|             0|         1|231911906|       null|231911981|  60|simread:1:2319119...|TCATGTAGCATGCATAT...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1| 50683371|       null| 50683446|  60|simread:1:5068337...|GCTCAGGCCTTGCAAGA...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1| 37577445|       null| 37577520|  60|simread:1:3757744...|CCTAGAGAAGCTCCCAC...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|195211965|       null|195212040|  60|simread:1:1952119...|AAATAAAGTTTGGCTTT...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|163841413|       null|163841488|  60|simread:1:1638414...|TGTGTAACTAACATAAT...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|101556378|       null|101556453|  60|simread:1:1015563...|TTTATTTTTTGAGCATG...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1| 20101800|       null| 20101875|  35|simread:1:2010180...|CTCAGGTGATCCACCCG...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:55	AS:i:75	N...|           null|             null|              null|          null|              null|
|             0|         1|186794283|       null|186794358|  60|simread:1:1867942...|GACAAGATAGTACTTGA...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|165341382|       null|165341457|  60|simread:1:1653413...|CTACTCTCATTGACTGT...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|  5469106|       null|  5469181|  60|simread:1:5469106...|CTCATTCTCTCTCCTGC...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1| 89554252|       null| 89554327|  60|simread:1:8955425...|AAATTAAACAGCTCGTT...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|              true|             false|            true|             false|                 false|                null|    null|XS:i:0	AS:i:75	NM...|           null|             null|              null|          null|              null|
|             0|         1|169801933|       null|169802008|  40|simread:1:1698019...|AGACTGGGTCTCACTAT...|null|  75M|    null|                    0|                  0|     false|     false|      true|     false|                    false|        false|             false|             false|            true|             false|                 false|                null|    null|XS:i:52	AS:i:75	N...|           null|             null|              null|          null|              null|
+--------------+----------+---------+-----------+---------+----+--------------------+--------------------+----+-----+--------+---------------------+-------------------+----------+----------+----------+----------+-------------------------+-------------+------------------+------------------+----------------+------------------+----------------------+--------------------+--------+--------------------+---------------+-----------------+------------------+--------------+------------------+
{% endcodeblock %}

While Spark SQL has specific optimizations for loading data from Apache Parquet
files, ADAM can be used to run Spark SQL queries against data stored in most
common genomics file formats, including SAM/BAM/CRAM, FASTQ, VCF/BCF, BED,
GTF/GFF3, IntervalList, NarrowPeak, FASTA and more.

Using ADAM through Python and R
===============================

As mentioned above, one of the major advantages of Spark SQL is that it provides
uniform query performance across Scala, Java, Python, and R. While ADAM is
mostly written in Scala, we have maintained Java APIs for a long time. However,
we have previously been unable to support Python or R APIs. Adding support
for Spark SQL eliminated the major issues that prevented us from adding Python
and R APIs. This release of ADAM introduces the `bdgenomics.adam` packages for
Python and R. Our Python API can be installed using `pip install
bdgenomics.adam`, and our R API is available from
[GitHub](https://github.com/bigdatagenomics/adam/releases/download/adam-parent-spark2_2.11-0.23.0/bdgenomics.adam_0.23.0.tar.gz).
We hope to make our R API available through CRAN in the 0.24.0 release of ADAM;
we are blocked on an issue upstream in Apache Spark and are tracking progress on
this issue at [ADAM-1851](https://github.com/bigdatagenomics/adam/issues/1851).

In addition to installing the `bdgenomics.adam` libraries, running `pip install
bdgenomics.adam` installs all of the ADAM command line tools:

{% codeblock %}
$ pip install bdgenomics.adam
...
Successfully installed bdgenomics.adam-0.23.0 py4j-0.10.4 pyspark-2.2.1

$ adam-submit

       e        888~-_         e            e    e
      d8b       888   \       d8b          d8b  d8b
     /Y88b      888    |     /Y88b        d888bdY88b
    /  Y88b     888    |    /  Y88b      / Y88Y Y888b
   /____Y88b    888   /    /____Y88b    /   YY   Y888b
  /      Y88b   888_-~    /      Y88b  /          Y888b

Usage: adam-submit [<spark-args> --] <adam-args>

Choose one of the following commands:

ADAM ACTIONS
          countKmers : Counts the k-mers/q-mers from a read dataset.
    countContigKmers : Counts the k-mers/q-mers from a read dataset.
 transformAlignments : Convert SAM/BAM to ADAM format and optionally perform read pre-processing transformations
   transformFeatures : Convert a file with sequence features into corresponding ADAM format and vice versa
  transformGenotypes : Convert a file with genotypes into corresponding ADAM format and vice versa
   transformVariants : Convert a file with variants into corresponding ADAM format and vice versa
         mergeShards : Merges the shards of a file
      reads2coverage : Calculate the coverage from a given ADAM file

CONVERSION OPERATIONS
          fasta2adam : Converts a text FASTA sequence file into an ADAMNucleotideContig Parquet file which represents assembled sequences.
          adam2fasta : Convert ADAM nucleotide contig fragments to FASTA files
          adam2fastq : Convert BAM to FASTQ files
  transformFragments : Convert alignment records into fragment records.

PRINT
               print : Print an ADAM formatted file
            flagstat : Print statistics on reads in an ADAM file (similar to samtools flagstat)
                view : View certain reads from an alignment-record file.


$ adam-shell 

Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.2.1
      /_/
         
Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_152)

scala> import org.bdgenomics.adam.rdd.ADAMContext._
import org.bdgenomics.adam.rdd.ADAMContext._

scala> :quit
{% endcodeblock %}

Most of the major APIs in ADAM can be used through our Python and R bindings,
with the exception of the region join API. We plan to enable the use of the
region join API in Python and R in the 0.24.0 release of ADAM, along with other
API compatibility improvements.


Changes since Previous Release
===============================

The full list of changes since version 0.22.0 is below.

<!-- more -->

**Closed issues:**

 - Readthedocs build error [\#1854](https://github.com/bigdatagenomics/adam/issues/1854)
 - Add pip release to release scripts [\#1847](https://github.com/bigdatagenomics/adam/issues/1847)
 - Publish scaladoc script still attempts to build markdown docs [\#1845](https://github.com/bigdatagenomics/adam/issues/1845)
 - Allow variant annotations to be loaded into genotypes [\#1838](https://github.com/bigdatagenomics/adam/issues/1838)
 - Specify correct extensions for SAM/BAM output [\#1834](https://github.com/bigdatagenomics/adam/issues/1834)
 - Fix link anchors and other issues in readthedocs [\#1822](https://github.com/bigdatagenomics/adam/issues/1822)
 - Sphinx fulltoc is not included [\#1821](https://github.com/bigdatagenomics/adam/issues/1821)
 - Readme link to bigdatagenomics/lime 404s [\#1819](https://github.com/bigdatagenomics/adam/issues/1819)
 - Bump to Hadoop-BAM 7.9.1 [\#1817](https://github.com/bigdatagenomics/adam/issues/1817)
 - LoadVariants Header Format [\#1815](https://github.com/bigdatagenomics/adam/issues/1815)
 - Right and Left Outer Shuffle Region Join don't match [\#1813](https://github.com/bigdatagenomics/adam/issues/1813)
 - Pipe command can fail with empty partitions [\#1807](https://github.com/bigdatagenomics/adam/issues/1807)
 - adam files with outdated formats throw FileNotFoundException [\#1804](https://github.com/bigdatagenomics/adam/issues/1804)
 - Move GenomicRDD.writeTextRDD outside of GenomicRDD [\#1803](https://github.com/bigdatagenomics/adam/issues/1803)
 - find-adam-assembly fails to recognize more than 1 jar [\#1801](https://github.com/bigdatagenomics/adam/issues/1801)
 - tests/testthat.R failed on git head [\#1799](https://github.com/bigdatagenomics/adam/issues/1799)
 - Run python and R tests conditionally in build [\#1795](https://github.com/bigdatagenomics/adam/issues/1795)
 - scala-lang should be a provided dependency [\#1789](https://github.com/bigdatagenomics/adam/issues/1789)
 - loadIndexedBam does an unnecessary union [\#1784](https://github.com/bigdatagenomics/adam/issues/1784)
 - Release bdgenomics.adam R package on CRAN [\#1783](https://github.com/bigdatagenomics/adam/issues/1783)
 - Issue with transformVariant // Adam to vcf [\#1782](https://github.com/bigdatagenomics/adam/issues/1782)
 - Add code of conduct [\#1779](https://github.com/bigdatagenomics/adam/issues/1779)
 - Reinstantiation of SQLContext in pyadam ADAMContext [\#1774](https://github.com/bigdatagenomics/adam/issues/1774)
 - Genotypes should only contain the core variant fields [\#1770](https://github.com/bigdatagenomics/adam/issues/1770)
 - Add SingleFASTQInFormatter [\#1768](https://github.com/bigdatagenomics/adam/issues/1768)
 - INDEL realigner can emit negative partition IDs [\#1763](https://github.com/bigdatagenomics/adam/issues/1763)
 - Request for a new release [\#1762](https://github.com/bigdatagenomics/adam/issues/1762)
 - INDEL realigner generates targets for reads with more than 1 INDEL [\#1753](https://github.com/bigdatagenomics/adam/issues/1753)
 - Fragment Issue [\#1752](https://github.com/bigdatagenomics/adam/issues/1752)
 - Variant Caller!!! [\#1751](https://github.com/bigdatagenomics/adam/issues/1751)
 - Spark Version!! [\#1750](https://github.com/bigdatagenomics/adam/issues/1750)
 - ReferenceRegion.subtract eliminating valid regions [\#1747](https://github.com/bigdatagenomics/adam/issues/1747)
 - New Shuffle Join Implementation - Left Outer + Group By Left [\#1745](https://github.com/bigdatagenomics/adam/issues/1745)
 - command failure after build success [\#1744](https://github.com/bigdatagenomics/adam/issues/1744)
 - Recalibrate_base_Qualities [\#1743](https://github.com/bigdatagenomics/adam/issues/1743)
 - Standardize regionFn for ShuffleJoin returned objects [\#1740](https://github.com/bigdatagenomics/adam/issues/1740)
 - Shuffle, Broadcast Joins with threshold [\#1739](https://github.com/bigdatagenomics/adam/issues/1739)
 - Adam on Spark 2.1 [\#1738](https://github.com/bigdatagenomics/adam/issues/1738)
 - Opening up permission on GenericGenomicRDD constructor [\#1735](https://github.com/bigdatagenomics/adam/issues/1735)
 - Consistency on ShuffleRegionJoin returns [\#1734](https://github.com/bigdatagenomics/adam/issues/1734)
 - vcf2adam support [\#1731](https://github.com/bigdatagenomics/adam/issues/1731)
 - Cloud-scale BWA MEM [\#1730](https://github.com/bigdatagenomics/adam/issues/1730)
 - Aligned Human Genome couldn't convert to Adam  [\#1729](https://github.com/bigdatagenomics/adam/issues/1729)
 - Mark Duplicates [\#1726](https://github.com/bigdatagenomics/adam/issues/1726)
 - Genomics Pipeline [\#1724](https://github.com/bigdatagenomics/adam/issues/1724)
 - .fastq Alignment  [\#1723](https://github.com/bigdatagenomics/adam/issues/1723)
 - Is it correct Adam file [\#1720](https://github.com/bigdatagenomics/adam/issues/1720)
 - .fastQ to .adam [\#1718](https://github.com/bigdatagenomics/adam/issues/1718)
 - Unable to create .adam from .sam [\#1717](https://github.com/bigdatagenomics/adam/issues/1717)
 - Add adam- prefix to distribution module name [\#1716](https://github.com/bigdatagenomics/adam/issues/1716)
 - Python load methods don't have ability to specify validation stringency [\#1715](https://github.com/bigdatagenomics/adam/issues/1715)
 - NPE when trying to map *loadVariants* over RDD [\#1713](https://github.com/bigdatagenomics/adam/issues/1713)
 - Add left normalization of INDELs as an RDD level primitive [\#1709](https://github.com/bigdatagenomics/adam/issues/1709)
 - Allow validation stringency to be set in AnySAMOutFormatter [\#1703](https://github.com/bigdatagenomics/adam/issues/1703)
 - InterleavedFastqInFormatter should sort by readInFragment [\#1702](https://github.com/bigdatagenomics/adam/issues/1702)
 - Allow silencing the # of reads in fragment warning in InterleavedFastqInFormatter [\#1701](https://github.com/bigdatagenomics/adam/issues/1701)
 - GenomicRDD.toXxx method names should be consistent [\#1699](https://github.com/bigdatagenomics/adam/issues/1699)
 - Exception thrown in VariantContextConverter.formatAllelicDepth despite SILENT validation stringency [\#1695](https://github.com/bigdatagenomics/adam/issues/1695)
 - Make GenomicRDD.toString more adam-shell friendly [\#1694](https://github.com/bigdatagenomics/adam/issues/1694)
 - Add adam-shell friendly VariantContextRDD.saveAsVcf method [\#1693](https://github.com/bigdatagenomics/adam/issues/1693)
 - change bdgenomics.adam package name for adam-python to bdg-adam [\#1691](https://github.com/bigdatagenomics/adam/issues/1691)
 - Conflict in bdg-formats dependency version due to org.hammerlab:genomic-loci [\#1688](https://github.com/bigdatagenomics/adam/issues/1688)
 - Convert and store variant quality field. [\#1682](https://github.com/bigdatagenomics/adam/issues/1682)
 - Region join shows non-determinism [\#1680](https://github.com/bigdatagenomics/adam/issues/1680)
 - Shuffle region join throws multimapped exception for unmapped reads [\#1679](https://github.com/bigdatagenomics/adam/issues/1679)
 - Push validation checks down to INFO/FORMAT fields [\#1676](https://github.com/bigdatagenomics/adam/issues/1676)
 - IndexOutOfBounds thrown when saving gVCF with no likelihoods [\#1673](https://github.com/bigdatagenomics/adam/issues/1673)
 - Generate docs from R API for distribution [\#1672](https://github.com/bigdatagenomics/adam/issues/1672)
 - Support loading a subset of VCF fields [\#1670](https://github.com/bigdatagenomics/adam/issues/1670)
 - Error with metadata: Multivalued flags are not supported for INFO lines [\#1669](https://github.com/bigdatagenomics/adam/issues/1669)
 - Include bdg.adam-0.23.0.tar.gz in distribution tarballs [\#1668](https://github.com/bigdatagenomics/adam/issues/1668)
 - Include bdgenomics.adam-0.23.0_SNAPSHOT-py2.7.egg in distribution tarball [\#1667](https://github.com/bigdatagenomics/adam/issues/1667)
 - Add SUPPORT.md file to complement CONTRIBUTING.md [\#1664](https://github.com/bigdatagenomics/adam/issues/1664)
 - Can't merge BAM files containing the same sample [\#1663](https://github.com/bigdatagenomics/adam/issues/1663)
 - Incorrect README.md  kmer.scala loadAliments method parameter name [\#1662](https://github.com/bigdatagenomics/adam/issues/1662)
 - Add performance benchmarks similar to Samtools CRAM benchmarking page [\#1661](https://github.com/bigdatagenomics/adam/issues/1661)
 - Transient bad GZIP header bug when loading BGZF FASTQ [\#1658](https://github.com/bigdatagenomics/adam/issues/1658)
 - bdgenomics.adam vs bdg.adam for R/Python APIs [\#1655](https://github.com/bigdatagenomics/adam/issues/1655)
 - Need adamR script [\#1649](https://github.com/bigdatagenomics/adam/issues/1649)
 - incorrect grep for assembly jars in bin/pyadam [\#1647](https://github.com/bigdatagenomics/adam/issues/1647)
 - VariantRDD union creates multiple records for the same SNP ID [\#1644](https://github.com/bigdatagenomics/adam/issues/1644)
 - S3 access documentation [\#1643](https://github.com/bigdatagenomics/adam/issues/1643)
 - Algorithms docs formatting [\#1639](https://github.com/bigdatagenomics/adam/issues/1639)
 - Building downstream apps docs reformatting [\#1638](https://github.com/bigdatagenomics/adam/issues/1638)
 - FastqInputFormat.FILE_SPLITTABLE in conf not getting passed properly [\#1635](https://github.com/bigdatagenomics/adam/issues/1635)
 - Add benchmarks to documentation [\#1634](https://github.com/bigdatagenomics/adam/issues/1634)
 - Intro docs contain outdated/incompatible code [\#1633](https://github.com/bigdatagenomics/adam/issues/1633)
 - Intro docs missing a number of active projects [\#1632](https://github.com/bigdatagenomics/adam/issues/1632)
 - Installation instructions for Homebrew missing from documentation [\#1631](https://github.com/bigdatagenomics/adam/issues/1631)
 - Architecture section is missing from docs [\#1630](https://github.com/bigdatagenomics/adam/issues/1630)
 - Seq<VCFCompoundHeaderLine> vs. Seq<VCFHeaderLine> with javac [\#1625](https://github.com/bigdatagenomics/adam/issues/1625)
 - ProcessingStep missing from adam-codegen [\#1623](https://github.com/bigdatagenomics/adam/issues/1623)
 - Add ADAM recipe to bioconda [\#1618](https://github.com/bigdatagenomics/adam/issues/1618)
 - adam-submit cannot find assembly jar if installed as symlink [\#1616](https://github.com/bigdatagenomics/adam/issues/1616)
 - Expose transform/transmute in Java/Python/R [\#1615](https://github.com/bigdatagenomics/adam/issues/1615)
 - Expose VariantContextRDD in R/Python [\#1614](https://github.com/bigdatagenomics/adam/issues/1614)
 - Expose pipe API from Python/R [\#1611](https://github.com/bigdatagenomics/adam/issues/1611)
 - Serialization issue with TwoBitFile [\#1610](https://github.com/bigdatagenomics/adam/issues/1610)
 - Snapshot Distribution Does not include jar files [\#1607](https://github.com/bigdatagenomics/adam/issues/1607)
 - ManualRegionPartitioner is broken for ParallelFileMerger codepath [\#1602](https://github.com/bigdatagenomics/adam/issues/1602)
 - VariantRDD doesn't save partition map [\#1601](https://github.com/bigdatagenomics/adam/issues/1601)
 - Scala copy method not supported in abstract classes such as AlignmentRecordRDD [\#1599](https://github.com/bigdatagenomics/adam/issues/1599)
 - Interleaved FASTQ recognizes only /1 suffix pattern [\#1589](https://github.com/bigdatagenomics/adam/issues/1589)
 - Use empty sequence dictionary when loading features [\#1588](https://github.com/bigdatagenomics/adam/issues/1588)
 - New Illumina FASTQ spec adds metadata to read name line [\#1585](https://github.com/bigdatagenomics/adam/issues/1585)
 - first run of ADAM [\#1582](https://github.com/bigdatagenomics/adam/issues/1582)
 - Add unit test coverage for BED12 parser and writer [\#1579](https://github.com/bigdatagenomics/adam/issues/1579)
 - Spark 1.x Scala 2.10 snapshot artifacts missing since 31 March 2017 [\#1578](https://github.com/bigdatagenomics/adam/issues/1578)
 - Unable to save GenomicRDDs after a join. [\#1576](https://github.com/bigdatagenomics/adam/issues/1576)
 - Add filterBySequenceDictionary to GenomicRDD [\#1575](https://github.com/bigdatagenomics/adam/issues/1575)
 - Unaligned Trait does nothing [\#1573](https://github.com/bigdatagenomics/adam/issues/1573)
 - Bump to bdg-formats 0.11.1 [\#1570](https://github.com/bigdatagenomics/adam/issues/1570)
 - PhredUtils conversion to log probabilities has insufficient resolution for PLs [\#1569](https://github.com/bigdatagenomics/adam/issues/1569)
 - Reference model import code is borked [\#1568](https://github.com/bigdatagenomics/adam/issues/1568)
 - SequenceDictionary vs Feature[RDD] of reference length features [\#1567](https://github.com/bigdatagenomics/adam/issues/1567)
 - giab-NA12878 truth_small_variants.vcf.gz header issues [\#1566](https://github.com/bigdatagenomics/adam/issues/1566)
 - VCF header read from stream ignored in VCFOutFormatter [\#1564](https://github.com/bigdatagenomics/adam/issues/1564)
 - VCF genotype Number=A attribute throws ArrayIndexOutOfBoundsException [\#1562](https://github.com/bigdatagenomics/adam/issues/1562)
 - Save compressed single file VCF via HadoopBAM [\#1554](https://github.com/bigdatagenomics/adam/issues/1554)
 - bucketing strategy [\#1553](https://github.com/bigdatagenomics/adam/issues/1553)
 - Is parquet using delta encoding for positions? [\#1552](https://github.com/bigdatagenomics/adam/issues/1552)
 - Export to VCF does not include symbolic non-ref if site has a called alt [\#1551](https://github.com/bigdatagenomics/adam/issues/1551)
 - Refactor filterByOverlappingRegions not to require a List [\#1549](https://github.com/bigdatagenomics/adam/issues/1549)
 - Move docs to Sphinx/pure Markdown [\#1548](https://github.com/bigdatagenomics/adam/issues/1548)
 - java.lang.IncompatibleClassChangeError: Implementing class [\#1544](https://github.com/bigdatagenomics/adam/issues/1544)
 - Support locus predicate in `TransformAlignments` [\#1539](https://github.com/bigdatagenomics/adam/issues/1539)
 - Visibility from Java, jrdd has private access in AvroGenomicRDD [\#1538](https://github.com/bigdatagenomics/adam/issues/1538)
 - Rename o.b.adam.apis.java package to o.b.adam.api.java [\#1537](https://github.com/bigdatagenomics/adam/issues/1537)
 - VCF header genotype reserved key FT cardinality clobbered by htsjdk [\#1535](https://github.com/bigdatagenomics/adam/issues/1535)
 - Compute a SequenceDictionary from a *.genome file [\#1534](https://github.com/bigdatagenomics/adam/issues/1534)
 - Queryname sorted check should check for queryname grouped as well [\#1530](https://github.com/bigdatagenomics/adam/issues/1530)
 - Bump to bdg-formats 0.11.0 [\#1520](https://github.com/bigdatagenomics/adam/issues/1520)
 - Move to Spark 2.2, Parquet 1.8.2 [\#1517](https://github.com/bigdatagenomics/adam/issues/1517)
 - Minor refactor for TreeRegionJoin for consistency [\#1514](https://github.com/bigdatagenomics/adam/issues/1514)
 - Allow +Inf and -Inf Float values when reading VCF [\#1512](https://github.com/bigdatagenomics/adam/issues/1512)
 - SparkFiles temp directory path should be accessible as a variable [\#1510](https://github.com/bigdatagenomics/adam/issues/1510)
 - SparkFiles.get expects just the filename [\#1509](https://github.com/bigdatagenomics/adam/issues/1509)
 - Split apart #1324 [\#1507](https://github.com/bigdatagenomics/adam/issues/1507)
 - Where can I find "Phred-scaled quality score" (QUAL)? [\#1506](https://github.com/bigdatagenomics/adam/issues/1506)
 - Alignment Record sort is not consistent with samtools [\#1504](https://github.com/bigdatagenomics/adam/issues/1504)
 - Sequence dictionary records in TwoBitFile are not stable [\#1502](https://github.com/bigdatagenomics/adam/issues/1502)
 - Move coverage counter over to Dataset API [\#1501](https://github.com/bigdatagenomics/adam/issues/1501)
 - Allow users to set the minimum partition count across all load methods [\#1500](https://github.com/bigdatagenomics/adam/issues/1500)
 - Enable reuse of broadcast object across broadcast region joins [\#1499](https://github.com/bigdatagenomics/adam/issues/1499)
 - Take union across genomic RDDs [\#1497](https://github.com/bigdatagenomics/adam/issues/1497)
 - Adam files created by vcf2adam is not recognizable [\#1496](https://github.com/bigdatagenomics/adam/issues/1496)
 - Scalatest log output disappears with Maven 3.5.0 [\#1495](https://github.com/bigdatagenomics/adam/issues/1495)
 - ArrayOutOfBoundsException in vcf2adam (spark2_2.11-0.22.0) on UK10K VCFs (VCFv4.1) [\#1494](https://github.com/bigdatagenomics/adam/issues/1494)
 - ReferenceRegion overlaps and covers returns false if overlap is 1 [\#1492](https://github.com/bigdatagenomics/adam/issues/1492)
 - Provide asSingleFile parameter for saveAsFastq and related [\#1490](https://github.com/bigdatagenomics/adam/issues/1490)
 - Min Phred score gets bumped by 33 twice in BQSR [\#1488](https://github.com/bigdatagenomics/adam/issues/1488)
 - Should throw error when BAM header load fails [\#1486](https://github.com/bigdatagenomics/adam/issues/1486)
 - Default value for reads.toCoverage(collapse) should be false [\#1483](https://github.com/bigdatagenomics/adam/issues/1483)
 - Refactor ADAMContext loadXxx methods for consistency [\#1481](https://github.com/bigdatagenomics/adam/issues/1481)
 - loadGenotypes three time [\#1480](https://github.com/bigdatagenomics/adam/issues/1480)
 - Fall back to sequential concat when HDFS concat fails [\#1478](https://github.com/bigdatagenomics/adam/issues/1478)
 - VCF line with `.` ALT gets dropped [\#1476](https://github.com/bigdatagenomics/adam/issues/1476)
 - ADAM works on Cloudera but does NOT work on MAPR [\#1475](https://github.com/bigdatagenomics/adam/issues/1475)
 - Clean up ReferenceRegion.scala [\#1474](https://github.com/bigdatagenomics/adam/issues/1474)
 - Allow joins on regions that are within a threshold (instead of requiring overlap) [\#1473](https://github.com/bigdatagenomics/adam/issues/1473)
 - FeatureRDD.toCoverage throws NullPointerException when there is no coverage information [\#1471](https://github.com/bigdatagenomics/adam/issues/1471)
 - Add quality score binner [\#1462](https://github.com/bigdatagenomics/adam/issues/1462)
 - Splittable compression and FASTQ [\#1457](https://github.com/bigdatagenomics/adam/issues/1457)
 - Don't convert .{different-type}.adam in loadAlignments and loadFragments [\#1456](https://github.com/bigdatagenomics/adam/issues/1456)
 - New primitives for adam-core [\#1454](https://github.com/bigdatagenomics/adam/issues/1454)
 - Port over code for populating SequenceDictionaries from .dict files [\#1449](https://github.com/bigdatagenomics/adam/issues/1449)
 - Ignore failed push to Coveralls during CI builds [\#1444](https://github.com/bigdatagenomics/adam/issues/1444)
 - No asSingleFile parameter for saveAsFasta in NucleotideContigFragmentRDD [\#1438](https://github.com/bigdatagenomics/adam/issues/1438)
 - shufflejoin and ArrayIndexOutOfBoundsException [\#1436](https://github.com/bigdatagenomics/adam/issues/1436)
 - Document using ADAM snapshot [\#1432](https://github.com/bigdatagenomics/adam/issues/1432)
 - Improve metrics coverage across ADAMContext load methods [\#1428](https://github.com/bigdatagenomics/adam/issues/1428)
 - loadReferenceFile missing from Java API [\#1421](https://github.com/bigdatagenomics/adam/issues/1421)
 - loadCoverage missing from Java API [\#1420](https://github.com/bigdatagenomics/adam/issues/1420)
 - Question: How to get paired-end alignemntRecord like RDD[AlignmentRecord, AlignmentRecordRDD]? [\#1419](https://github.com/bigdatagenomics/adam/issues/1419)
 - Clean up possibly unused methods in Projection [\#1417](https://github.com/bigdatagenomics/adam/issues/1417)
 - Problem loading SNPeff annotated VCF [\#1390](https://github.com/bigdatagenomics/adam/issues/1390)
 - RecordGroupDictionary should support `isEmpty` [\#1380](https://github.com/bigdatagenomics/adam/issues/1380)
 - Get rid of mutable collection transformations in ShuffleRegionJoin [\#1379](https://github.com/bigdatagenomics/adam/issues/1379)
 - Add tab5/6 as native output format for AlignmentRecordRDD [\#1377](https://github.com/bigdatagenomics/adam/issues/1377)
 - ValidationStringency in MDTagging should apply to reads on unknown references [\#1365](https://github.com/bigdatagenomics/adam/issues/1365)
 - Assembly final name doesn't include spark2 for Spark 2.x builds [\#1361](https://github.com/bigdatagenomics/adam/issues/1361)
 - Merge reads2fragments and fragments2reads into a single CLI [\#1359](https://github.com/bigdatagenomics/adam/issues/1359)
 - Investigate failures to load ExAC.0.3.GRCh38.vcf variants [\#1351](https://github.com/bigdatagenomics/adam/issues/1351)
 - adam-shell does not allow additional jars via Spark jars argument [\#1349](https://github.com/bigdatagenomics/adam/issues/1349)
 - Loading GZipped VCF returns an empty RDD [\#1333](https://github.com/bigdatagenomics/adam/issues/1333)
 - Bump Spark 2 build to Spark 2.1.0 [\#1330](https://github.com/bigdatagenomics/adam/issues/1330)
 - Rename Transform command TransformAlignments or similar [\#1328](https://github.com/bigdatagenomics/adam/issues/1328)
 - Replace ADAM2Vcf and Vcf2ADAM commands with TransformGenotypes and TransformVariants [\#1327](https://github.com/bigdatagenomics/adam/issues/1327)
 - FeatureRDD instantiation tries to cache the RDD [\#1321](https://github.com/bigdatagenomics/adam/issues/1321)
 - Repository for Pipe API wrappers for bioinformatics tools [\#1314](https://github.com/bigdatagenomics/adam/issues/1314)
 - Trying to get Spark pipeline working with slightly out of date code. [\#1313](https://github.com/bigdatagenomics/adam/issues/1313)
 - Support for gVCF merging and genotyping (e.g. CombineGVCFs and GenotypeGVCFs) [\#1312](https://github.com/bigdatagenomics/adam/issues/1312)
 - Support for read alignment and variant calling in Adam? (e.g. BWA + Freebayes) [\#1311](https://github.com/bigdatagenomics/adam/issues/1311)
 - Don't include log4j.properties in published JAR [\#1300](https://github.com/bigdatagenomics/adam/issues/1300)
 - Removing ProgramRecords info when saving data to sam/bam? [\#1257](https://github.com/bigdatagenomics/adam/issues/1257)
 - ADAM on Slurm/LSF [\#1229](https://github.com/bigdatagenomics/adam/issues/1229)
 - Maintaining sorted/partitioned knowledge [\#1216](https://github.com/bigdatagenomics/adam/issues/1216)
 - Evaluate bdg-convert external conversion library proposal [\#1197](https://github.com/bigdatagenomics/adam/issues/1197)
 - Port AMPCamp Tutorial over [\#1174](https://github.com/bigdatagenomics/adam/issues/1174)
 - Top level WrappedRDD or similar abstraction [\#1173](https://github.com/bigdatagenomics/adam/issues/1173)
 - GFF3 formatted features written as single file must include gff-version pragma [\#1169](https://github.com/bigdatagenomics/adam/issues/1169)
 - Can probably eliminate sort in RealignIndels [\#1137](https://github.com/bigdatagenomics/adam/issues/1137)
 - Load SV type info field - need for allele uniquness [\#1134](https://github.com/bigdatagenomics/adam/issues/1134)
 - BroadcastRegionJoin is not a broadcast join [\#1110](https://github.com/bigdatagenomics/adam/issues/1110)
 - AlignmentRecordRDD does not extend GenomicRDD per javac [\#1092](https://github.com/bigdatagenomics/adam/issues/1092)
 - Add generic ReferenceRegion pushdown for parquet files [\#1047](https://github.com/bigdatagenomics/adam/issues/1047)
 - Use of dataset api in ADAM [\#1018](https://github.com/bigdatagenomics/adam/issues/1018)
 - Difference running markdups with and without projection [\#1014](https://github.com/bigdatagenomics/adam/issues/1014)
 - ADAM to BAM conversion fails using relative path [\#1012](https://github.com/bigdatagenomics/adam/issues/1012)
 - Refactor SequenceDictionary to use Contig instead of SequenceRecord [\#997](https://github.com/bigdatagenomics/adam/issues/997)
 - NoSuchMethodError due to kryo minor-version mismatch [\#955](https://github.com/bigdatagenomics/adam/issues/955)
 - Autogen field names in projection package [\#941](https://github.com/bigdatagenomics/adam/issues/941)
 - Future of schemas in bdg-formats [\#925](https://github.com/bigdatagenomics/adam/issues/925)
 - genotypeType for genotypes with multiple OtherAlt alleles? [\#897](https://github.com/bigdatagenomics/adam/issues/897)
 - How to filter genotype RDD with FeatureRDD [\#890](https://github.com/bigdatagenomics/adam/issues/890)
 - How to convert genotype DataFrame to VariantContext DataFrame / RDD [\#886](https://github.com/bigdatagenomics/adam/issues/886)
 - R language package for Adam [\#882](https://github.com/bigdatagenomics/adam/issues/882)
 - How to count genotypes with a 10 node Spark/Adam cluster faster than with BCFTools on a single machine? [\#879](https://github.com/bigdatagenomics/adam/issues/879)
 - Ensure Java API is up-to-date with Scala API [\#855](https://github.com/bigdatagenomics/adam/issues/855)
 - BroadcastRegionJoin fails with unmapped reads [\#821](https://github.com/bigdatagenomics/adam/issues/821)
 - Resolve Fragment vs. SingleReadBucket [\#789](https://github.com/bigdatagenomics/adam/issues/789)
 - Updating/Publishing the docs/ directory [\#774](https://github.com/bigdatagenomics/adam/issues/774)
 - Next on empty iterator in BroadcastRegionJoin [\#661](https://github.com/bigdatagenomics/adam/issues/661)
 - Cleanup code smell in sort work balancing code [\#635](https://github.com/bigdatagenomics/adam/issues/635)
 - Provide low-impact alternative to `transform -repartition` for reducing partition size [\#594](https://github.com/bigdatagenomics/adam/issues/594)
 - Create an ADAM Python API [\#538](https://github.com/bigdatagenomics/adam/issues/538)
 - Migrate serialization libraries out of ADAM core [\#448](https://github.com/bigdatagenomics/adam/issues/448)
 - Create standardized, interpretable exceptions for error reporting [\#420](https://github.com/bigdatagenomics/adam/issues/420)
 - Build info/version info inside ADAM-generated files [\#188](https://github.com/bigdatagenomics/adam/issues/188)

**Merged and closed pull requests:**

 - [ADAM-1854] Add requirements.txt file for RTD. [\#1856](https://github.com/bigdatagenomics/adam/pull/1856) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1783] Resolve check issues that block pushing to CRAN. [\#1849](https://github.com/bigdatagenomics/adam/pull/1849) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1847] Update ADAM scripts to support self-contained pip install. [\#1848](https://github.com/bigdatagenomics/adam/pull/1848) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1845] Only build and publish scaladocs in publish-scaladoc.sh. [\#1846](https://github.com/bigdatagenomics/adam/pull/1846) ([heuermh](https://github.com/heuermh))
 - [ADAM-1843] Install sources before calling scala:doc in publish scaladoc [\#1844](https://github.com/bigdatagenomics/adam/pull/1844) ([fnothaft](https://github.com/fnothaft))
 - Remove python and R profiles from release script [\#1842](https://github.com/bigdatagenomics/adam/pull/1842) ([heuermh](https://github.com/heuermh))
 - [ADAM-1817] Bump to Hadoop-BAM 7.9.1. [\#1841](https://github.com/bigdatagenomics/adam/pull/1841) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1838] Make populating variant.annotation field in Genotype configurable [\#1839](https://github.com/bigdatagenomics/adam/pull/1839) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1834] Add proper extensions for SAM/BAM/CRAM output formats. [\#1835](https://github.com/bigdatagenomics/adam/pull/1835) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1822] Misc docs cleanup [\#1827](https://github.com/bigdatagenomics/adam/pull/1827) ([fnothaft](https://github.com/fnothaft))
 - Added missing __init__.py for fulltoc. [\#1824](https://github.com/bigdatagenomics/adam/pull/1824) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1821] Add missing fulltoc for Sphinx documentation. [\#1823](https://github.com/bigdatagenomics/adam/pull/1823) ([fnothaft](https://github.com/fnothaft))
 - Fix link to documentation [\#1820](https://github.com/bigdatagenomics/adam/pull/1820) ([nzachow](https://github.com/nzachow))
 - [ADAM-1634] Add algorithm benchmarks to documentation. [\#1818](https://github.com/bigdatagenomics/adam/pull/1818) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1813] Delegate right outer shuffle region join to left OSRJ implementation. [\#1814](https://github.com/bigdatagenomics/adam/pull/1814) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1807] Check for empty partition when running a piped command. [\#1812](https://github.com/bigdatagenomics/adam/pull/1812) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1803] Refactor GenomicRDD.writeTextRdd to util.TextRddWriter. [\#1809](https://github.com/bigdatagenomics/adam/pull/1809) ([heuermh](https://github.com/heuermh))
 - Added Filter error when file loaded does not match schema [\#1805](https://github.com/bigdatagenomics/adam/pull/1805) ([akmorrow13](https://github.com/akmorrow13))
 - changed num_jars count [\#1802](https://github.com/bigdatagenomics/adam/pull/1802) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1795] Map -DskipTests=true to exec.skip for Python and R tests. [\#1800](https://github.com/bigdatagenomics/adam/pull/1800) ([heuermh](https://github.com/heuermh))
 - [ADAM-1672] Use working directory for R devtools::document(). [\#1798](https://github.com/bigdatagenomics/adam/pull/1798) ([heuermh](https://github.com/heuermh))
 - [ADAM-1789] Move scala-lang to provided scope. [\#1790](https://github.com/bigdatagenomics/adam/pull/1790) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1784] loadIndexedBam should pass the raw globbed path to Hadoop-BAM [\#1785](https://github.com/bigdatagenomics/adam/pull/1785) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1664] Add SUPPORT.md file to complement CONTRIBUTING.md. [\#1781](https://github.com/bigdatagenomics/adam/pull/1781) ([heuermh](https://github.com/heuermh))
 - [ADAM-1779] Adding code of contact adapted from the Contributor Convenant, version 1.4. [\#1780](https://github.com/bigdatagenomics/adam/pull/1780) ([heuermh](https://github.com/heuermh))
 - [ADAM-1661] Add file storage benchmarks. [\#1772](https://github.com/bigdatagenomics/adam/pull/1772) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1770] Genotype should only store core variant fields. [\#1771](https://github.com/bigdatagenomics/adam/pull/1771) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1768] Add InFormatter for unpaired FASTQ. [\#1769](https://github.com/bigdatagenomics/adam/pull/1769) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1643] Add S3 access documentation. [\#1767](https://github.com/bigdatagenomics/adam/pull/1767) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1763] Apply absolute value to destination partition in ModPartitioner [\#1766](https://github.com/bigdatagenomics/adam/pull/1766) ([fnothaft](https://github.com/fnothaft))
 - Add R and Python into distribution artifacts [\#1765](https://github.com/bigdatagenomics/adam/pull/1765) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1655] Move R package to bdgenomics.adam. [\#1764](https://github.com/bigdatagenomics/adam/pull/1764) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1753] Only emit realignment targets for reads containing a single INDEL [\#1756](https://github.com/bigdatagenomics/adam/pull/1756) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1715] Support validation stringency in Python/R. [\#1755](https://github.com/bigdatagenomics/adam/pull/1755) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1680] Eliminate non-determinism in the ShuffleRegionJoin. [\#1754](https://github.com/bigdatagenomics/adam/pull/1754) ([fnothaft](https://github.com/fnothaft))
 - update to _replaceRdd with tests [\#1749](https://github.com/bigdatagenomics/adam/pull/1749) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1747] Fixed subtract bug and tests [\#1748](https://github.com/bigdatagenomics/adam/pull/1748) ([devin-petersohn](https://github.com/devin-petersohn))
 - [ADAM-1745] Adding LeftOuterShuffleRegionJoinAndGroupByLeft and tests [\#1746](https://github.com/bigdatagenomics/adam/pull/1746) ([devin-petersohn](https://github.com/devin-petersohn))
 - Enabled thresholding for joins and standardized regionFn [\#1741](https://github.com/bigdatagenomics/adam/pull/1741) ([devin-petersohn](https://github.com/devin-petersohn))
 - Making join return types consistent [\#1737](https://github.com/bigdatagenomics/adam/pull/1737) ([devin-petersohn](https://github.com/devin-petersohn))
 - Opening up permissions on GenericGenomicRDD [\#1736](https://github.com/bigdatagenomics/adam/pull/1736) ([devin-petersohn](https://github.com/devin-petersohn))
 - [ADAM-1716] Add adam- prefix to distribution module name. [\#1733](https://github.com/bigdatagenomics/adam/pull/1733) ([heuermh](https://github.com/heuermh))
 - [ADAM-1695] Check for illegal genotype index after splitting multi-allelic variants. [\#1725](https://github.com/bigdatagenomics/adam/pull/1725) ([heuermh](https://github.com/heuermh))
 - [ADAM-1517] Bump Parquet version in a manner compatible with Spark 2.2.x [\#1722](https://github.com/bigdatagenomics/adam/pull/1722) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1512] Support VCFs with +Inf/-Inf float values. [\#1721](https://github.com/bigdatagenomics/adam/pull/1721) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1709] Add ability to left normalize reads containing INDELs. [\#1711](https://github.com/bigdatagenomics/adam/pull/1711) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1691] Move bdgenomics.adam to use a namespace package. [\#1706](https://github.com/bigdatagenomics/adam/pull/1706) ([fnothaft](https://github.com/fnothaft))
 - moved bdgenomics.adam package to bdgenomics-adam [\#1705](https://github.com/bigdatagenomics/adam/pull/1705) ([akmorrow13](https://github.com/akmorrow13))
 - Misc cleanup needed for bigdatagenomics/cannoli#65 [\#1704](https://github.com/bigdatagenomics/adam/pull/1704) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1699] Make GenomicRDD.toXxx method names consistent. [\#1700](https://github.com/bigdatagenomics/adam/pull/1700) ([heuermh](https://github.com/heuermh))
 - [ADAM-1694] Add short readable descriptions for toString in subclasses of GenomicRDD. [\#1698](https://github.com/bigdatagenomics/adam/pull/1698) ([heuermh](https://github.com/heuermh))
 - [ADAM-1693] Add adam-shell friendly VariantContextRDD.saveAsVcf method. [\#1696](https://github.com/bigdatagenomics/adam/pull/1696) ([heuermh](https://github.com/heuermh))
 - [ADAM-1688] Add bdg-formats exclusion to org.hammerlab:genomic-loci dependency. [\#1690](https://github.com/bigdatagenomics/adam/pull/1690) ([heuermh](https://github.com/heuermh))
 - [ADAM-1679] Unmapped items should not get caught in requirement when sorting [\#1687](https://github.com/bigdatagenomics/adam/pull/1687) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1566] Merge VCF header lines with VCFHeaderLineCount.INTEGER correctly. [\#1685](https://github.com/bigdatagenomics/adam/pull/1685) ([heuermh](https://github.com/heuermh))
 - [ADAM-1682] Add variant quality field. [\#1684](https://github.com/bigdatagenomics/adam/pull/1684) ([fnothaft](https://github.com/fnothaft))
 - Remove adam- prefix from module directory names. [\#1681](https://github.com/bigdatagenomics/adam/pull/1681) ([heuermh](https://github.com/heuermh))
 - Update to hadoop-bam 7.9.0 and htsjdk 2.11.0. [\#1678](https://github.com/bigdatagenomics/adam/pull/1678) ([heuermh](https://github.com/heuermh))
 - [ADAM-1676] Add more finely grained validation for INFO/FORMAT fields. [\#1677](https://github.com/bigdatagenomics/adam/pull/1677) ([fnothaft](https://github.com/fnothaft))
 - Python API fixes for AlignmentRecordRDD [\#1675](https://github.com/bigdatagenomics/adam/pull/1675) ([akmorrow13](https://github.com/akmorrow13))
 - [ADAM-1673] Don't set PL to empty when no PL is attached to a gVCF record [\#1674](https://github.com/bigdatagenomics/adam/pull/1674) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1670] Add ability to selectively project VCF fields. [\#1671](https://github.com/bigdatagenomics/adam/pull/1671) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1663] Enable read groups with repeated names when unioning. [\#1665](https://github.com/bigdatagenomics/adam/pull/1665) ([fnothaft](https://github.com/fnothaft))
 - Maint 2.11 0.18.0 [\#1659](https://github.com/bigdatagenomics/adam/pull/1659) ([Douglas-H](https://github.com/Douglas-H))
 - [ADAM-1630] Overhauled docs introduction and added architecture section. [\#1653](https://github.com/bigdatagenomics/adam/pull/1653) ([fnothaft](https://github.com/fnothaft))
 - Add adamR script [\#1651](https://github.com/bigdatagenomics/adam/pull/1651) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1647] Fix bad JAR discovery grep in bin/pyadam. [\#1648](https://github.com/bigdatagenomics/adam/pull/1648) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1548] Generate reStructuredText from pandoc markdown. [\#1646](https://github.com/bigdatagenomics/adam/pull/1646) ([fnothaft](https://github.com/fnothaft))
 - Algorithms docs formatting [\#1645](https://github.com/bigdatagenomics/adam/pull/1645) ([gunjanbaid](https://github.com/gunjanbaid))
 - Cleaned up docs. [\#1642](https://github.com/bigdatagenomics/adam/pull/1642) ([gunjanbaid](https://github.com/gunjanbaid))
 - Making example code compatible with current ADAM build [\#1641](https://github.com/bigdatagenomics/adam/pull/1641) ([devin-petersohn](https://github.com/devin-petersohn))
 - Cleaning up formatting and spacing of docs. [\#1640](https://github.com/bigdatagenomics/adam/pull/1640) ([devin-petersohn](https://github.com/devin-petersohn))
 - added ExtractRegions [\#1637](https://github.com/bigdatagenomics/adam/pull/1637) ([antonkulaga](https://github.com/antonkulaga))
 - [ADAM-1635] Eliminate passing FASTQ splittable status via config. [\#1636](https://github.com/bigdatagenomics/adam/pull/1636) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1614] Add VariantContextRDD to R and Python APIs. [\#1628](https://github.com/bigdatagenomics/adam/pull/1628) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1615] Add transform and transmute APIs to Java, R, and Python [\#1627](https://github.com/bigdatagenomics/adam/pull/1627) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1625] Use explicit types for header lines [\#1626](https://github.com/bigdatagenomics/adam/pull/1626) ([heuermh](https://github.com/heuermh))
 - [ADAM-1623] Add ProcessingStep to adam-codegen. [\#1624](https://github.com/bigdatagenomics/adam/pull/1624) ([heuermh](https://github.com/heuermh))
 - [ADAM-1607] Update distribution assembly task to attach assembly überjar [\#1622](https://github.com/bigdatagenomics/adam/pull/1622) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1490] Add asSingleFile to saveAsFastq and related. [\#1621](https://github.com/bigdatagenomics/adam/pull/1621) ([heuermh](https://github.com/heuermh))
 - Update load method docs in Python and R. [\#1619](https://github.com/bigdatagenomics/adam/pull/1619) ([heuermh](https://github.com/heuermh))
 - [ADAM-1616] Resolve installation directory if scripts are symlinks. [\#1617](https://github.com/bigdatagenomics/adam/pull/1617) ([heuermh](https://github.com/heuermh))
 - [ADAM-1611] Extend pipe APIs to Java, Python, and R. [\#1613](https://github.com/bigdatagenomics/adam/pull/1613) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1610] Mark non-serializable field in TwoBitFile as transient. [\#1612](https://github.com/bigdatagenomics/adam/pull/1612) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1554] Support saving BGZF VCF output. [\#1608](https://github.com/bigdatagenomics/adam/pull/1608) ([fnothaft](https://github.com/fnothaft))
 - Adding examples of how to use joins in the real world [\#1605](https://github.com/bigdatagenomics/adam/pull/1605) ([devin-petersohn](https://github.com/devin-petersohn))
 - [ADAM-1599] Add explicit functions for updating GenomicRDD metadata. [\#1600](https://github.com/bigdatagenomics/adam/pull/1600) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1576] Allow translation between two different GenomicRDD types. [\#1598](https://github.com/bigdatagenomics/adam/pull/1598) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1444] Ignore failed push to Coveralls. [\#1595](https://github.com/bigdatagenomics/adam/pull/1595) ([fnothaft](https://github.com/fnothaft))
 - Testing, testing, 1... 2... 3... [\#1592](https://github.com/bigdatagenomics/adam/pull/1592) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1417] Removed unused Projection.apply method, add test for Filter. [\#1591](https://github.com/bigdatagenomics/adam/pull/1591) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1579] Add unit test coverage for BED12 format. [\#1587](https://github.com/bigdatagenomics/adam/pull/1587) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1585] Support additional Illumina FASTQ metadata. [\#1586](https://github.com/bigdatagenomics/adam/pull/1586) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1438] Add ability to save FASTA back as a single file. [\#1581](https://github.com/bigdatagenomics/adam/pull/1581) ([fnothaft](https://github.com/fnothaft))
 - Bump bdg-formats correctly to 0.11.1, not SNAPSHOT. [\#1577](https://github.com/bigdatagenomics/adam/pull/1577) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1573] Remove unused Unaligned trait. [\#1574](https://github.com/bigdatagenomics/adam/pull/1574) ([fnothaft](https://github.com/fnothaft))
 - Slurm deployment readme [\#1571](https://github.com/bigdatagenomics/adam/pull/1571) ([jpdna](https://github.com/jpdna))
 - [ADAM-1564] Read VCF header from stream in VCFOutFormatter. [\#1565](https://github.com/bigdatagenomics/adam/pull/1565) ([heuermh](https://github.com/heuermh))
 - [ADAM-1562] Index off by one for VCF genotype Number=A attributes. [\#1563](https://github.com/bigdatagenomics/adam/pull/1563) ([heuermh](https://github.com/heuermh))
 - [ADAM-1533] Set Theory [\#1561](https://github.com/bigdatagenomics/adam/pull/1561) ([devin-petersohn](https://github.com/devin-petersohn))
 - Freebayes FORMAT=<ID=AO,Number=A attribute throws ArrayIndexOutOfBoundsException [\#1560](https://github.com/bigdatagenomics/adam/pull/1560) ([heuermh](https://github.com/heuermh))
 - [ADAM-1551] Emit non-reference model genotype at called sites. [\#1559](https://github.com/bigdatagenomics/adam/pull/1559) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1449] Add loadSequenceDictionary to ADAM context. [\#1557](https://github.com/bigdatagenomics/adam/pull/1557) ([heuermh](https://github.com/heuermh))
 - [ADAM-1537] Rename o.b.adam.apis.java package to o.b.adam.api.java [\#1556](https://github.com/bigdatagenomics/adam/pull/1556) ([heuermh](https://github.com/heuermh))
 - [ADAM-1549] Make regions provided to filterByOverlappingRegions an Iterable. [\#1550](https://github.com/bigdatagenomics/adam/pull/1550) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-941] Automatically generate projection enums.  [\#1547](https://github.com/bigdatagenomics/adam/pull/1547) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1361] Fix misnamed ADAM überjar. [\#1546](https://github.com/bigdatagenomics/adam/pull/1546) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1257] Add program record support for alignment/fragment files. [\#1545](https://github.com/bigdatagenomics/adam/pull/1545) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1359] Merge `reads2fragments` and `fragments2reads` into `transformFragments` [\#1543](https://github.com/bigdatagenomics/adam/pull/1543) ([fnothaft](https://github.com/fnothaft))
 - Fix minor format mistakes (and typo) in docs [\#1542](https://github.com/bigdatagenomics/adam/pull/1542) ([kkaneda](https://github.com/kkaneda))
 - Add a simple unit test to SingleFastqInputFormat [\#1541](https://github.com/bigdatagenomics/adam/pull/1541) ([kkaneda](https://github.com/kkaneda))
 - Support locus predicate in Transform [\#1540](https://github.com/bigdatagenomics/adam/pull/1540) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1421] Add java API for `loadReferenceFile`. [\#1536](https://github.com/bigdatagenomics/adam/pull/1536) ([fnothaft](https://github.com/fnothaft))
 - Refactor Vcf2ADAM and ADAM2Vcf into TransformGenotypes and TransformVariants [\#1532](https://github.com/bigdatagenomics/adam/pull/1532) ([heuermh](https://github.com/heuermh))
 - [ADAM-1530] Support loading GO:query (S/CR/B)AMs as fragments. [\#1531](https://github.com/bigdatagenomics/adam/pull/1531) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1169] Write GFF header line pragma in single file mode. [\#1529](https://github.com/bigdatagenomics/adam/pull/1529) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1501] Compute coverage using Dataset API. [\#1528](https://github.com/bigdatagenomics/adam/pull/1528) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1497] Add union to GenomicRDD. [\#1526](https://github.com/bigdatagenomics/adam/pull/1526) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1486] Respect validation stringency if BAM header load fails. [\#1525](https://github.com/bigdatagenomics/adam/pull/1525) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1499] Enable reuse of broadcasted objects in region join. [\#1524](https://github.com/bigdatagenomics/adam/pull/1524) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1520] Bump to bdg-formats 0.11.0. [\#1523](https://github.com/bigdatagenomics/adam/pull/1523) ([fnothaft](https://github.com/fnothaft))
 - Adding fragment InFormatter for Bowtie tab5 format [\#1522](https://github.com/bigdatagenomics/adam/pull/1522) ([heuermh](https://github.com/heuermh))
 - [ADAM-1328] Rename `Transform` to `TransformAlignments`. [\#1521](https://github.com/bigdatagenomics/adam/pull/1521) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1517] Move to Parquet 1.8.2 in preparation for moving to Spark 2.2.0 [\#1518](https://github.com/bigdatagenomics/adam/pull/1518) ([fnothaft](https://github.com/fnothaft))
 - Fixed minor typos in README. [\#1516](https://github.com/bigdatagenomics/adam/pull/1516) ([gunjanbaid](https://github.com/gunjanbaid))
 - Making TreeRegionJoin consistent with ShuffleRegionJoin [\#1515](https://github.com/bigdatagenomics/adam/pull/1515) ([devin-petersohn](https://github.com/devin-petersohn))
 - Resolve #1508, #1509 for Pipe API [\#1511](https://github.com/bigdatagenomics/adam/pull/1511) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1502] Preserve contig ordering in TwoBitFile sequence dictionary. [\#1508](https://github.com/bigdatagenomics/adam/pull/1508) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1483] Remove collapse parameter from AlignmentRecordRDD.toCoverage [\#1493](https://github.com/bigdatagenomics/adam/pull/1493) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1377] Adding fragment InFormatter for Bowtie tab6 format [\#1491](https://github.com/bigdatagenomics/adam/pull/1491) ([heuermh](https://github.com/heuermh))
 - [ADAM-1488] Only increment BQSR min quality by 33 once. [\#1489](https://github.com/bigdatagenomics/adam/pull/1489) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1481] Refactor ADAMContext loadXxx methods for consistency [\#1487](https://github.com/bigdatagenomics/adam/pull/1487) ([heuermh](https://github.com/heuermh))
 - Add quality score binner [\#1485](https://github.com/bigdatagenomics/adam/pull/1485) ([fnothaft](https://github.com/fnothaft))
 - Clean up ReferenceRegion.scala and add thresholded overlap and covers [\#1484](https://github.com/bigdatagenomics/adam/pull/1484) ([devin-petersohn](https://github.com/devin-petersohn))
 - [ADAM-1456] Remove .{type}.adam file extension conversions in type-guessing methods. [\#1482](https://github.com/bigdatagenomics/adam/pull/1482) ([heuermh](https://github.com/heuermh))
 - [ADAM-1480] Add switch to disable the fast concat method. [\#1479](https://github.com/bigdatagenomics/adam/pull/1479) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1476] Treat `.` ALT allele as symbolic non-ref. [\#1477](https://github.com/bigdatagenomics/adam/pull/1477) ([fnothaft](https://github.com/fnothaft))
 - Adding require for Coverage Conversion and related tests [\#1472](https://github.com/bigdatagenomics/adam/pull/1472) ([devin-petersohn](https://github.com/devin-petersohn))
 - Add cache argument to loadFeatures, additional Feature timers [\#1427](https://github.com/bigdatagenomics/adam/pull/1427) ([heuermh](https://github.com/heuermh))
 - [ADAM-882] R API [\#1397](https://github.com/bigdatagenomics/adam/pull/1397) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1018] Add support for Spark SQL Datasets. [\#1391](https://github.com/bigdatagenomics/adam/pull/1391) ([fnothaft](https://github.com/fnothaft))
 - WIP Python API [\#1387](https://github.com/bigdatagenomics/adam/pull/1387) ([fnothaft](https://github.com/fnothaft))
 - [ADAM-1365] Apply validation stringency to reads on missing contigs when MD tagging [\#1366](https://github.com/bigdatagenomics/adam/pull/1366) ([fnothaft](https://github.com/fnothaft))
 - Update dependency and plugin versions [\#1360](https://github.com/bigdatagenomics/adam/pull/1360) ([heuermh](https://github.com/heuermh))
 - [ADAM-1330] Move to Spark 2.1.0. [\#1332](https://github.com/bigdatagenomics/adam/pull/1332) ([fnothaft](https://github.com/fnothaft))
 - Efficient Joins and (re)Partitioning [\#1324](https://github.com/bigdatagenomics/adam/pull/1324) ([devin-petersohn](https://github.com/devin-petersohn))
