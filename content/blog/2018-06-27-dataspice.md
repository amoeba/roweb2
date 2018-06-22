---
slug: "dataspice"
title: Title to be decided
package_version: 0.0.0.9000
authors:
  - name: Carl Boettiger
    url: https://github.com/cboettig
  - name: Auriel Fournier
    url: https://github.com/aurielfournier
  - name: Kelly Hondula
    url: https://github.com/khondula
  - name: Anna Krystalli
    url: https://github.com/annakrystalli
  - name: Bryce Mecum
    url: https://github.com/amoeba
  - name: Maëlle Salmon
    url: https://github.com/maelle
  - name: Kate Webbink
    url: https://github.com/magpiedin
  - name: Kara Woo
    url: https://github.com/karawoo
date: 2018-06-27
categories: blog
topicid: 9999
tags:
- r
- software
- metadata
- eml
- schema-org
- jsonld
- xml
- github-pagesw
---

## Notes

Notes:

- potential title: Making minimum viable metadata
- Link out to related work such as the EML package, emld, eml2, rdflib, emldown

Potential outline:

- What does dataspice do?
- Why use Google's standard?
- How is this different than `emldown`?
- How can you contribute?
- Discuss codebook (See [the ropensci discuss post](https://discuss.ropensci.org/t/dataspice-codebook-and-ropensci-scope/1179/9))

## The post body (remove this header prior to publicatoin)

As researchers, we generate and consume data all the time.
But data, all by itself, often lacks enough information for us to answer questions such as:

- How do I cite the data?
- Where did the data come from and what are its usage rights?
- Are the data appropriate to use with another dataset?
- What does that obscure code even mean?

A common approach to documenting the information we need to answer these questions is to generate a metadata record that sits alongside the data.
Very often, this metadata record is an XML file that follows a metadata standard that lets us describe a ton of information about our data.
These XML metadata records are great in their expressiveness and useful for computers but suffer a few common problems:

- Creating or editing these XML files by hand is confusing and error-prone and tools for working with them may not exist or are difficult to use
- Using these standards is like learning a new language which is a large task for someone just trying to document their data

Stemming from a [discussion](https://github.com/ropensci/unconf18/issues/72) started by [Anna Krystalli](https://github.com/annakrystalli) as part of the [2018 rOpenSci Unconf](http://unconf18.ropensci.org) that there should be a way to create a minimal amount of useful metadata that both adheres to standards but is intuitively easy to use and widely applicable, we decided to test an idea to address these issues.

Enter [`dataspice`](https://github.com/ropenscilabs/dataspice).

`dataspice` takes your raw data, and creates the spice on top of it, the metadata, which is so important for communicating with yourself in the future, as well as any others who may want to use your data. `dataspice` creates template metadata files based on your data, provides easy to use functions and even Shiny apps to help you guide you to filling them in. Once filled in, the templates can be used to:

- Make a simple website (like [pkgdown](https://github.com/r-lib/pkgdown) but for your data) that you can easily host on [GitHub Pages](https://pages.github.com)
- Generate a [JSON-LD](https://json-ld.org) record that Google and others understand which you can embed in a website (including the one `dataspice` makes for you).

Our hope is to make creating metadata more accessible to everyone!

To address the problem of existing metadata standards providing such a vast language, we started looking into Google's specification of a [Dataset](https://developers.google.com/search/docs/data-types/dataset), which would be a lowest common denominator of metadata that could support search and discovery of datasets regardless of what they are about or what repository they end up in (if any!). Google adopts their definition from [schema.org](http://schema.org/Dataset) and recommends a minimal number of those properties that should describe a dataset, such as a title, description, keywords, variable measured, and spatial and temporal coverage. But because even Google doesn't have a bounded definition of what a dataset is, it seems like the only required detail is identifying the "@type" property as a dataset, and providing a name. So, at the minimum, our package would need to produce a [json-ld](https://json-ld.org/) file that looks something like this:

```json
<script type="application/ld+json">
{
  "@context":"http://schema.org/",
  "@type":"Dataset",
  "name":"My dataset"
}
</script>
```

For a tabular dataset (e.g. `csv` or `tsv` files), some key elements of the metadata are human readable descriptions of what is in each column, and what units numeric data are in. For our framework, these translate to the dataset's [measured variable](http://pending.webschemas.org/variableMeasured) properties, which themselves can have properties of type (such as text or number), unit, and description.  

Although we couldn't automate the whole process, we wrote functions that create the metadata structure for a given set of data files. These templates are a few spreadsheets that can be filled in manually or with some helper functions or a Shiny app.

Once the information is filled in, `dataspice` will slot it into a properly formatted json file, and then create a summary [webpage](https://amoeba.github.io/dataspice-example/) or [card](https://cboettig.github.io/dataspice-web/) about the "spiced" dataset. This information could also later be used to help automate the creation of more in-depth metadata like EML.

Head over to our [GitHub repo](https://github.com/ropenscilabs/dataspice) for more details and to try it out!
