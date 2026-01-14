---
layout: docs
title: "Resolving Biosynthetic Pathways Across Species"
order: 50 
---

# A multispecies example using capsaicin biosynthesis in Capsicum

## Background

Plant specialized metabolic pathways are often incompletely characterized in individual species. While a biosynthetic pathway may be experimentally resolved in one species, annotations in closely related species are frequently partial, missing reactions, enzymes, or regulatory steps.

This fragmentation limits comparative analysis, pathway reconstruction, and hypothesis generation.

Plant Metabolic Pathways Wiki (PlantMetWiki) addresses this challenge by modeling plant biosynthetic pathways as multispecies, linked open data objects, enabling cross-species integration of biochemical knowledge.

In this tutorial, we demonstrate how PlantMetWiki can be used to resolve pathway annotations across related species by leveraging shared reactions, substrates, and products.


## Research question

How can **incomplete biosynthetic pathway annotations** in plant species be resolved by leveraging shared biochemical reactions and metabolites across related species using linked open data?

We explore this question using the capsaicin biosynthesis pathway as a case study across Capsicum species.

## Case study: capsaicin biosynthesis in Capsicum

Capsaicin is a specialized metabolite responsible for pungency in chili peppers (Capsicum spp.). While capsaicin biosynthesis is well studied in some species (e.g. Capsicum annuum), pathway annotations across the genus are uneven.

PlantMetWiki models capsaicin biosynthesis as a single pathway that can be associated with multiple species, allowing comparative inspection.

## Step 1 — Identify species associated with a pathway

We start by asking:

> Which plant species are associated with the capsaicin biosynthesis pathway in PlantMetWiki?

SPARQL query

```sparql 
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX wp: <http://vocabularies.wikipathways.org/wp#>
PREFIX pmw: <http://rdf-plantmetwiki.bioinformatics.nl/pathways/>

SELECT ?species ?taxonID ?title
FROM <http://plantmetwiki.bioinformatics.nl/>
WHERE {
  ?pw rdf:type wp:Pathway ;
      dc:identifier pmw:PC159 ;
      wp:organismName ?species ;
      wp:organism ?taxonID ;
      dc:title ?title .
}
``` 

Run the query in PlantMetWiki 

## Step 2 — Interpret the results
  

  The query returns multiple Capsicum species associated with the same pathway:

	•	Capsicum annuum
	•	Capsicum chinense
	•	Capsicum frutescens
	•	Capsicum (genus-level annotations)

All are linked to capsaicin biosynthesis, but with different NCBI Taxon identifiers, reflecting species- or clade-level annotations.

## Key observation

> The same biosynthetic pathway is associated with multiple related species, even though species-specific pathway annotations differ in completeness.

This reflects a core design principle of PlantMetWiki:

	•	pathways are modeled independently of species
	•	species associations can be partial or evolving


## Step 3 — Resolving Pathways across species 

The presence of a pathway annotation does not imply that:

	•	all reactions are experimentally validated in every species
	•	all enzymes or genes are known for each species

Instead, PlantMetWiki enables cross-species inference:

If a reaction, substrate, or product is known in one Capsicum species, it can serve as a hypothesis for unresolved steps in another closely related species.

This is particularly powerful for:

	•	specialized metabolism
	•	lineage-specific pathways
	•	species with limited experimental characterization

## Step 4 — Generating hypotheses using shared pathway structure

Using PlantMetWiki, researchers can now ask follow-up questions such as:
	•	Which reactions are shared across Capsicum species?
	•	Which enzymes are present in one species but missing in another?
	•	Are missing steps supported by shared substrates or products?
	•	Do predicted biosynthetic gene clusters (from plantiSMASH) overlap with these pathways?

Because all information is represented as linked open data, these questions can be answered by extending the query or combining it with:
	•	gene annotations
	•	biosynthetic gene cluster predictions
	•	metabolite identifiers
	•	external resources (e.g. Wikidata, LOTUS)

## Takeaway 

This example illustrates how PlantMetWiki supports hypothesis-driven pathway reconstruction:

Incompletely annotated biosynthetic pathways in one plant species can be resolved by leveraging biochemical knowledge from related species that share pathway structure and metabolites.

Rather than treating missing annotations as gaps, PlantMetWiki enables researchers to reason across species boundaries using shared biochemical context.

In this tutorial, we showed how PlantMetWiki can be used to:

	•	Query multispecies pathway annotations
	•	Identify related species sharing a biosynthetic pathway
	•	Interpret pathway presence in the context of incomplete annotations
	•	Generate hypotheses for resolving missing pathway steps

This multispecies, linked-data approach is central to advancing the discovery and characterization of plant specialized metabolism.