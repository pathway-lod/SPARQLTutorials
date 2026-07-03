---
layout: docs
title: "Resolving Biosynthetic Pathways Across Species"
order: 50 
---

# A multispecies example using capsaicin biosynthesis in Capsicum

## Background

Plant specialised metabolic pathways are often incompletely characterised in individual species. While a biosynthetic pathway may be experimentally resolved in one species, annotations in closely related species are frequently partial, missing reactions, enzymes, or regulatory steps.

This fragmentation limits comparative analysis, pathway reconstruction, and hypothesis generation.

Plant Metabolic Pathways Wiki (PlantMetWiki) addresses this challenge by modelling plant biosynthetic pathways as multispecies, linked open data objects, enabling cross-species integration of biochemical knowledge.

In this tutorial, we demonstrate how PlantMetWiki can be used to resolve pathway annotations across related species by leveraging shared reactions, substrates, and products.

## Research question

How can **incomplete biosynthetic pathway annotations** in plant species be resolved by leveraging shared biochemical reactions and metabolites across related species using linked open data?

We explore this question using the capsaicin biosynthesis pathway as a case study across *Capsicum* species.

## Case study: capsaicin biosynthesis in Capsicum

Capsaicin is a specialised metabolite responsible for pungency in chilli peppers (*Capsicum* spp.). While capsaicin biosynthesis is well studied in some species (e.g. *Capsicum annuum*), pathway annotations across the genus are uneven.

PlantMetWiki models capsaicin biosynthesis as pathway **PC629** (PlantCyc stable ID: **PWY-5710**). This pathway can be associated with multiple *Capsicum* species, allowing comparative inspection.

## Step 1 — Identify species associated with the capsaicin biosynthesis pathway

We start by asking:

> Which plant species are associated with the capsaicin biosynthesis pathway in PlantMetWiki?

In PlantMetWiki, species are stored at **data-node level** (not on the pathway node itself). We therefore join three named graphs:
1. `graph/pathways` — to find data nodes that belong to the capsaicin pathway
2. `graph/gpml-taxonomy-extra` — to get the taxon IRI for each data node
3. `graph/ncbitaxon` — to get a readable species name from the taxon IRI

```sparql
PREFIX wp:      <http://vocabularies.wikipathways.org/wp#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX dc:      <http://purl.org/dc/elements/1.1/>
PREFIX rdfs:    <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ncbi:    <http://identifiers.org/taxonomy/>

SELECT DISTINCT ?species ?taxon
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/pathways> {
    ?node dcterms:isPartOf ?pw .
    FILTER(CONTAINS(STR(?pw), '/PC629_'))
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-taxonomy-extra> {
    ?node wp:organism ?taxon .
    FILTER(?taxon != ncbi:33090)
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/ncbitaxon> {
    ?taxon rdfs:label ?species .
  }
}
ORDER BY ?species
```

Run the query in PlantMetWiki.

## Step 2 — Interpret the results

The query returns *Capsicum* species associated with the capsaicin biosynthesis pathway, for example:

- *Capsicum annuum*
- *Capsicum chinense*
- *Capsicum frutescens*
- Genus-level *Capsicum* annotations

All are linked to the same pathway, but with different NCBI Taxon identifiers reflecting species- or clade-level annotations.

The `FILTER(?taxon != ncbi:33090)` line excludes the *Viridiplantae* (NCBI:33090) placeholder, which is used as a pathway-level default and does not represent a real species annotation.

## Key observation

> The same biosynthetic pathway is associated with multiple related species, even though species-specific pathway annotations differ in completeness.

This reflects a core design principle of PlantMetWiki:

- pathways are modelled independently of species
- species associations can be partial or evolving

## Step 3 — Look up the pathway using its stable PlantCyc ID

PlantMetWiki also stores the stable **PlantCyc PWY ID** in a dedicated graph (`graph/gpml-properties-extra`). This lets you find the pathway even if its internal version ID changes:

