---
layout: docs
title: "Answering Biological Questions on Wikidata"
---

## Changing the Question

In the previous exercises, we were making (small) adjustments, to understand the structure of a SPARQL query (better). 
Now, we will focus on adapting the query to answer aditional questions.

### ?biological_process

Suppose you are interested in not only "cell proliferation" as a GO- term, but you would also want to include genes that 
are related to 'apoptosis' (wd:Q14599311) and 'cell death' (wd:Q2383867). Change the example query to obtain the required results.

(Answers can be found [here](../Answers/AnswersAssignment2.md)). 


### ?drug structure

Suppose you want to obtain the chemical structure of the drugs involved as an InChIKey, if these are available in Wikidata.
Change the example query to obtain the required results.

(Answers can be found [here](../Answers/AnswersAssignment2.md)). 

### ?References

Since there are only 4 known true positives (see the (Un)Comment section), you want to find out if there are any papers
describing these drugs. The results should be ordered based on drugname, and we want to see the article names. 
Change the example query to obtain the required results.

(Answers can be found [here](../Answers/AnswersAssignment2.md)). 


### ?MORE biology

You can probably think of some other questions you would like to ask to Wikidata. Try to come up with a expansion 
of the query we are working on now, or find another example on Wikidata you want to understand and expand 
(ask your instructors for help if needed).

Return to [HOME](https://pathway-lod.github.io/SPARQLTutorials/)
