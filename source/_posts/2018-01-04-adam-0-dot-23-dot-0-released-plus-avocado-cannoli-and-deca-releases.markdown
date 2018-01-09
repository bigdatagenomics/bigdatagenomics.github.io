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
over 375 issues and pull requests merged since the last ADAM release. Some of
the highlights include:

* A validated, high-performance end-to-end alignment/variant calling pipeline
  using ADAM, Cannoli, and Avocado.
* Support for manipulating data using Spark SQL.
* R and Python APIs for ADAM, including the ability to get a working deployment
  of ADAM simply by running `pip install bdgenomics.adam`.

With this release, we have also moved our documentation to Read The Docs:

* [Read the Docs for ADAM](http://adam.readthedocs.io)
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
10--15 minutes when running on a 1,024 core cluster. We can couple this rapid
alignment pipeline with the fast preprocessing stages in ADAM and the variant
calling stages in Avocado to call variants on a 60x coverage WGS dataset in
approximately 45 minutes on a 1,024 core cluster. Avocado can be used to
call variants on a single sample, or to jointly call variants using a [gVCF-based
workflow](http://bdg-avocado.readthedocs.io/en/latest/workflows/joint.html). When
running on 1,024 cores, we were able to jointly genotype more than 10TB of gVCFs
within approximately 6 hours. Avovcado has >99% accuracy when genotyping SNPs,
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
marking, which provides an additional >20% performance improvement. We hope to
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
$ ./bin/adam-shell 
Using SPARK_SHELL=./bin/spark-shell
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Spark context Web UI available at http://10.8.5.208:4040
Spark context available as 'sc' (master = local[*], app id = local-1515449100368).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.2.0
      /_/
         
Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_152)
Type in expressions to have them evaluated.
Type :help for more information.

scala> import org.bdgenomics.adam.rdd.ADAMContext._
import org.bdgenomics.adam.rdd.ADAMContext._

scala> val reads = sc.loadAlignments("adam-core/src/test/resources/small.sam")
reads: org.bdgenomics.adam.rdd.read.AlignmentRecordRDD = RDDBoundAlignmentRecordRDD with 2 reference sequences, 0 read groups, and 2 processing steps

scala> reads.transformDataset(_.filter("readMapped=true")).dataset.show
18/01/08 14:06:55 WARN Utils: Truncated the string representation of a plan since it was too large. This behavior can be adjusted by setting 'spark.debug.maxToStringFields' in SparkEnv.conf.
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
Collecting bdgenomics.adam
  Downloading bdgenomics.adam-0.23.0.tar.gz (48.3MB)
    100% |████████████████████████████████| 48.4MB 33kB/s 
Collecting pyspark>=1.6.0 (from bdgenomics.adam)
  Downloading pyspark-2.2.1.tar.gz (188.2MB)
    100% |████████████████████████████████| 188.2MB 8.2kB/s 
Collecting py4j==0.10.4 (from pyspark>=1.6.0->bdgenomics.adam)
  Using cached py4j-0.10.4-py2.py3-none-any.whl
Building wheels for collected packages: bdgenomics.adam, pyspark
  Running setup.py bdist_wheel for bdgenomics.adam ... done
  Stored in directory: /Users/frank.nothaft/Library/Caches/pip/wheels/b3/2a/00/4f4676ffc3683c6ec3a4d90b58c6b3dc7fdaa49f6b7f31c1f4
  Running setup.py bdist_wheel for pyspark ... done
  Stored in directory: /Users/frank.nothaft/Library/Caches/pip/wheels/15/13/d2/79b478cd48d20956d136216574cbc38e35b4957d918127c26f
Successfully built bdgenomics.adam pyspark
Installing collected packages: py4j, pyspark, bdgenomics.adam
Successfully installed bdgenomics.adam-0.23.0 py4j-0.10.4 pyspark-2.2.1

$ adam-submit
Using ADAM_MAIN=org.bdgenomics.adam.cli.ADAMMain
Using spark-submit=/usr/local/bin/spark-2.2.1-bin-hadoop2.7/bin/spark-submit

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

Using SPARK_SHELL=/usr/local/bin/spark-2.2.1-bin-hadoop2.7/bin/spark-shell
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.2.1
      /_/
         
Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_152)
Type in expressions to have them evaluated.
Type :help for more information.

scala> import org.bdgenomics.adam.rdd.ADAMContext._
import org.bdgenomics.adam.rdd.ADAMContext._

scala> :quit
{% endcodeblock %}

Most of the major APIs in ADAM can be used through our R and Python bindings,
with the exception of the region join API. We plan to enable the use of the
region join API in Python and R in the 0.24.0 release of ADAM, along with other
API compatibility improvements.