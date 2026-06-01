---
layout: docs
title: "Modelling Multispecies pathways"
order: 80
---

PlantMetWiki integrates pathway data from **PlantCyc 17.0**, which covers biochemical reactions across **439 plant species** — from *Arabidopsis thaliana* to *Solanum tuberosum*, *Zea mays*, and many non-model species. This section explains how species information is stored and how to query it.

Goals:
1. Understand how multispecies annotation is modelled in PlantMetWiki
2. Retrieve species-specific genes, proteins, and pathways using SPARQL

---

## How species annotation works

In PlantMetWiki, **species annotations are at the DataNode level**, not the pathway level. Each gene or protein DataNode carries a `wp:organism` triple pointing to its NCBI Taxonomy IRI. Every pathway collectively covers Viridiplantae (the green plants clade), but individual genes and enzymes are annotated to the specific organism they come from.

This information is stored across **three named graphs** that need to be joined:

| Named graph | What it contains |
|---|---|
| `graph/pathways` | Core WikiPathways RDF: pathways, DataNodes, interactions |
| `graph/gpml-taxonomy-extra` | Per-DataNode species annotations (`wp:organism`) |
| `graph/ncbitaxon` | NCBITaxon ontology (OBO Foundry, CC0) for label resolution |

**SPARQL endpoint**
```
https://sparql-plantmetwiki.bioinformatics.nl/sparql
```

---

## Query 1 — List all species in PlantMetWiki

Species names are resolved via the local NCBITaxon ontology graph (loaded from the OBO Foundry `ncbitaxon.owl`).

```sparql
PREFIX wp:   <http://vocabularies.wikipathways.org/wp#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ncbi: <http://purl.obolibrary.org/obo/NCBITaxon_>

SELECT DISTINCT ?taxonID ?species
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-taxonomy-extra> {
    ?node wp:organism ?taxonID .
    FILTER(?taxonID != ncbi:33090)  # exclude pathway-level Viridiplantae
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/ncbitaxon> {
    ?taxonID rdfs:label ?species .
  }
}
ORDER BY ?species
```

---

## Query 2 — Count pathways per species

```sparql
PREFIX wp:      <http://vocabularies.wikipathways.org/wp#>
PREFIX rdfs:    <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX ncbi:    <http://purl.obolibrary.org/obo/NCBITaxon_>

SELECT ?species (COUNT(DISTINCT ?pw) AS ?nPathways)
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-taxonomy-extra> {
    ?node wp:organism ?taxonID .
    FILTER(?taxonID != ncbi:33090)
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/pathways> {
    ?node dcterms:isPartOf ?pw .
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/ncbitaxon> {
    ?taxonID rdfs:label ?species .
  }
}
GROUP BY ?species
ORDER BY DESC(?nPathways)
```

---

## Query 3 — All pathways for a given species (by name)

Replace `"Arabidopsis thaliana"` with any Latin species name.

```sparql
PREFIX wp:      <http://vocabularies.wikipathways.org/wp#>
PREFIX rdfs:    <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dc:      <http://purl.org/dc/elements/1.1/>
PREFIX dcterms: <http://purl.org/dc/terms/>

SELECT DISTINCT ?pwID (STR(?titleLit) AS ?title) (COUNT(DISTINCT ?dataNode) AS ?nDataNodes)
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/ncbitaxon> {
    ?taxon rdfs:label "Arabidopsis thaliana" .
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-taxonomy-extra> {
    ?dataNode wp:organism ?taxon .
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/pathways> {
    ?dataNode dcterms:isPartOf ?pw .
    ?pw a wp:Pathway ;
        dc:identifier ?pwID ;
        dc:title ?titleLit .
  }
}
GROUP BY ?pwID ?titleLit
ORDER BY DESC(?nDataNodes)
```

---

## Query 4 — Proteins in pathways for a given taxon ID

Replace `ncbi:4113` with the NCBI Taxonomy ID of your species
(4113 = *Solanum tuberosum* / potato).

```sparql
PREFIX wp:      <http://vocabularies.wikipathways.org/wp#>
PREFIX dc:      <http://purl.org/dc/elements/1.1/>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX ncbi:    <http://purl.obolibrary.org/obo/NCBITaxon_>

SELECT DISTINCT ?protein (STR(?titleLit) AS ?title)
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-taxonomy-extra> {
    ?protein wp:organism ncbi:4113 .
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/pathways> {
    ?protein a wp:Protein ;
             dcterms:isPartOf ?pw .
    ?pw dc:title ?titleLit .
  }
}
ORDER BY ?title
```

