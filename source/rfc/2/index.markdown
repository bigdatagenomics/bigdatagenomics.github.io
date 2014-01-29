---
layout: page
title: "Genome annotation schema"
date: 2014-01-21 17:47
comments: true
sharing: true
footer: true
---


### Introduction

There exists a large amount of annotation data that overlays the human genome.
Many users want to ingest this data in order to join it against their database
of genome variants.  We want to generate a general schema that will cover the
vast majority of these annotations.  Example sources include ENCODE, UCSC genome
browser, dbSNP/COSMIC/etc.

Much of the data come in several common text formats (e.g., BED, GFF), reviewed
here: https://genome.ucsc.edu/FAQ/FAQformat.html


### Schema

Below is the proposed schema for genome annotations.  In addition to the schema,
there will be ADAM tools to ingest the most common formats.

```c
record ADAMAnnotation {
  // the name of this annotation/track (e.g., centipede, conservation, etc.)
  union { null, string } name = null;
  
  // reference identifiers
  union { null, int } referenceId = null;
  union { null, string } referenceName = null;
  union { null, long } referenceLength = null;
  union { null, string } referenceUrl = null;
  
  // position
  union { null, long } start = null;
  union { null, long } end = null;
  union { null, Strand} strand = null;
  
  // convenience information (possibly derived from name); for join convenience
  union { null, string } experiment = null;
  union { null, string } sample = null;
  
  // the actual observation
  // at most one of these should be non-null
  union { null, string} stringVal  = null;
  union { null, long} longVal = null;
  union { null, double} doubleVal = null;
}
```

### Some issues

1. Some annotations may be hierarchical. For example, ChIP-seq|sample1|TF2.  How
could you aggregate across the different "fields" in the annotation.  In
principle, these should all be in the `name`.

2. BED format allows for blocked annotations (e.g. exons).  If we ingest this
kind of data, should the solution be that each exon is its own annotation, with
an additional "grouping" field that represents the gene name?  Alternatively,
you could instead have a nested annotation object. (Are self-referential types
allowed?  The ADAMAnnotation could have an array of ADAMAnnotation) in it.

3. Or should we just scrap this general annotation type and have a specific type
for each of the popular formats (e.g., ADAMBED, ADAMGFF, etc.)?

4. Or do a Google-style monster annotation type that simply contains pretty much
any field anyone would ever want.  As ADAM evolves, we would simple continue to
add more and more fields.
