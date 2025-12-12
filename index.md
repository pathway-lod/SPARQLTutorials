---
layout: default
---

SPARQLing Plant Metabolic Pathways Wiki
=============================================================================================

<p align="center">
  <a href="https://pathway-lod.github.io/SPARQLTutorials/" style="display:inline-block;padding:12px 28px;background:#2196F3;color:white;border-radius:8px;text-decoration:none;font-size:16px;margin:8px;">
    Tutorial Pages Home
  </a>
  <a href="https://plantmetwiki.bioinformatics.nl/" style="display:inline-block;padding:12px 28px;background:#4CAF50;color:white;border-radius:8px;text-decoration:none;font-size:16px;margin:8px;">
    PlantMetWiki Webserver
  </a>
</p>

Summary 
---------
Plant Metabolic Pathways Wiki (PlantMetWiki) is an open online portal for querying linked specialized plant pathway information. PlantMetWiki is available in **Semantic Web format** as Resource Description Framework (**RDF**) and can be accessed via an easy-to-use **SNORQL user interface**. **Pre-written SPARQL queries** are available for users to execute or adapt to retrieve pathway information. **Federated queries** with other linked open data tools are supported, thereby expanding the [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page) framework. 


Using the SPARQL Explorer 
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


Output 
---------

Output data is available for download in native RDF format (.ttl), TSV, CSV, and json. 


Resources
---------

* [Introduction to RDF and SPARQL](/Presentation_introRDF.pdf) by BiGCaT Maastricht University

* Wikipathway ontology [The WikiPathways WP Ontology](https://vocabularies.wikipathways.org/)

* [The WikiPathways Semantic Web Portal](https://classic.wikipathways.org/index.php/Portal:Semantic_Web)


Tutorial 
---------
* Understand the Basics
   * [Understanding SPARQL Queries](Assignments/assignment1A.md)
   * [Execute the query and retain results: an example from Wikidata](Assignments/assignment1B.md) --> Need to adapt !? No Wikidata link to PlantMetWiki
   * [Expand and change Query](Assignments/assignment1C.md)
   
* Federated Queries 
   * [A more complicated query](Assignments/assignment2A.md) --> TO DELETE! 
   * [Answering Biological Questions](Assignments/assignment2B.md) --> TO DELETE! 
  
* Additional Analyses 
  * Something about the possibility to do coexpression analyses? 
   
* Recap
   * Other Biological databases with RDF ([presentation](/Presentation_introRDF.pdf))
   * Update your SPARQL query [here](./ParticipantQueries/Example1.md)

An [addendum](Assignments/AddendumBioSb2019.md) is available, where we added:
* Answers to questions asked during the tutorial.
* More information on where to find Biological and Chemical properties (aka relationships) to expand your query.
* More detailed explanation of the SERVICE statement (since this is not directly part of SPARQL, but constructed by Wikidata for easier querying).

License
---------

The material for this tutorial is available under [CC-BY-SA 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/legalcode) licence.

Feedback
---------
If you have feedback on this documentation please submit it as a GitHub issue [issue tracker](https://github.com/pathway-lod/SPARQLTutorials/issues).
