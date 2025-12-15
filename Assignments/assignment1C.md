---
layout: docs
title: "3. Expand and Change Queries"
prev: "/Assignments/assignment1B"
prev_title: "Previous page"
next: "/Assignments/assignment1D/"
next_title: "Next page"
---

## Change is Coming 

In the previous exercise, we focused on one PlantMetWiki pathway.
In this section, we will explore how to extend a query by including multiple species, and how to make the results more informative.

SPARQL endpoint: https://plantmetwiki.bioinformatics.nl/sparql
Graph used in all queries: FROM <http://plantmetwiki.bioinformatics.nl/>

### More species:

So far, we have implicitly focused on a single species by querying a single pathway.
If we want to explore pathways from multiple species, we can do this by changing the VALUES line in our query.

```sparql 
{ 
	VALUES ?organism { "Solanum tuberosum" }
}
```

This restricts the query to pathways annotated for potato.

### Adding another species 

Adding another species

Suppose we also want to include pathways from Arabidopsis thaliana.
We can expand the VALUES block as follows:


```sparql 
{ 
	VALUES ?organism {
		"Solanum tuberosum"
		"Arabidopsis thaliana"
}
}
```

Now the query will return pathways for both species.

### Full query 

``` 
PREFIX gpml: <http://vocabularies.wikipathways.org/gpml#>

SELECT ?organism ?pathway
FROM <http://plantmetwiki.bioinformatics.nl/>
WHERE {
  VALUES ?organism {
    "Solanum tuberosum"
    "Arabidopsis thaliana"
  }

  ?pathway gpml:organism ?organism .
}
LIMIT 200
```

You should now see more results, coming from more than one species.

### Questions 

<details>
  <summary><strong>How would the VALUES line look if we also want to include <em>Oryza sativa</em>?</strong></summary>

  <p><strong>Answer:</strong><br>
  <code>
  VALUES ?organism {<br>
  &nbsp;&nbsp;"Solanum tuberosum"<br>
  &nbsp;&nbsp;"Arabidopsis thaliana"<br>
  &nbsp;&nbsp;"Oryza sativa"<br>
  }
  </code>
  </p>
</details>

### Which species?

Since we are now retrieving pathways from multiple species, it is useful to explicitly show the species in the results.
To do this, we modify the SELECT clause so that the organism is visible:

```sparql 
SELECT ?organism ?pathway
```
If we also want to include the pathway name (when available), we can extend this further:

```sparql 
SELECT ?organism ?pathway ?pathwayName
```

And add the corresponding triple pattern:
```
OPTIONAL { ?pathway gpml:name ?pathwayName }
```

### Updated query with pathway names 
```
PREFIX gpml: <http://vocabularies.wikipathways.org/gpml#>

SELECT ?organism ?pathway ?pathwayName
FROM <http://plantmetwiki.bioinformatics.nl/>
WHERE {
  VALUES ?organism {
    "Solanum tuberosum"
    "Arabidopsis thaliana"
  }

  ?pathway gpml:organism ?organism .
  OPTIONAL { ?pathway gpml:name ?pathwayName }
}
LIMIT 200
```

### Questions 

<details>
  <summary><strong>Which variable adds the species name to the results?</strong></summary>
  <p><strong>Answer:</strong><br>
  <code>?organism</code>, filled via <code>?pathway gpml:organism ?organism</code>
  </p>
</details>

### Easier querying: discovering species in PlantMetWiki

Unlike Wikidata, PlantMetWiki does not require numeric identifiers (such as Q-numbers).
Species names are stored directly as literals.

If you are not sure which species are present in the database, you can list them:

```
PREFIX gpml: <http://vocabularies.wikipathways.org/gpml#>

SELECT DISTINCT ?organism
FROM <http://plantmetwiki.bioinformatics.nl/>
WHERE {
  ?pathway gpml:organism ?organism .
}
ORDER BY ?organism
``` 

This query gives you a controlled vocabulary of species that you can copy directly into a VALUES block.

### Small expansion: count pathways per species

We can also aggregate results to answer questions such as:

Which species have the most pathways in PlantMetWiki?

```
PREFIX gpml: <http://vocabularies.wikipathways.org/gpml#>

SELECT ?organism (COUNT(DISTINCT ?pathway) AS ?nPathways)
FROM <http://plantmetwiki.bioinformatics.nl/>
WHERE {
  ?pathway gpml:organism ?organism .
}
GROUP BY ?organism
ORDER BY DESC(?nPathways)
``` 

### Notes on visualization

Unlike Wikidata, the PlantMetWiki SPARQL endpoint does not provide built-in image visualizations.

However, you can:
	•	export results as tables
	•	click through to PlantCyc reaction links (as shown in Assignment 1)
	•	use external tools (e.g. notebooks, R, Python) to visualize pathway statistics


#### Adding protein images on Wikidata 
The SPARQL endpoint of Wikidata has several interesting data visualisation options; we will use one to add protein domain images for the genes we just queried.
1. Just above the SERVICE element, add the following line:

```SPARQL
?gene wdt:P18 ?image .
```
2. Click on the play button... What just happened? We had 13 variants, and now the results went down to 6?!

Since not all genes have an image in Wikidata, we are only retrieving the ones that have an image. This can be avoided by using an OPTIONAL statement, such as:
```SPARQL
OPTIONAL{?gene wdt:P18 ?image }.
```

However, we are not seeing the images in our results panel. Every time we want to see a variable that we are querying, we need to add it to the SELECT statement.
1. Change the SELECT statement to the following:
```SPARQL
SELECT ?geneLabel ?variantLabel ?diseaseLabel ?image
```
2. Click on the play button... We do not see the images directly, we do get a link to the images in a Table. If we want to actually see the images, we need to change the visualisation options of the SPARQL endpoint. 
3. Directly under the Run button, there is an option called 'Table". Click on this option, and select the option "Image Grid":

![Image Grid query 1](/Images/Image_grid_Wikidata.jpg)

Now, the genes in Wikidata which have an image connected to them, are displayed.

![Image example query 1](/Images/Images_genes_Wikidata.JPG)

If you would like to have images for all the genes you queried, you can add these to Wikidata yourself. Since the data in Wikidata is built by community efforts, everyone can get involved. If you would like to know more about becoming a database editor and/or curator for Wikidata, ask one of the instructors for more information.

## Next assignments:

To continue, you can do one of the following:
1. Progress to [Assignment 2](/Assignments/assignment2A.md), where we will discuss another query in more detail
1. Stay with the current query to adapt it to your own needs. Several example questions to work on are given in this [additional assignment](/Assignments/assignment1D.md).

Return to [HOME](https://pathway-lod.github.io/SPARQLTutorials/)

