---
title: "BioHackJP 2023 Report R1: RDF Data integration using Shape Expressions"
title_short: "BioHackJP 2023 Data Integration ShEx"
tags:
  - Linked Data
  - Shape Expressions
  - RDF
  - Data integration
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
  - name: Deepak Unni
    orcid: 0000-0002-3583-7340
    affiliation: 5
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
  - name: Nishad Thalhath
    orcid: 0000-0001-9845-9714
    affiliation: 12
  - name: Robert Hoehndorf
    orcid: 0000-0001-8149-5890
    affiliation: 13
  - name: Eric Prud’hommeaux
    orcid: 0000-0003-1775-9921
    affiliation: 14
  - name: Claude Nanjo
    orcid: 0000-0000-0000-0000
    affiliation: 15
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
  - name: Swiss Institute of Bioinformatics, Basel, Switzerland
    index: 5
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
biohackathon_url: "https://2023.biohackathon.org/"
biohackathon_location: "Kagawa, Japan, 2023"
group: R4
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/biohackathon-japan/bh23-dataintegrationshex
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: Jose E. Labra \emph{et al.}
---

# Background

In this report, we describe the activities that we have been carrying on during the Biohackathon 2023, held in Shodoshima, Japan. The main goal of the project has been to identify approaches and issues that can be used to integrate large RDF datasets creating subsets described by Shape Expressions.

# Hackathon results

## Creating Knowledge graph subsets using Shape Expressions

During the hackathon, we have been working on the creation of knowledge graph subsets of using Shape Expressions. For us to achieve so, several Shapes were proposed and a selection of them was used to create the subsets. Provided the Shape Expressions written in a formal language, we have also rewritten them in such a way that is accepted by the [`pschema-rs`](https://github.com/angelip2303/pschema-rs) validator. It is worth mentioning that we have created a subset of the UniProt knowledge graph, more precisely, we have used the `uniprotkb_reviewed_viruses_10239_0.rdf` dump with a size of 1.13GB. As it is serialized in a RDF/XML format, a tiny pipeline for converting it into N-Triples, the serialization format accepted by the tool, is required. The script that automatically processes the [compressed dump](https://github.com/angelip2303/pschema-rs/blob/main/examples/from_uniprot/uniprotkb_reviewed_viruses_10239_0.rdf.xz) can be seen in the [examples](https://github.com/angelip2303/pschema-rs/blob/main/examples/bh23/run.sh) section of the tool's repository. According to this, the [first expression](https://github.com/angelip2303/pschema-rs/blob/af0af7da9d217f4fe626ec49edc73deaa08e1f7e/examples/from_uniprot/main.rs#L25) that we have created was a simple one that selects all the proteins that have a glycosylation annotation. The expression is shown below:

```{shex}
# Example created at Biohackathon 2023
# Author: Jose Emilio Labra Gayo
# Based on SPARQL query provided by: Kiyoko Aoki-Kinoshita

PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX up: <http://purl.uniprot.org/core/>

<protein> {
    a             [ up:Protein ] ;
    up:annotation @<annotation>
}

<annotation> {
    a [ up:Glycosylation_Annotation ]
}
```

The [second expression](https://github.com/angelip2303/pschema-rs/blob/af0af7da9d217f4fe626ec49edc73deaa08e1f7e/examples/bh23/main.rs#L27) that we have created is a more complex one trying to seek for the limits of the tool. Implementing it with the tool posed a challenge as it is still in an early stage of development. The expression is shown below:

```{shex}
PREFIX up: <http://purl.uniprot.org/core/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX faldo: <http://biohackathon.org/resource/faldo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

<protein> {
   up:annotation @<annotation> ;
}

<annotation> {
  up:range     @<range> ;
  rdfs:comment . ;
  a [ up:Transmembrane_Annotation
      up:Topological_Domain_Annotation
   ] *
}

<range> {
    faldo:begin @<begin> ;
    faldo:end   @<end>
}

<begin> {
    faldo:position .
}
<end> {
    faldo:position .
}
```

With that being said, `pschema-rs` has been tested against the already described Shapes tracking the time it takes for the tool to process the dataset and the memory consumed. The program is run three times for each Shape Expression, after which the average results are calculated. Note that a machine with an Intel(R) Xeon(R) Silver 4214 CPU @ 2.20GHz (12 cores and 24 threads), and 40GB of RAM memory installed is used to run the experiment. The results are presented in the table below:

| Shape Expression | Number of triples Initially | Number of resulting triples | Time (s) | Memory consumption (GB) |
| ---------------- | --------------------------- | --------------------------- | -------- | ----------------------- |
| 1                | 7,346,129                   | 226,241                     | 14.58    | 3.87                    |
| 2                | 7,346,129                   | 1,084,151                   | 37.76    | 3.75                    |

In relation to the results obtained, it is important to note that despite being in the early stages of development, the tool demonstrates the capability to process large datasets in a reasonable amount of time, while maintaining an acceptable level of memory consumption. To achieve this, numerous optimizations were implemented, including the use of a system that converts textual representations of subject-predicate-object triple into numeric ones. This approach enables the tool to utilize less memory and improve its speed through the cache. Additionally, it is observed that the memory usage of the tool does not appear to be directly related to the complexity of the expression, but with the number of triples matched. However, further testing is being conducted to make this assertion more robust.

Lastly, it is worth mentioning that the subset resulting from the application of the first Shape Expression is available at Zenodo [@angel_iglesias_prestamo_2023_8086938]. The subset is serialized in N-Triples format.

# Future work

## Acknowledgements

We would like to thank the fellow participants at BioHackathon 2023 for their collaboration and constructive advice, which greatly influenced our project. We are grateful to the organizers for providing this platform and the developers of open source language models. Special thanks to our mentors, advisors, and colleagues for their guidance and support. Without their contributions, our project in linked data standardization with LLMs in bioinformatics would not have been possible.

## References

1.
