---
layout: docs
title: "SPARQLing Plant Metabolic Pathways Wiki"
description: "Documentation and Tutorial for the PlantMetWiki SPARQL Explorer"
next: "/Assignments/assignment1/"
---

SPARQLing Plant Metabolic Pathways Wiki
=============================================================================================

## Summary 
---------
Plant Metabolic Pathways Wiki (PlantMetWiki) is an open online portal for querying linked specialized plant pathway information. PlantMetWiki is available in **Semantic Web format** as Resource Description Framework (**RDF**) and can be accessed via an easy-to-use **SNORQL user interface**. **Pre-written SPARQL queries** are available for users to execute or adapt to retrieve pathway information. **Federated queries** with other linked open data tools are supported, thereby expanding the [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page) framework. 


## Using the SPARQL Explorer 
---------

Visit our PlantMetWiki SPARQL interface at [plantmetwiki.bioinformatics.nl](https://plantmetwiki.bioinformatics.nl/). 

Follow the steps below to execute a pre-written query: 

1: **Select a query** from the list of example SPARQL queries. You can **adapt the query** by typing in the SPARQL Query box. 

2: Press the green **Query** button to execute your selected query. 

3: View the **results** on the same page. 

4: You can **select your own** list of example queries from github, by adding the link and click the **refresh button**.  

<p align="center">
  <img src="Images/Image-NewSnorqlInterface.png" alt="New Snorql Interface" />
</p>


## Output 
---------

Output data is available for download in native RDF format (.ttl), TSV, CSV, and json. 


## Resources
---------

* [Introduction to RDF and SPARQL](/Presentation_introRDF.pdf) by BiGCaT Maastricht University

* Wikipathway ontology [The WikiPathways WP Ontology](https://vocabularies.wikipathways.org/)

* [The WikiPathways Semantic Web Portal](https://classic.wikipathways.org/index.php/Portal:Semantic_Web)


## Tutorial 
---------
### Understand the Basics
* [Understanding SPARQL Queries]({{ "/Assignments/assignment1A/" | relative_url }})
* [Execute & retain results]({{ "/Assignments/assignment1B/" | relative_url }})
* [Expand and change query]({{ "/Assignments/assignment1C/" | relative_url }})

### Federated Queries on Wikidata
* [A more complicated query]({{ "/Assignments/assignment2D/" | relative_url }})
* [Additional analyses]({{ "/Assignments/assignment2A/" | relative_url }})
* [Answering biological questions]({{ "/Assignments/assignment2B/" | relative_url }})

### Recap:
* Update your SPARQL query from [this template]({{ "/ParticipantQueries/Example1/" | relative_url }})
* An [addendum]({{ "/Assignments/AddendumBioSb2019/" | relative_url }})

### Addendum
An [addendum](Assignments/AddendumBioSb2019.md) is available, containing:
* Answers to biological questions on Wikidata.
* More information on where to find Biological and Chemical properties (aka relationships) to expand your query.
* More detailed explanation of the SERVICE statement (since this is not directly part of SPARQL, but constructed by Wikidata for easier querying).

