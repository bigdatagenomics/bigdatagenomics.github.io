---
layout: page
title: "Requests for Comments: Adding or Updating"
date: 2013-12-26 11:04
comments: true
sharing: true
footer: true
---

### To create or modify an RFC

1. Join the [ADAM Mailing List](/mail)
2. Fork the [bigdatagenomics.github.io repo](https://github.com/bigdatagenomics/bigdatagenomics.github.io)
3. Switch to the `source` branch and edit (or create) files in the `source/rfc` directory
   - To create a new RFC, you simply run e.g. `rake new_page["rfc/42"]` to create RFC #42
   - See [RFC #1](/rfc/1), for an example RFC
   - RFCs are written in [Markdown](http://daringfireball.net/projects/markdown/)
   - All [Octopress](http://octopress.org/docs/) extensions are supported
4. Test your edits by running `rake generate preview` and opening [http://localhost:4000](http://localhost:4000) in your browser
5. Submit your changes as a pull request against the `source` branch