```sparql
PREFIX wp:      <http://vocabularies.wikipathways.org/wp#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX dc:      <http://purl.org/dc/elements/1.1/>

SELECT ?pw ?title
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-properties-extra> {
    ?pw dcterms:identifier "PWY-5710" .
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/pathways> {
    ?pw dc:title ?title .
  }
}
```

## Step 4 — Which reactions are covered by which Capsicum species?

We can now ask which individual reactions within the capsaicin pathway are annotated for each *Capsicum* species. This reveals where coverage is complete and where gaps remain.

```sparql
PREFIX wp:      <http://vocabularies.wikipathways.org/wp#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX rdfs:    <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ncbi:    <http://identifiers.org/taxonomy/>

SELECT DISTINCT ?species ?reactionId
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/pathways> {
    ?pw a wp:Pathway .
    FILTER(CONTAINS(STR(?pw), '/PC629_'))

    ?rxn dcterms:isPartOf ?pw ;
         a wp:Conversion .
    FILTER(!CONTAINS(STR(?rxn), "_anchor_"))

    BIND(REPLACE(STRAFTER(STR(?rxn), "/Interaction/"), "_", "-") AS ?reactionId)

    # Find enzymes catalysing this reaction
    ?catalysis a wp:Catalysis ;
               dcterms:isPartOf ?pw ;
               wp:source ?enzyme ;
               wp:target ?rxn .
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-taxonomy-extra> {
    ?enzyme wp:organism ?taxon .
    FILTER(?taxon != ncbi:33090)
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/ncbitaxon> {
    ?taxon rdfs:label ?species .
  }
  FILTER(CONTAINS(?species, "Capsicum"))
}
ORDER BY ?species ?reactionId
```

This query connects reactions to species via the enzyme nodes: a reaction is "covered" by a species if at least one enzyme catalysing that reaction is annotated for that species.

## Step 5 — Resolving pathways across species

The presence of a pathway annotation does not imply that:

- all reactions are experimentally validated in every species
- all enzymes or genes are known for each species

Instead, PlantMetWiki enables cross-species inference:

If a reaction, substrate, or product is known in one *Capsicum* species, it can serve as a hypothesis for unresolved steps in another closely related species.

This is particularly powerful for:

- specialised metabolism
- lineage-specific pathways
- species with limited experimental characterisation

## Step 6 — Generating hypotheses using shared pathway structure

Using PlantMetWiki, researchers can now ask follow-up questions such as:
- Which reactions are shared across *Capsicum* species?
- Which enzymes are present in one species but missing in another?
- Are missing steps supported by shared substrates or products?
- Do predicted biosynthetic gene clusters (from plantiSMASH) overlap with these pathways?

Because all information is represented as linked open data, these questions can be answered by extending the queries above or combining them with:
- gene annotations
- biosynthetic gene cluster predictions (Tutorial 3)
- metabolite identifiers
- external resources (e.g. Wikidata, LOTUS) (Tutorial 4)

## Takeaway

This example illustrates how PlantMetWiki supports hypothesis-driven pathway reconstruction:

Incompletely annotated biosynthetic pathways in one plant species can be resolved by leveraging biochemical knowledge from related species that share pathway structure and metabolites.

Rather than treating missing annotations as gaps, PlantMetWiki enables researchers to reason across species boundaries using shared biochemical context.

In this tutorial, we showed how PlantMetWiki can be used to:

- Query multispecies pathway annotations using the correct multi-graph pattern
- Identify related species sharing a biosynthetic pathway via data-node–level taxon annotations
- Use stable PlantCyc IDs (PWY-5710) to locate pathways robustly
- Cross-tabulate reaction coverage by species to identify annotation gaps
- Generate hypotheses for resolving missing pathway steps

This multispecies, linked-data approach is central to advancing the discovery and characterisation of plant specialised metabolism.

## Summary of queries in this tutorial

| Query | Graphs used |
|---|---|
| Species in capsaicin pathway | pathways + gpml-taxonomy-extra + ncbitaxon |
| Lookup by stable PWY ID | gpml-properties-extra + pathways |
| Reaction coverage by species | pathways + gpml-taxonomy-extra + ncbitaxon |
