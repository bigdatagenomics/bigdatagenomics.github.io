---
layout: post
title: "ADAM 0.11.0 Released"
date: 2014-06-02 16:48:42 -0700
comments: true
categories: 
---

ADAM [0.11.0](https://github.com/bigdatagenomics/adam/releases/tag/adam-parent-0.11.0) is now available. 

This release allows you not just read but also write to SAM/BAM files, adds utilities for trimming reads,
implements contig-to-RefSeq translation, refactors SequenceDictionary to include RefSeq information (and without numeric IDs) 
and prepare ADAMGenotype for incorporating reference model information, and fixes a bug in FASTA fragments.

For details see the following issues...

* ISSUE [250](https://github.com/bigdatagenomics/adam/pull/250): Adding ADAM to SAM conversion.
* ISSUE [248](https://github.com/bigdatagenomics/adam/pull/248): Adding utilities for read trimming.
* ISSUE [252](https://github.com/bigdatagenomics/adam/pull/252): Added a note about rebasing-off-master to CONTRIBUTING.md
* ISSUE [249](https://github.com/bigdatagenomics/adam/pull/249): Cosmetic changes to FastaConverter and FastaConverterSuite.
* ISSUE [251](https://github.com/bigdatagenomics/adam/pull/251): CHANGES.md is updated at release instead of per pull request
* ISSUE [247](https://github.com/bigdatagenomics/adam/pull/247): For #244, Fragments were incorrect order and incomplete
* ISSUE [246](https://github.com/bigdatagenomics/adam/pull/246): Making sample ID field in genotype nullable.
* ISSUE [245](https://github.com/bigdatagenomics/adam/pull/245): Adding ADAMContig back to ADAMVariant.
* ISSUE [243](https://github.com/bigdatagenomics/adam/pull/243): Rebase PR#238 onto master

