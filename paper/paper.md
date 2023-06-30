---
title: 'BioHackJP 2023 Report R1: RDF Data integration using Shape Expressions'
title_short: 'BioHackJP 2023 Data Integration ShEx'
tags:
  - Linked Data
  - Shape Expressions
  - RDF
  - Data Integration
  - SPARQL
authors:
  - name: Jose Emilio Labra Gayo
    orcid: 0000-0001-8907-5348
    affiliation: 1
  - name: Andra Waagmeester
    orcid: 0000-0001-9773-4008
    affiliation: 2
  - name: Yasunori Yamamoto
    orcid: 0000-0002-6943-6887
    affiliation: 3
  - name: Ángel Iglesias Préstamo
    orcid: 0009-0004-0686-4341
    affiliation: 1
  - name: Toshiaki Katayama
    orcid: 0000-0003-2391-0384
    affiliation: 3
  - name: Thomas Liener
    orcid: 0000-0003-3257-9937
    affiliation: 4
  - name: Deepak R. Unni
    orcid: 0000-0002-3583-7340
    affiliation: 6
  - name: Jerven Bolleman
    orcid: 0000-0002-7449-1266
    affiliation: 6
  - name: Kiyoko F. Aoki-Kinoshita
    orcid: 0000-0002-6662-8015
    affiliation: 7
  - name: Masashi Yokochi
    orcid: 0000-0002-3253-7449
    affiliation: 8
  - name: Núria Queralt Rosinach
    orcid: 0000-0003-0169-8159
    affiliation: 9
  - name: Hiroshi Mori
    orcid: 0000-0003-0806-7704
    affiliation: 12
  - name: Daniel Fernández Álvarez
    orcid: 0000-0002-8666-7660
    affiliation: 1
  - name: Alberto Labarga
    orcid: 0000-0001-6781-893X
    affiliation: 11
  - name: Robert Hoehndorf
    orcid: 0000-0001-8149-5890
    affiliation: 13
  - name: Eric Prud’hommeaux
    orcid: 0000-0003-1775-9921
    affiliation: 14
  - name: Claude Nanjo
    orcid: 0009-0002-1208-8858
    affiliation: 15
  - name: Nishad Thalhath
    orcid: 0000-0001-9845-9714
    affiliation: 12
  - name: Yoko Okabeppu
    orcid: 0000-0000-0000-0000
    affiliation: 16
affiliations:
  - name: WESO Lab, University of Oviedo, Spain
    index: 1
  - name: Micelio, Belgium
    index: 2  
  - name: Database Center for Life Science, Japan
    index: 3
  - name: Unaffiliated 
    index: 4
  - name: SIB Swiss Institute of Bioinformatics, Switzerland
    index: 6
  - name: Soka University, Hachioji, Tokyo, Japan
    index: 7
  - name: Osaka University, Suita, Osaka, Japan
    index: 8
  - name: Leiden University Medical Center, Netherlands
    index: 9
  - name: National Institute of Genetics, Mishima, Japan 
    index: 10  
  - name: Barcelona Supercomputing Center
    index: 11
  - name: RIKEN Center for Integrative Medical Sciences, Yokohama, JP
    index: 12
  - name: Janeiro Digital, USA
    index: 13
  - name: MedOntology, LLC, USA
    index: 14
  - name: OKBP inc. Japan
    index: 15
date: 30 June 2023
cito-bibliography: paper.bib
event: BH23JP
biohackathon_name: "BioHackathon Japan 2023"
biohackathon_url:   "https://2023.biohackathon.org/"
biohackathon_location: "Kagawa, Japan, 2023"
group: R4
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/biohackathon-japan/bh23-dataintegrationshex
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: Jose E. Labra \emph{et al.}
---

# Background

