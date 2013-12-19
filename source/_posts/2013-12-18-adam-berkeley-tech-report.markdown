---
layout: post
title: "ADAM Tech Report Released"
date: 2013-12-18 16:39:20 -0800
comments: true
categories: ADAM, Publications 
---

Current genomics data formats and processing pipelines are not designed to scale well to large datasets. 
The current Sequence/Binary Alignment/Map (SAM/BAM) formats were intended for single node processing. 
There have been attempts to adapt BAM to distributed computing environments, but they see limited scalability 
past eight nodes. Additionally, due to the lack of an explicit data schema, there are well known 
incompatibilities between libraries that implement SAM/BAM/Variant Call Format (VCF) data access. 

To address these 
problems, we introduce ADAM, a set of formats, APIs, and processing stage implementations for genomic data. 
ADAM is fully open source under the Apache 2 license, and is implemented on top of Avro and Parquet for data storage. 
Our reference pipeline is implemented on top of Spark, a high performance in-memory map-reduce system. This combination 
provides the following advantages: 

1. Avro provides explicit data schema access in C/C++/C#, Java/Scala, Python, php, and Ruby; 
2. Parquet allows access by database systems like Impala and Shark; and 
3. Spark improves performance through in-memory caching and reducing disk I/O. 

{% pullquote %}
In addition to improving the format’s cross-platform portability, these changes lead to significant performance improvements. 
On a single node, we are able to speedup sort and duplicate marking by 2×. More importantly, 
{"on a 250 Gigabyte (GB) high (60×) coverage human genome, this system achieves a 50× speedup"} 
on a 100 node computing cluster (see Table 1), fulfilling the promise of scalability of ADAM. 
{% endpullquote %}

You can read the full tech report at http://www.eecs.berkeley.edu/Pubs/TechRpts/2013/EECS-2013-207.html
