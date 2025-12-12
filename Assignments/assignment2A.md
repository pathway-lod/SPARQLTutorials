Assignment 2: Find drugs for cancers that target genes related to cell proliferation
=================

Return to [HOME](https://pathway-lod.github.io/SPARQLTutorials/)

During this assignment, we will investigate another example SPARQL query of Wikidata, called ["Find drugs for cancers that target genes related to cell proliferation"](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Find_drugs_for_cancers_that_target_genes_related_to_cell_proliferation). We will first go through the basics of a SPARQL query. Second, we will find out how to execute the query and retain or share results. Last, we will expand the query and make other (small) changes, to understand the structure of a SPARQL query better, and see what other data is available in Wikidata.

## Step by Step

We will use the following [example](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Find_drugs_for_cancers_that_target_genes_related_to_cell_proliferation), which makes use of Gene Ontology terms in Wikidata and drug information. There are lots of comments written before the actual query starts (which we will ignore for now).

### New building blocks
The complete query is depicted below, and has one new building block, called LIMIT (last line of query). This is a so-called _solution modifier_, which limits the number of rows returned from a query. In the example below, we will receive a maximum of 1000 rows as a result.

#### LIMIT
```SPARQL
SELECT ?drugLabel ?geneLabel ?biological_processLabel ?diseaseLabel
WHERE {
  ?drug wdt:P129 ?gene_product .   # drug interacts with a gene_product
  ?gene wdt:P688 ?gene_product .  # gene_product (usually a protein) is a product of a gene (a region of DNA)
  ?disease	wdt:P2293 ?gene .    # genetic association between disease and gene
  ?disease wdt:P279*  wd:Q12078 .  # limit to cancers wd:Q12078 (the * operator runs up a transitive relation..)
  ?gene_product wdt:P682 ?biological_process . #add information about the GO biological processes that the gene is related to 
  
   ?biological_process (wdt:P361|wdt:P279)* wd:Q14818032 .  # chain down subclass/part-of
   #Change the last statement (wd:Q14818032) to limit to genes related to certain biological processes (and their sub-processes):
  		#cell proliferation wd:Q14818032 (Current example)
                #apoptosis wd:Q14599311

    #uncomment the next line to find a subset of the known true positives (there are not a lot of them in here yet; will lead to 4 drugs if biological process is cell proliferation 2018-12-17)
  #?disease wdt:P2176 ?drug . 	# disease is treated by a drug
  	SERVICE wikibase:label {bd:serviceParam wikibase:language "en" .	}
}
LIMIT 1000
```

There are other _solution modifiers_ available, such as ORDER BY, which can be used to sort the query solutions on the value of one or more variables. Adding ORDER BY ?diseaseLabel above the LIMIT line, will sort our results based on the name of the diseases we are retrieving from this query.

#### SELECT DISTINCT

1. Run the query above in the SPARQL endpoint, and look up how many results you have.
1. Change the _result clause_ to the line depicted below, and look at the amount of results:

```SPARQL
SELECT DISTINCT ?drugLabel ?geneLabel ?biological_processLabel ?diseaseLabel
```

The DISTINCT modifier removes duplicate rows from the query results.

#### (Un)Comment

1. Comments (depicted with a hash '#') can help explaining the query (not only to others, but also to yourself).
1. Within comments, one can also add additional query options, which has been done in the following lines:

```SPARQL
  #uncomment the next line to find a subset of the known true positives (there are not a lot of them in here yet; will lead to 4 drugs if biological process is cell proliferation 2018-12-17)
  #?disease wdt:P2176 ?drug . 	# disease is treated by a drug
```
You can uncomment by removing the '#' sign, and run the query again.

#### Additional visualisations

1. Comment the line related to true positives again (from the assignment above).
1. Change the view from 'Table' to 'Scatter chart':

![Select Scatter Chart](../Images/Scatter_chart_example2.jpg)

The following graph should now appear (click on on of the coloured circles in the graph, to obtain the diseaseLabel):

![Select Scatter Chart](../Images/Scatter_chart_visualisation_example2.JPG)


**Question 1A:** Which variables are depicted in which manner? 

**Question 1B:** What would change to the visualisation, if you switch the place of the variables ?geneLabel and  ?biological_processLabel with one another?

(Answers can be found [here](../Answers/AnswersAssignment2.md)). 

In the [last exercise](../Answers/assignment2B.md) related to this assignment, we will look at expansion options for the query above.

Return to [HOME](https://pathway-lod.github.io/SPARQLTutorials/)