---

## Query 5 — Show all species in a specific pathway

Replace `pmw:PC159` with the pathway ID of interest.

```sparql
PREFIX wp:      <http://vocabularies.wikipathways.org/wp#>
PREFIX rdfs:    <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dc:      <http://purl.org/dc/elements/1.1/>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX ncbi:    <http://purl.obolibrary.org/obo/NCBITaxon_>
PREFIX pmw:     <http://rdf-plantmetwiki.bioinformatics.nl/pathways/>

SELECT DISTINCT ?taxonID ?species ?title
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/pathways> {
    ?pw dc:identifier pmw:PC159 ;
        dc:title ?title .
    ?dataNode dcterms:isPartOf ?pw .
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-taxonomy-extra> {
    ?dataNode wp:organism ?taxonID .
    FILTER(?taxonID != ncbi:33090)
  }
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/ncbitaxon> {
    ?taxonID rdfs:label ?species .
  }
}
ORDER BY ?species
```

---

## Taxonomy coverage and known limitations

PlantCyc 17.0 includes species annotations for **439 unique NCBI taxa** across 25,141 DataNodes. When using the local NCBITaxon ontology (OBO Foundry, CC0 licence), 6 taxa are absent from the OBO Foundry release, affecting 54 GeneProduct and Protein nodes (0.2% of annotated nodes). All affected nodes are gene or enzyme annotations — no metabolite nodes are impacted. One taxon (*Lycopersicon hirsutum*, NCBI:283673) is deprecated and merged into *Solanum habrochaites* (NCBI:62890); the remaining five are valid taxa not included in the OBO Foundry release scope.

Use this query to inspect which taxa in your deployment cannot be resolved:

```sparql
PREFIX wp:   <http://vocabularies.wikipathways.org/wp#>
PREFIX ncbi: <http://purl.obolibrary.org/obo/NCBITaxon_>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?taxon (STRAFTER(STR(?taxon), "NCBITaxon_") AS ?ncbiID)
       (COUNT(DISTINCT ?node) AS ?nNodes)
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-taxonomy-extra> {
    ?node wp:organism ?taxon .
    FILTER(?taxon != ncbi:33090)
  }
  FILTER NOT EXISTS {
    GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/ncbitaxon> {
      ?taxon rdfs:label ?label .
    }
  }
}
GROUP BY ?taxon
ORDER BY DESC(?nNodes)
```

---

## Alternative: taxonomy via UniProt federation

If you prefer not to load the NCBITaxon ontology locally, you can federate to the UniProt SPARQL endpoint to resolve taxon labels. UniProt uses a parallel IRI scheme (`http://purl.uniprot.org/taxonomy/4113`) that maps 1:1 to OBO Foundry IRIs (`http://purl.obolibrary.org/obo/NCBITaxon_4113`).

```sparql
PREFIX wp:   <http://vocabularies.wikipathways.org/wp#>
PREFIX up:   <http://purl.uniprot.org/core/>
PREFIX ncbi: <http://purl.obolibrary.org/obo/NCBITaxon_>

SELECT ?taxon ?scientificName
WHERE {
  GRAPH <http://rdf-plantmetwiki.bioinformatics.nl/graph/gpml-taxonomy-extra> {
    ?node wp:organism ?taxon .
    FILTER(?taxon != ncbi:33090)
  }
  BIND(IRI(REPLACE(STR(?taxon),
       "http://purl.obolibrary.org/obo/NCBITaxon_",
       "http://purl.uniprot.org/taxonomy/")) AS ?uniprotTaxon)

  SERVICE <https://sparql.uniprot.org/sparql> {
    ?uniprotTaxon up:scientificName ?scientificName .
  }
}
LIMIT 20
```

**Advantage:** no local ontology required — enriches pathways with taxonomy labels without importing all taxonomy into your database.  
**Disadvantage:** depends on an external endpoint being available; slower for large result sets.

This approach can also answer:
- What scientific name corresponds to this taxon?
- Is this species under Viridiplantae?
- What is the parent taxon?