[![hackmd-github-sync-badge](https://hackmd.io/jdpyyAXkSEK1RgT5QEx45g/badge)](https://hackmd.io/jdpyyAXkSEK1RgT5QEx45g)
<!-- TODO: Review by Toshiaki -->

In this report, we describe the activities that we have been carrying on during the Biohackathon 2023, held in Shodoshima, Japan. The main goal of the project has been to identify approaches and issues that can be used to integrate large RDF datasets by creating subsets described by [Shape Expressions](http://shex.io/).

We have recently submitted a publication on creating [subsets from Wikidata](https://www.semantic-web-journal.net/content/wikidata-subsetting-approaches-tools-and-evaluation-0). Wikidata is a knowledge graph which is constantly in flux and getting to a size which makes it hard to locally replicate. By creating topical subsets we are able to dissect a managable subset that can be loaded in local RDF stores for further processing. 

However, this subsetting app approach relies on Wikidata daily dumps, which are available in JSON format. For this hackathon 
we specifically choose to extend the subsetting mechanisms to work on RDF dumps or SPARQL endpoints.

RDF data provides a solution for data interoperability which in principle can enable different data sources to be smoothly integrated. Nevertheless, in practice, the growing adoption of RDF has also made that the consumption of RDF data is challenging given the size of RDF data collections makes them not feasible to be easily collected or manipulated. 
As an example, UniProt RDF data size is around 110 billions of triples or over 700 gigabytes of gzipped RDF/XML, and PubChem RDF is in the same order of magnitude in volume, both these resources describe 100's of different kinds of data. This situation requires some agents to provide intermediate resources to manipulate the RDF data which is consumed, and described. As an example, DBCLS has created the rdfconfig tool which provides descriptions of the RDF data collections they contain. 

In order to facilitate the integration of those RDF data, this project has been exploring ways to create subsets of RDF data which could contain only the information of interest for some specific tasks. Making those subsets available for researchers in an easy way, could facilitate the research activities and give the RDF data more value. 

Creating subsets of RDF data can also help when the information available in those large RDF data sources is continually evolving. So if a researcher wants to make reproducible research based on those SPARQL endpoints, it may be possible that the results of the queries can differ along the time. 

The possibility of having a tool that creates subsets can be considered as a way to create snapshots of the RDF data which could later be packaged and distributed along the research results, helping the creation of reproducible research based on RDF data.

Another reason for the creation of RDF subsets is to make a subset from multiple data sources where it is unfeasible to get a designated dataset using federated queries due to these data sizes and technical immaturity of SPARQL federated query handling.

The Shape Expressions language was designed as a concise and human-readable language to describe and validate RDF data. Although it was not initially designed to create subsets, the possibility of having a concise way to describe RDF makes it an ideal candidate for this task. 

Indeed, ShEx has already been used to create subsets of Wikidata. The following paper contains a list of different subseting approaches which have been used in Wikidata: https://www.semantic-web-journal.net/content/wikidata-subsetting-approaches-tools-and-evaluation-0. 

The main goals of this project have been to review the different approaches to create subsets of large RDF data in order to facilitate the integration of different RDF collections. 

We identified the participation of 3 main agents:
- Data producers or providers who are interested in produce RDF data that can have more value and be used by third parties.
- Data consumers who are interested in an easy way to get access to the RDF data available in those sources and to create reproducible workflows which can contain manageable RDF subsets.
- Data integrators who can help in the intermediate process of bringing over the RDF data produced by the providers to the end consumers. One important aspect to take into account is that the RDF data may need to be transformed with actions like changing URI prefix declarations or manipulating the topology of the RDF data. Another aspect is the need to understand the structure of the RDF data collected and to agree in a common structure. For this, Shape expressions also offer the possibility to validate the RDF data that is produced and the RDF data that may be consumed. Avoiding the need of defensive programming techniques. 

# Outcomes

## Creation of ShEx based subsets

One important aspect of data integration is the possibility of creating small subsets from large RDF data. During the biohackathon we explored several use cases taking data from UniProt, PubChem, TogoGenome, etc. 

We created a github repository called [subsetting-examples](https://github.com/shex-consolidator/subsetting-examples) that contains some example SPARQL queries, ShEx schemas and scripts used during the biohackathon. 

The use cases that we explored are the following:


### UniProt subset based on proteins
<!-- TODO: Review by Kiyoko -->
<!-- TODO: Review by Ángel Iglesias -->
We did a first experiment taking as input RDF dumps from UniProt. Kiyoko provided a [simple SPARQL query](https://github.com/shex-consolidator/subsetting-examples/blob/master/protein/protein.sparql) that obtains proteins and annotations. Jose Labra converted the SPARQL query to [ShEx](https://github.com/shex-consolidator/subsetting-examples/blob/master/protein/protein.shex). That ShEx schema was used as input in the [PSChema tool](https://github.com/angelip2303/pschema-rs) that has been developed by Ángel Iglesias in Rust and we already created a subset that was later [published in Zenodo](https://doi.org/10.5281/zenodo.8086938) with its proper doi. 

Some statistics:
- Given an input dataset obtained from the Uniprot’s release page were several dumps are published, we have downloaded one with the following characteristics:
- Input data: Size of input data: 1,2 GB
- Number of Triples: 7,346,129 entities
- Subset generated:
    - Number of Triples: 1,084,151 entities
    - Size of the parquet file (compressed): 0,000101923 GB
    - N-Triples (uncompressed): 0,151250127 GB

The experiment was conducted in such a manner that the program was executed three times and the mean was calculated. The following results were obtained:
- Execution time: 14.59163319 seconds
- Memory consumption: 6.03 GB
- Number of cores:  16 cores


### UniProt subset based on Subcellular locations
<!-- TODO: Review by: Yasunori -->
<!-- TODO: Review by: Ángel Iglesias -->

Yasunori provided an example [SPARQL query about Subcellular locations](https://github.com/shex-consolidator/subsetting-examples/blob/master/subcellular-locations/subcellular-locations.sparql) that was later converted by Jose Labra to [ShEx](https://github.com/shex-consolidator/subsetting-examples/blob/master/subcellular-locations/subcellular-locations.shex) in order to create subsets from UniProt. The subsets were generated by Ángel Iglesias using the PSChema Rust tool which was interesting as we discovered a bug that required an update of the tool. 

### Analysed some use cases to extracr subsets from PubChem, GlyCosmos and Reactome
<!-- TODO: Review by Kiyoko -->
Kiyoko suggested that we could use [example 10](https://pubchem.ncbi.nlm.nih.gov/docs/rdf-use-cases#section=Case-10-Summarize-the-statistics-about-the-total-number-of-substances-tested-in-the-PubChem-database-against-each-protein-target-) from PubChem documentation which works on PubChem RDF data. We analysed the [RDF data dumps](https://ftp.ncbi.nlm.nih.gov/pubchem/RDF/)

Another use case suggested from GlyCosmos was this SPARQL query:

```sparql=
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#>
PREFIX gco: <http://purl.jp/bio/12/glyco/conjugate#>
PREFIX faldo: <http://biohackathon.org/resource/faldo#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX mass: <https://glycoinfo.gitlab.io/wurcsframework/org/glycoinfo/wurcsframework/1.0.1/wurcsframework-1.0.1.jar#>

SELECT 
(SUBSTR(STR(?protein), 33) AS ?ac) 
?beginP
    ?saccharide
WHERE {
      ?protein <http://purl.jp/bio/12/glyco/conjugate#glycosylated_at> ?glyco_site .
    ?glyco_site faldo:location ?site  .
    ?site faldo:position ?beginP .
    ?glyco_site <http://purl.jp/bio/12/glyco/conjugate#has_saccharide> ?saccharide .
  }

```


That SPARQL query should work in [GlyCosmos SPARQL endpoint]( https://ts.alpha.glycosmos.org/sparql) which follows [this schema](https://glycosmos.org/programmatic). The RDF dump can be obtained by downloading the results of this SPARQL query as RDF:

```sparql=
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX glycan: <http://purl.jp/bio/12/glyco/glycan#>
PREFIX gco: <http://purl.jp/bio/12/glyco/conjugate#>
PREFIX faldo: <http://biohackathon.org/resource/faldo#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX mass: <https://glycoinfo.gitlab.io/wurcsframework/org/glycoinfo/wurcsframework/1.0.1/wurcsframework-1.0.1.jar#>
CONSTRUCT {
  ?protein <http://purl.jp/bio/12/glyco/conjugate#glycosylated_at> ?glyco_site .
  ?glyco_site faldo:location ?site  .
  ?site faldo:position ?beginP .
  ?glyco_site <http://purl.jp/bio/12/glyco/conjugate#has_saccharide> ?saccharide .
}
WHERE {
    ?protein <http://purl.jp/bio/12/glyco/conjugate#glycosylated_at> ?glyco_site .
    ?glyco_site faldo:location ?site  .
    ?site faldo:position ?beginP .
    ?glyco_site <http://purl.jp/bio/12/glyco/conjugate#has_saccharide> ?saccharide .
}

```

Another example suggested by Kiyoko was based on Reactome, using this SPARQL query:

```Sparql=
PREFIX biopax3: <http://www.biopax.org/release/biopax-level3.owl#>
SELECT DISTINCT ?pathway_ID ?pathwayName ?organism
FROM<http://rdf.glycosmos.org/pathway_reactome_v83>
WHERE {
  ?pathway a biopax3:Pathway .
  ?pathway biopax3:displayName ?pathwayName .
  ?pathway biopax3:organism/biopax3:name ?organism .

  ?pathway biopax3:xref ?unixref .
  ?unixref biopax3:db ?db_g .
  FILTER(STR(?db_g)="GlyCosmos")
  ?pathway biopax3:xref ?unixref_r .
  ?unixref_r biopax3:db ?db_r ;
                    biopax3:id ?pathway_ID .
  FILTER(STR(?db_r)="Reactome")
}
```

which should be run in the following SPARQL endpoint: https://ts.glycosmos.org/sparql and the expected results should be [the following](https://reactome.org/download/current/biopax.zip)


### TogoGenome
<!-- TODO: Review by Hiroshi Mori -->
<!-- TODO: Review by Yoko Okabeppu -->
Hiroshi Mori and Yoko Okabepu created this [example SPARQL query](https://is.gd/MGeyxI) which is working agains the [TogoGenome SPARQL endpoint](http://togogenome.org/sparql) or the [TogoGenome RDF data dumps](http://togogenome.org/rdf). Jose Labra converted the SPARQL query to [ShEx](https://github.com/shex-consolidator/subsetting-examples/blob/master/togogenome/togogenome.shex). 

### Reactome
<!-- TODO: Review by Kiyoko -->
Kiyoko selected a query from Reactome. 

## Subsetting book
<!-- TODO: Review by Andra Waagmeester --> 
Andra Waagmeester created an executable book explaining some approaches to create subsets. 

<!-- TODO: Review by Nuria Queralt --> 
The book was automatically published using Github actions created by Núria Queralt Rosinach. 


## Work on sheXer 
<!-- TODO: Review by Yasunori, Daniel Fernández -->
sheXer is a tool that extracts ShEx descriptions from RDF dumps. One problem is that it can raise out-of-memory errors or take much longer time when the RDF dumps are large so one possibility is to split the RDF dumps in portions, run sheXer for those portions and consolidate their results. During the biohackathon, Daniel Fernández created a tool that consolidates ShEx results.


## Use case about Multi-omics data integration
<!-- TODO: Review by Alberto Labarga -->
<!-- TODO: Review by Robert Hoehndorf -->

We were discussing a possible use case about data integration on multi-omics. 

[Med2RDF](http://med2rdf.org/)
- UniProt
- A possible example from natural language: “Patient with headache and some symptoms”
- Tasks to do: Obtain relationships, extract triplets, obtain treatments/possible diagnostics/test
- HPO
- https://athena.ohdsi.org/ (OMOP vocabularies)
- Generate RDF from relationships

Possible tools to use: [ShExML](http://shexml.herminiogarcia.com/).

## Analysis about RDF data description mechanisms
<!-- TODO: Review by Deepak Unni -->
<!-- TODO: Review by Toshiaki -->

As with any data source, it is necessary to describe its structure. And if we want to obtain subsets of RDF data then the RDF data has to be described in sufficient detail that can 
In order to obtain RDF data subsets it is necessary to describe the desired contents of the RDF data. Apart from ShEx, we were analysing other possibilities like [LinkML](https://linkml.io/) and [RDF-config tool](https://github.com/dbcls/rdf-config). 

Both LinkML and RDF-config describe RDF models in YAML and are able to generate ShEx shapes, so we started to compare the output of both using PubChem as an example.

Deepak provided an example [LinkML-based ShEx file](https://github.com/shex-consolidator/subsetting-examples/blob/master/linkml_rdfconfig/pubchemrdf_by_linkml.shex) generated from a LinkML schema that describes PubChem.

Toshiaki provided an example [RDF-config based ShEx file](https://github.com/shex-consolidator/subsetting-examples/blob/master/linkml_rdfconfig/pubchem_by_rdfconfig.shex) generated by RDF-config. 

We were analysing the shapes and it seems that the ones generated by RDF-config look simpler and more clean although the ones generated by LinkML seem to contain more properties. As an example, this is the shape of the `taxonomy` class as generated by LinkML:

```shex=
<Taxonomy> CLOSED {
    (  $<Taxonomy_tes> (  rdf:type @linkml:Uri * ;
          skos:prefLabel @linkml:String ? ;
          skos:altLabel @linkml:String ? ;
          owl:sameAs @linkml:Uri ? ;
          skos:closeMatch @linkml:Uri *
       ) ;
       rdf:type [ <Taxonomy> ] ?
    )
}
```


and this is the shape generated from RDF-config:

```shex=
<PubChemTaxonomyShape> {
  rdf:type [biopax:taxonomy] ;
  dcterms:title xsd:string  ;
  skos:closeMatch IRI *
}
```

<!-- TODO: Review by Jose -- this is from updated version of RDF-config -->
```shex=
<PubChemTaxonomyShape> {
  rdf:type [sio:SIO_010000] ;
  skos:prefLabel xsd:string  ;
  skos:altLabel xsd:string * ;
  skos:closeMatch IRI * ;
  cito:isDiscussedBy IRI * ;
  owl:sameAs IRI 
}
```

One aspect of the shapes generated by LinkML is that the constraint on `rdf:type` property needs to be improved to capture the enumeration of possible values. It seems that the LinkML ShEx generator doesn't convert enums so Deepak [raised a issue](https://github.com/linkml/linkml/issues/1513) about this. A fix for the issue is already in place and will be incorporated into LinkML soon.


# Future work

## Add FROM <...> to ShEx
<!-- TODO: Review by Eric Prud'hommeaux -->
- Problem to solve: how to validate/extract RDF data when it is behind different RDF graphs? 
- Currently ShEx processors assume a single RDF graph but it may be interesting to add some kind of annotation to ShEx which allows ShEx processors to take into accout the `FROM` declarations when they are working against SPARQl endpoints.

## Machine-readable way that RDF data providers describe their RDF dumps
<!-- TODO: Review by Jerven Bolleman  -->

One problem for creating RDF data subsets from RDF data dumps is that not all RDF providers follow the same structure to publish the RDF dumps and that the descriptions of the contents of those RDF dumps are not published. 

In order to solve that problem it would be interesting to follow some guidelines or common patterns such as providing files in the `./well-known/` namespace. These files should contain or direct to machine-readable descriptions in either ShEx or SHACL.

During the biohackathon we were reviewing the way that different data providers follow to describe their RDF dumps. Some of the providers we took into account were:
    - [Uniprot](https://ftp.uniprot.org/pub/databases/uniprot/current_release/rdf/) 
    - [PubChem](https://ftp.ncbi.nlm.nih.gov/pubchem/RDF/)
    - [TogoGenome](http://togogenome.org/rdf/)

In order to add `.well-known/void` declarations, Jerven Bolleman already had the [void-generator](https://github.com/JervenBolleman/void-generator). [VoID](https://www.w3.org/TR/void/) is a framework to describe the statistical distribution of data elements and their links. During the biohackathon, Jerven Bolleman and Jose Labra started a new project called [void2shapes](https://github.com/shex-consolidator/void2shapes) to generate ShEx and SHACL declarations from [void descriptions](https://www.w3.org/TR/void/) in UniProt. We succeeded in generating a minimal ShACL representation from the VoID files of [UniProt](https://sparql.uniprot.org/sparql) and [SwissLipids](https://beta.sparql.swisslipids.org/sparql).


## Improve the ShEx tooling
<!-- TODO: Review by Nishad -->

We are currently transitioning some of the ShEx implementations that were initially implemented in Scala to Rust. 
We have been looking to some tools in the Rust ecosystem like the [Severless SPARQL endpoint](https://github.com/nishad/serverless-sparql-endpoint) that has been developed by Nishad in Javascript for Deno based on the [Oxigraph database](https://oxigraph.org/) that is implemented in Rust that has a binding that works in [WebAssembly](https://webassembly.org/). We consider that this solution could enable further performance and dynamic scalability and enable edge-computing use cases. One possible use case would be to adapt the next Rust implementation of ShEx to work on WebAssembly and could be integrated with OxiGraph and do ShEx validation on-the-fly.

## Improvements to sheXer
<!-- TODO: Review by: Yasunori -->

- Automatic splitting of large RDF data files
- Parallel/distributed extraction of ShEx portions
- Automatic consolidation of ShEx portions
- Generation of rdfconfig yaml files

## SPARQL to ShEx converter

- We noticed that in several use cases, we start with an example SPARQL query which we want later to convert to ShEx. An interesting project would be to generate ShEx from those SPARQL queries.
<!-- TODO: Review by Jose Labra -->

## Explore the idea about ShEx transitions

- Being able to declare transitions between ShEx schemas so they can be checked as pre- and post- conditions, example before/after SPARQL updates. This idea has been provided by Thomas Liener [in this document](https://docs.google.com/document/d/1LyBlRuwvl6BQZkEQG6_2gc2kJrs05qaYVie-w4-Lnvc/edit?usp=sharing).
<!-- TODO: Review by Thomas Liener -->

## ShEx template injection 
- Being able to have parameterizable ShEx schemas whose specific values could be injected in a way that ShEx schemas could be dynamically generated
<!-- TODO: Review by Thomas Liener -->

## RDF representation of ShapeMaps

- We resumed the work on [this issue](https://github.com/shexSpec/shex/issues/67) and we expect to update the [ShapeMap specification](http://shex.io/shape-map/) including a description of a representation of shape maps that shows how to represent them in RDF.
<!-- Review by Eric Prud'hommeaux --> 

## Generating ShEx files from void descriptions
- RDF representation of ShEx uses RDF lists which are a bit challenging to generate from SPARQL Construct queries. Jerven Bolleman solved it generating IRIs for the intermediate nodes but it may be interesting to generate the nodes in the list as blank nodes. Eric Prod'hommeaux raised [this issue](https://github.com/apache/jena/issues/1933) in Apache Jena and it would be interesting work to see if we could solve the issue or find some way to solve the problem. 
- This work will be folllowed on in the [void2shapes](https://github.com/shex-consolidator/void2shapes/) project.
<!-- TODO: Review by Eric Prud'hommeaux --> 
 
## Conversion from ShEx to LinkML
<!-- TODO: Review by Deepak Unni --> 
We were discussing the possibility of converting ShEx to LinkML which would allow to roundtrip between both data formats and this is something that Jose Labra and Deepak would like to work in the future. 

Preliminary analysis seem to indicate that the conversion is possible but with some limitations which can be addressed if the use-cases are well defined and sufficent examples are available for rapid prototyping.

## Conversion from ShEx to RDF-config YAML files

Another possibility is to explore the generattion of RDF-config YAML files that follow [this specification](https://github.com/dbcls/rdf-config/blob/master/doc/spec.md).



## Use case about integrating RDF data and clinical records
<!-- TODO: Review by Claude Nanjo -->

## Use case about drug repurposing 
<!-- TODO: Review by Núria -->


## Acknowledgements

We would like to thank the fellow participants at BioHackathon 2023 for their collaboration and constructive advice, which greatly influenced our project. We are grateful to the organizers for providing this platform and the developers of open source language models. Special thanks to our mentors, advisors, and colleagues for their guidance and support. Without their contributions, our project in linked data standardization with LLMs in bioinformatics would not have been possible.

## References

1.
