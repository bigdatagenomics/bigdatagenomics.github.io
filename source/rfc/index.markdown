---
layout: page
title: "Requests for Comments"
date: 2013-12-26 11:04
comments: true
sharing: true
footer: true
---

An RFC is a "Request for Comment" in the same spirit as the [IETF RFC](http://www.ietf.org/rfc.html) but
less formal. There are no requirements on the language and structure of an
RFC; however, the focus should always be on concrete [Avro IDL](http://avro.apache.org/docs/1.7.5/idl.html)
examples to define the data structures with pseudo-code and/or clear descriptions of data transformations and use
cases. Once the team has reached consensus on an RFC, it should be *easy to turn the ideas
into working code*.

To create or modify an RFC,

1. Join the [ADAM Mailing List](/mail)
2. Fork the [bigdatagenomics.github.io repo](https://github.com/bigdatagenomics/bigdatagenomics.github.io)
3. Switch to the `source` branch and edit (or create) files in the `source/rfc` directory. 
   - To create a new RFC, you simply run e.g. `rake new_page["rfc/42"]` to create RFC #42. 
   - See [RFC #1](/rfc/1), for an example RFC. 
   - RFCs are written in [Markdown](http://daringfireball.net/projects/markdown/)
4. Test your edits using `rake generate preview` and opening your browser to `http://localhost:4000`.
5. Submit your changes as a pull request against the `source` branch.

All RFCs are open for comment with comment boxes in the footer.

List of RFCs:

1. [Example RFC](/rfc/1)
2. Next RFC



