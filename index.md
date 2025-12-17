---
layout: docs
title: "SPARQLing Plant Metabolic Pathways Wiki"
description: "Documentation and Tutorial for the PlantMetWiki SPARQL Explorer"
order: 0
permalink: /
---

## Summary 
---------
Plant Metabolic Pathways Wiki (PlantMetWiki) is an open online portal for querying linked specialized plant pathway information. PlantMetWiki is available in **Semantic Web format** as Resource Description Framework (**RDF**) and can be accessed via an easy-to-use **SNORQL user interface**. **Pre-written SPARQL queries** are available for users to execute or adapt to retrieve pathway information. **Federated queries** with other linked open data tools are supported, thereby expanding the [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page) framework. 


## Using the SPARQL Explorer 
---------

Visit our PlantMetWiki SPARQL interface at [plantmetwiki.bioinformatics.nl](https://plantmetwiki.bioinformatics.nl/). 

Follow the steps below to execute a pre-written query: 

1: **Select a query** from the list of example SPARQL queries. You can **adapt the query** by typing in the SPARQL Query box or from the source repository [pathway-lod/SPARQLQueries](https://github.com/pathway-lod/SPARQLQueries) 

2: Press the green **Query** button to execute your selected query. 

3: View the **result s** on the same page. 

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

* [Guide to WikiPathways SPARQL Queries](https://www.wikipathways.org/sparql.html)

* [The WikiPathways Semantic Web Portal](https://classic.wikipathways.org/index.php/Portal:Semantic_Web)



## Tutorial Pages
---------

{% assign tutorials = site.tutorial | sort: "order" %}

{% for p in tutorials %}
* [{{ p.title }}]({{ p.url | relative_url }})
{% endfor %}

## Recap
---------
* Update your SPARQL query from [this template]({{ "/ParticipantQueries/Example1/" | relative_url }})
* An [addendum]({{ "/ParticipantQueries/AddendumBioSb2019/" | relative_url }}) iw available containing:
  * Answers to biological questions on Wikidata.
  * More information on where to find Biological and Chemical properties (aka relationships) to expand your query.
  * More detailed explanation of the SERVICE statement (since this is not directly part of SPARQL, but constructed by Wikidata for easier querying).

## Data availability 
---------

Data related to PlantMetWiki is available at [Zenodo PlantMetWiki Community](https://zenodo.org/communities/plantmetwiki/). 