---
layout: docs
title: "Diving into Natural Products"
order: 60 
---

## A molecule-centric example using castor oil components

Natural products research often starts from the molecule, not from the pathway.

For many compounds of industrial, nutritional, or toxicological relevance, researchers want to understand:

	•	Which plants produce this compound?
	•	In which biosynthetic pathways does it occur?
	•	What biochemical context surrounds its production?
	•	What chemical knowledge already exists about it?

However, this information is typically fragmented across:

	•	pathway databases,
	•	organism-specific studies,
	•	chemistry resources,
	•	and the scientific literature.

Plant Metabolic Pathways Wiki (PlantMetWiki) addresses this fragmentation by modeling plant metabolism as linked open data, enabling researchers to start from a molecule and traverse seamlessly across pathways, species, and external chemical knowledge bases.

In this tutorial, we demonstrate a molecule-centric workflow using components of castor oil as a case study.

## Research question

> How can plant biosynthetic pathways and producing species be identified starting from natural product molecules, and how can this knowledge be enriched with external chemical information using linked open data?

This directly reflects the PlantMetWiki goal of connecting biochemistry, biosynthesis, and chemistry.

## Case study: natural products in castor oil

Castor oil, derived primarily from Ricinus communis, contains a mixture of fatty acids and specialized metabolites with important industrial applications.

Rather than starting from a known pathway, we begin from specific molecules of interest, identified by their InChIKeys, and ask:

> Where do these molecules occur in plant metabolism, and what is known about them?

## Step 1 — Identify pathways and species associated with molecules of interest

We first ask:

Which plant pathways and species contain specific natural product molecules related to castor oil?

SPARQL query
```sparql 
PREFIX dc:       <http://purl.org/dc/elements/1.1/>
PREFIX dcterms:  <http://purl.org/dc/terms/>
PREFIX rdf:      <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs:     <http://www.w3.org/2000/01/rdf-schema#>
PREFIX wp:       <http://vocabularies.wikipathways.org/wp#>

SELECT DISTINCT ?title
       (GROUP_CONCAT(DISTINCT ?species; separator=", ") AS ?Species)
       ?inchikey ?wd ?wdLabel
FROM <http://plantmetwiki.bioinformatics.nl/>
WHERE {
  ?metabolite a wp:Metabolite ;
              dcterms:isPartOf ?pw ;
              dc:source "InChIKey" ;
              dcterms:identifier ?inchikey .

  FILTER(?inchikey IN (
    "BGWGXPAPYGQALX-ARQDHWQXSA-L",
    "CZMRCDWAGMRECN-UGDNZRGBSA-N",
    "ASKKPQKSCFYPPP-FVLDFCIYSA-J"
  ))

  ?pw dc:title ?title ;
      wp:organismName ?species .

  SERVICE <https://qlever.cs.uni-freiburg.de/api/wikidata> {
    ?wd <http://www.wikidata.org/prop/direct/P235> ?inchikey .
    OPTIONAL {
      ?wd rdfs:label ?wdLabel .
      FILTER(LANG(?wdLabel) = "en")
    }
  }
}
ORDER BY ?wd
``` 

## Step 2 — Interpret the results

This query returns:
	•	Pathways in which the selected molecules occur
	•	Plant species associated with those pathways
	•	Chemical identifiers and labels retrieved from Wikidata

Key observation

Starting from only chemical identifiers (InChIKeys), PlantMetWiki enables the identification of plant pathways and species involved in the biosynthesis or utilization of these molecules.

This demonstrates the reverse traversal of classical pathway analysis:
	•	molecule → pathway → species


## Step 3 — Linking molecules to broader biochemical context

Because metabolites in PlantMetWiki are part of explicitly modeled pathways, we can now place individual molecules into a biochemical and biological context, including:

	•	upstream and downstream reactions,
	•	shared substrates and products,
	•	related metabolites across pathways.

This allows researchers to move beyond isolated compound lists and reason about biosynthetic logic.

## Step 4 — Enriching natural products with external chemical knowledge

PlantMetWiki supports federated SPARQL queries, enabling live integration with external resources such as Wikidata.

This allows us to retrieve additional chemical annotations (e.g. ingredient classifications, identifiers) without duplicating data.

Example: linking metabolites to Wikidata ingredient information

```sparql
PREFIX dc:       <http://purl.org/dc/elements/1.1/>
PREFIX dcterms:  <http://purl.org/dc/terms/>
PREFIX rdf:      <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs:     <http://www.w3.org/2000/01/rdf-schema#>
PREFIX wp:       <http://vocabularies.wikipathways.org/wp#>

SELECT ?pwID
       (GROUP_CONCAT(DISTINCT ?species; separator=", ") AS ?Species)
       ?name ?wd ?ingredientLabel
FROM <http://plantmetwiki.bioinformatics.nl/>
WHERE {
  ?metabolite a wp:Metabolite ;
              rdfs:label ?name ;
              dcterms:isPartOf ?pw ;
              dc:source "InChIKey" ;
              dcterms:identifier ?inchikey .

  FILTER(?name IN (
    "caffeine",
    "theobromine",
    "xanthine",
    "7-methylxanthine"
  ))

  ?pw a wp:Pathway ;
      dc:identifier ?pwID ;
      wp:organismName ?species .

  SERVICE <https://qlever.cs.uni-freiburg.de/api/wikidata> {
    ?wd <http://www.wikidata.org/prop/direct/P235> ?inchikey .
    OPTIONAL {
      ?wd <http://www.wikidata.org/prop/direct/P3780> ?ingredient .
      ?ingredient rdfs:label ?ingredientLabel .
      FILTER(LANG(?ingredientLabel) = "en")
    }
  }
}
ORDER BY ?name
``` 

## Step 5 — From molecules to hypotheses

This molecule-centric approach enables new research questions, such as:

	•	Which plant species produce chemically similar compounds?
	•	Are related molecules produced via shared or divergent pathways?
	•	Can missing biosynthetic steps be inferred from chemically related compounds in other species?
	•	How do chemical classifications align with biosynthetic pathway structure?

Because PlantMetWiki integrates biosynthesis, species, and chemistry as linked data, these questions can be addressed programmatically.

## Takeaways 

This example illustrates how PlantMetWiki supports natural product-driven hypothesis generation:

By starting from molecular identifiers, researchers can traverse plant metabolic knowledge across pathways, species, and external chemical databases, enabling integrative analysis of biosynthesis and chemistry.

This workflow is particularly powerful for:

	•	natural products discovery,
	•	food and ingredient research,
	•	toxicology,
	•	comparative metabolomics.


## Summary

In this tutorial, we demonstrated how PlantMetWiki can be used to:

	•	Start from natural product molecules
	•	Identify associated pathways and producing species
	•	Integrate biosynthetic context with chemical knowledge
	•	Enrich results via federated queries to Wikidata

Together with pathway-centric analyses, this molecule-centric perspective highlights the flexibility and power of PlantMetWiki as a linked open data platform for plant metabolism.