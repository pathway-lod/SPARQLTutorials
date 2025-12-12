1 - Understanding SPARQL Queries on PlantMetWiki
=================

[HOME](https://pathway-lod.github.io/SPARQLTutorials/)

During this assignment, we will have a closer look at an example SPARQL query. 

1. We will go through the basics of a SPARQL query. 
2. We will execute the query and learn how to interpret results.
3. We will expand the query with small changes to explore more data, including linking to PlantCyc.

SPARQL endpoint: https://plantmetwiki.bioinformatics.nl/sparql
Graph used in all queries: FROM <http://plantmetwiki.bioinformatics.nl/>

## What goes Where

A SPARQL query consist out of several elements, which can be considered as building blocks. 

Our PlantMetWiki question

Which reactions (PlantCyc reactions) are in a given PlantMetWiki pathway, and how can we click through to PlantCyc to validate them?

We will use this pathway URI throughout:

```
<http://rdf.plantmetwiki.bioinformatics.nl/Pathway/RC1000_r20251206224344>
```

### First element: SELECT

The SELECT clause defines what will be returned as results.

For our question, we want:
	•	the reaction identifier (?reactionId)
	•	a clickable PlantCyc link (?plantCycReactionURL)

```sparql 
SELECT ?reactionId ?plantCycReactionURL
```

SELECT is used to indicate with variables from the (to follow) SPARQL query you want to visualise as a result (in other words: which variables we find relevant as output to answer our biological question). 

### Second element: WHERE

The second element we encouter, is the _query pattern_, which starts with the word WHERE, with the query itself enclosed in curly brackets: {} .

The WHERE clause defines the graph pattern to match (triples in the form subject–predicate–object).

For PlantMetWiki pathways, we already discovered the key predicates:
	•	gpml:hasInteraction (links a pathway to interactions)
	•	some interactions represent real PlantCyc reactions (e.g. RXN-10730)
	•	some interactions are GPML anchor helper nodes (contain _anchor_) and should not be linked to PlantCyc

```sparql 
WHERE {
  VALUES ?pathway { <...> }
  ?pathway gpml:hasInteraction ?interaction .
  ...
}
```
This is a set of RDF triples (subject–predicate–object), just like in the Wikidata tutorial, but with PlantMetWiki predicates.


#### Line-by-line explanation (PlantMetWiki version)

##### Line 1 — VALUES (what are we querying about?)

VALUES lets us “pin” the query to one (or multiple) specific items.

```sparql
VALUES ?pathway {
  <http://rdf.plantmetwiki.bioinformatics.nl/Pathway/RC1000_r20251206224344>
}
``` 
You can add more pathways inside the braces later (separated by spaces) if you want to compare multiple pathways.

##### Line 2 — Retrieve interactions from the pathway

This line uses the pathway as the subject and gets all linked interactions:

```sparql
?pathway gpml:hasInteraction ?interaction .
```

##### Line 3 — Turn an interaction URI into a PlantCyc reaction link

PlantMetWiki does not use Wikidata’s label service. Instead, we often extract meaningful identifiers from URIs.

1. Extract the part after /Interaction/:

```
BIND(STRAFTER(STR(?interaction), "/Interaction/") AS ?reactionId)
```

2. Keep only “real” reactions and exclude anchor helper nodes:

```
FILTER(CONTAINS(?reactionId, "RXN-"))
FILTER(!CONTAINS(?reactionId, "_anchor_"))
```

3. Construct a clickable PlantCyc URL:

```
BIND(
  IRI(CONCAT(
    "https://pmn.plantcyc.org/PLANT/NEW-IMAGE?type=REACTION&object=",
    ?reactionId
  )) AS ?plantCycReactionURL
)
```

#### Full query 

```sparql 
PREFIX gpml: <http://vocabularies.wikipathways.org/gpml#>

SELECT ?reactionId ?plantCycReactionURL
FROM <http://plantmetwiki.bioinformatics.nl/>
WHERE {
  VALUES ?pathway {
    <http://rdf.plantmetwiki.bioinformatics.nl/Pathway/RC1000_r20251206224344>
  }

  ?pathway gpml:hasInteraction ?interaction .

  BIND(STRAFTER(STR(?interaction), "/Interaction/") AS ?reactionId)

  # Only keep real PlantCyc reactions, not GPML helper anchors
  FILTER(CONTAINS(?reactionId, "RXN-"))
  FILTER(!CONTAINS(?reactionId, "_anchor_"))

  BIND(
    IRI(CONCAT(
      "https://pmn.plantcyc.org/PLANT/NEW-IMAGE?type=REACTION&object=",
      ?reactionId
    )) AS ?plantCycReactionURL
  )
}
ORDER BY ?reactionId
LIMIT 200
```

### Questions 
<details>
  <summary><strong>Question 1: Which part of the query selects the pathway we want to investigate?</strong></summary>
  <p><strong>Answer:</strong><br>
  <code>VALUES ?pathway { &lt;http://rdf.plantmetwiki.bioinformatics.nl/Pathway/RC1000_r20251206224344&gt; }</code>
  </p>
</details>

<details>
  <summary><strong>Question 2: Which line retrieves all interactions that belong to the pathway?</strong></summary>
  <p><strong>Answer:</strong><br>
  <code>?pathway gpml:hasInteraction ?interaction .</code>
  </p>
</details>

<details>
  <summary><strong>Question 3: Why do we filter out <code>_anchor_</code> interactions?</strong></summary>
  <p><strong>Answer:</strong><br>
  Interactions that contain <code>_anchor_</code> are GPML helper nodes used for drawing/connecting edges. They are not real PlantCyc reaction identifiers, so PlantCyc will not recognize them.
  </p>
</details>

#### Small expansion: also list DataNodes (metabolites/genes) in the pathway

If you want to see which entities are present in the same pathway:

```
PREFIX gpml: <http://vocabularies.wikipathways.org/gpml#>

SELECT ?dataNode ?dataNodeId
FROM <http://plantmetwiki.bioinformatics.nl/>
WHERE {
  VALUES ?pathway {
    <http://rdf.plantmetwiki.bioinformatics.nl/Pathway/RC1000_r20251206224344>
  }

  ?pathway gpml:hasDataNode ?dataNode .
  BIND(STRAFTER(STR(?dataNode), "/DataNode/") AS ?dataNodeId)
}
ORDER BY ?dataNodeId
LIMIT 200
```

#### Notes on labels (why there is no SERVICE clause)

Notes on labels (why there is no SERVICE clause)

Wikidata uses a special label service (SERVICE wikibase:label) to fetch human-readable labels for identifiers.

PlantMetWiki does not use this service. Instead:
	•	some properties already store readable text (e.g. gpml:name, gpml:organism, gpml:textLabel)
	•	otherwise we often extract readable identifiers from URIs (using STRAFTER() as shown above)


Return to [HOME](https://pathway-lod.github.io/SPARQLTutorials/)