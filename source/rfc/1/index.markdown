---
layout: page
title: "An example RFC"
date: 2013-12-29 10:51
comments: true
sharing: true
footer: true
---

_This is an example of an ADAM RFC. It is meant to demonstrate
syntax of an RFC as well as example headings and structure.
You can [view the source for this RFC online](https://raw.github.com/bigdatagenomics/bigdatagenomics.github.io/source/source/rfc/1/index.markdown).

### Schema

The following schema defines how to store 
[FASTA formatted data](http://en.wikipedia.org/wiki/FASTA_format)
in ADAM. The following schema captures all FASTA content.

```c
record ADAMFastaFragment {
    union {null, string } description = null;
    union {null, long } start = null;
    union {null, long } end = null;
    union {null, string } sequence = null;
}
```

All fields are optional and default to `null`. 

### Performance Considerations

The `end` field can be elided as it can be inferred from the `start` and `sequence` length.
However, for performance, a pushdown predicate on the start and end position would be
faster than materializing the sequence.

### Predicates

A commonly used predicate would be to find all sequences with a specific description that
`start` and `end` within a specified range.

### Common Operations

1. Reading the entire sequence
2. Reading a portion of the sequence that falls in a specified range.

### Open Questions

1. Should we require that the `end` field is always specified for performance?
2. Should we break up the description into the superset of all sequence identifiers?

### Filename extension

Once coverted to ADAM, the file extension will be `.fasta.adam`.


