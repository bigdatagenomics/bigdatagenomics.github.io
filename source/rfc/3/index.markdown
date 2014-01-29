---
layout: page
title: "Variant Annotation Proposal from Mount Sinai"
date: 2014-01-29 16:08
comments: true
sharing: true
footer: true
---

Status: DRAFT

### Introduction

Similar to [RFC #2](/rfc/2), we'd like to propose one way of storing
annotations in a format that is amenable to joining against ADAM
variant data.  The purpose of this RFC is to propose a schema (in the
form of AVRO IDL) to represent both a set of called variants and
annotations on those variants.

One particular requirement of our approach is to separate several concepts:

* The notion of what a variant is, which is represented by the ADAMVariant record:
    * Chromosome
    * Location on the chromosome
    * Reference allele
    * Alternate allele

  We believe that by making a minimalist definition of a variant, and using that
  as a key in the various other structures, a high degree of consistency is achieved with respect
  to what annotations and genotypes are actually associated with.

* A set of annotations on a variant, such as dbSNP ID, Clinvar data,
  or arbitrary properties of a novel variant, represented by
  ADAMDatabaseVariantAnnotation.  In this proposal, it only contains the
  dbSNP ID, but, in the future, it will contain a field for each new
  type of annotation.

* A set of genotypes that use the variant definition (1) to indicate
  calls on samples, as represented by the ADAMGenotype record.

### Schema

```c
protocol ADAM {
import idl "adam.avdl";

record ADAMContig {
  union { null, string } contigName;
  // Do we need the reference filename if we also have URL?
  union { null, string } referenceName;
  union { null, long }   referenceLength = null;

  union { null, string } referenceURL = null;
  union { null, string } referenceMD5 = null;
}

record ADAMVariant {
  union { null, ADAMContig } contig;
  union { null, long }       position;
  array<Base> referenceAlleles;
  array<Base> variantAlleles;
}

enum ADAMGenotypeAllele {
  Ref, Alt, NoCall
}

// This record represents all stats that, inside a VCF, are stored outside of the
// sample but are computed based on the samples.  For instance,  MAPQ0 is an aggregate
// stat computed from all samples and stored inside the INFO line.
record VariantStatistics {
  union { null, double } baseQRankSum;
  union { null, double } clippingRankSum;
  union { null, int } readDepth;
  union { null, boolean } downsampled;
  union { null, double } fisherStrandBiasPValue; // Phred-scaled.
  union { null, float } haplotypeScore;
  union { null, float } inbreedingCoefficient;
  array<int> alleleCountMLE;
  array<int> alleleFrequencyMLE;
  union { null, float } rmsMapQ;
  union { null, int } mapq0reads;
  union { null, float } mqranksum;
  union { null, boolean } usedForNegativeTrainingSet;
  union { null, boolean } usedForPositiveTrainingSet;
  union { null, float } variantQualityByDepth;
  union { null, float } readPositionRankSum;
  union { null, float } vqslod; // log odds ratio of being a true vs false variant under trained gaussian mixture model
  union { null, string } culprit;
}

record ADAMGenotype {
  union { null, ADAMVariant } variant;

  // Variant-level "provenance" data, i.e. data shared amongst all concurrently genotyped samples
  union { null, VariantStatistics } variantStats;

  union { null, boolean } varIsFiltered = null;  // "null" implies no filters were applied
  array <string> varFilters = null;

  // Sample-level data, i.e. data specific to this particular sample
  union { null, string } sampleId = null;

  // Length is equal to the ploidy
  array <ADAMGenotypeAllele> alleles = null;

  union { null, int } referenceReadDepth;
  union { null, int } alternateReadDepth;
  union { null, int } readDepth;

  union { null, string } filter;
  union { null, int } genotypeQuality;

  // In the ADAM world we split multiallelic VCF lines into multiple
  // single-alternate records.  This bit is set if that happened for this
  // record.
  union { null, boolean } splitFromMultiAllelic;

  array<int> genotypeLikelihoods; // Phred-scaled. Always length 3 since we are not multiallelic.

  union { null, boolean } isPhased = null;
  union { null, string }  phaseSetId = null;
  union { null, int }     phaseQuality = null;
}

record ADAMDatabaseVariantAnnotation {
  union { null, ADAMVariant } variant;

  union { null, int } dbsnpId = null;
}

}
```

