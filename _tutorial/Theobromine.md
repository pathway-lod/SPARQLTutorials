---
layout: docs
title: "Theobromine and Federated queries"
order: 70 
---

## From Chemistry to Biosynthesis: Exploring Theobromine with PlantMetWiki

Plant specialized metabolites are often chemically well known across many organisms, while their biosynthesis is only resolved in a handful of species.

In this tutorial, we explore theobromine, a purine alkaloid best known from cocoa and tea, to show how PlantMetWiki connects biosynthetic knowledge with chemical knowledge using federated queries.

We will combine: PlantMetWiki (biosynthetic pathways, genes, enzymes from curated data from PlantCyc/PMN) and Wikidata (chemical properties, uses, species distribution). 


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

In this tutorial, we demonstrate a molecule-centric workflow using components of theobromine as a case study.


## Research question

> How can plant biosynthetic pathways and producing species be identified starting from natural product molecules, and how can this knowledge be enriched with external chemical information using linked open data?

This directly reflects the PlantMetWiki goal of connecting biochemistry, biosynthesis, and chemistry.

# Example application : Breeding for naturally sweet (and Dog-safe!) chocolate! 

Theobromine is one of the key alkaloids that give chocolate its characteristic bitterness and stimulant properties. But it’s also the main reason chocolate is toxic to dogs, who metabolize it much more slowly than humans.

Without theobromine, chocolate becomes naturally sweeter and safe for dogs! By tweaking theobromine synthesis with plant engineering we could breed cacao varieties that keep the nice flavor profile, but reduce theobromine to levels for sweet dog-safe cacao. 

Here we show how PlantMetWiki can support integrating information from toxicology, chemistry, and biosynthesis to identify breeding targets for reduced theobromine content. 


## What is the InChIKey for theobromine? 

First, let's get the InChIKey for theobromine which uniquely identifies theobromine as a compound and can be used for follow up federated queries with other databases. 

[SPARQL query](https://bit.ly/3O3PeKF)

## Chemistry coverage: In how many species is theobromine found? 

Use the InChIKey you got in the previous query to query Wikidata to find the taxa and species where theobromine is found. 

[SPARQL query](https://bit.ly/4k5iDAq)

What are the taxa and species names that have theobromine?  

[SPARQL query](https://edu.nl/uaff4)

## Biosynthesis coverage: how many PlantMetWiki species have a pathway containing theobromine

In how many species is the pathway for theobromine been experimentally characterised? 

[SPARQL query](https://bit.ly/49WS6R1)

We see that even if theobromine is chemically known to be present in 38 species, its pathway is only known in 7 species.  

> We see a gap between chemical and biosynthetic characterization of theobromine across species. This shows that PlantMetWiki can help link biosynthesis to biochemistry to solve gaps in knowledge about plant metabolism. 

What are the PlantCyc pathways and species in PlantWiki that contain theobromine? 

[SPARQL query](https://bit.ly/4pZ8p5W)

## Industrial/drug identifiers DrugBank, ChEMBL, ChEBI, PubChem from Wikidata : anchors in the chemical universe 

Now, we use the lookup of the theobromine InChIkey on Wikidata to get the identifiers from chemical and drug databases for theobromine. 

[SPARQL query](https://edu.nl/3p6uc)

This shows how it’s a well-characterized small molecule. Gives you canonical IDs to link to toxicity databases (DSSTox) and cheminformatics tools, if you want to simulate analogues or structure–activity relationships.

> These identifiers let us connect theobromine to toxicology dashboards (DSSTox/CompTox), regulatory databases, and medicinal chemistry resources. 

In our breeding example application, thanks to these databases, we can quantify how much theobromine is too much for dogs and humans.

Once we have these identifiers, theobromine stops being “just a node in Wikidata” and becomes a portal into many different data worlds. Each ID suggests a different line of investigation:

1.	Toxicology and safety: how dangerous is it, and for whom?

        Use the PubChem and ChEBI IDs to jump into:

        •	LD₅₀ data
        •	reported adverse effects
        •	species-specific toxicity reports
        •	Follow the DSSTox/CompTox links (via IDs we get in a later query) to:
        •	compare dog vs human sensitivity
        •	identify safe exposure ranges
        
        In the breeding story, this tells us what target concentration we need to stay below in cacao beans to meaningfully reduce risk for dogs.

2.	Pharmacology and mechanism: what does theobromine actually do?

        Use the DrugBank and ChEMBL IDs to:

        •	see known targets (receptors, enzymes)
        •	collect assay data (IC₅₀, EC₅₀, Ki, etc.)
        •	check which indications (diseases/conditions) it has been studied for.
    
        This helps explain the “subject has role” annotations we saw (vasodilator, bronchodilator…) and shows which biological pathways might be perturbed if we strongly decrease theobromine in plants.

3.	Chemical neighbourhood: what are the “cousins” of theobromine? 

        With ChEBI, KEGG, HMDB, PubChem:

        •	find structurally related methylxanthines (caffeine, paraxanthine, 7-methylxanthine, etc.)
        •	inspect which ones are less bitter or less toxic, but still contribute desirable flavor or physiological effects.
        •	This opens a design question:
    
        Can we push flux away from theobromine and towards a safer, less bitter analogue?

4.	Link back to biosynthesis: which genes control its levels in plants?

	    Once we know which derivatives exist, we can:
        •	use PlantMetWiki to find reactions that produce or consume theobromine
        •	list the genes and enzymes responsible
        •	compare them across species that accumulate different methylxanthine profiles.
        
        In the breeding scenario, these genes become markers in genomic selection and candidates for gene editing to tweak methylxanthine balance.

5.	Literature and evidence base: what do recent papers say?

	    Using these IDs (especially PubChem, ChEBI, DrugBank), we can:

        •	query PubMed or Europe PMC for recent works mentioning theobromine
        •	filter by topics like toxicity, metabolism in dogs, flavor chemistry, plant breeding.

	    This lets us validate which of the hypotheses from our graph exploration are already tested, and which remain open research questions.
    

## Toxicity profile of theobromine : is theobromine a toxic compound? 

> Is theobromine registered as a toxic substance? These links let us inspect detailed toxicity data (e.g. species-specific LD₅₀). 

Use your DSSTox federated query: DSSTox substance and compound IDs → link to EPA CompTox dashboard. CompTox then provides LD₅₀, NOAELs, species sensitivity (you don’t have to query those via SPARQL; just note that you can click through).

We want to show:
	1.	how to retrieve the DrugBank ID for theobromine from Wikidata (P715) ￼
	2.	how to build a clickable URL to DrugBank
	3.	a concise description we can quote in the text (we’ll reuse the Wikidata “description” field as a neutral summary)

[SPARQL query](https://edu.nl/pq98w)

If we click for instance on the [DRUGbank ID url](https://go.drugbank.com/drugs/DB01412) we can see more information about theobromine. 

By clicking on the [DSSTox / CompTox url](https://comptox.epa.gov/dashboard/chemical/details/DTXSID9026132) from the result table and explore the acute toxicity endpoints (LD₅₀), species sensitivity, and reported effects. Based on these values, we can define a rough ‘safe upper bound’ for theobromine exposure in dogs, which we will later map onto plausible cacao bean concentrations.

> By following the DSSTox IDs, we can pull toxicological data indicating the dose ranges at which theobromine is harmful in different species. Dogs are particularly sensitive. 

For our breeding program, this tells us what target concentration range cacao beans should stay below to be considered “pet-safer” while still palatable for humans.


## Pharmacological role of theobromine 

How is theobromine used and what pharmacological roles does it play?

[SPARQL query](https://edu.nl/fe9ry)

WikiData annotates theobromine with roles such as vasodilator, bronchodilator, nootropic, and with uses such as medication.

So when we try to reduce theobromine in cacao, we are perturbing a molecule that’s important both ecologically and pharmacologically.

Even though we encounter theobromine mostly as “the bitter part of chocolate”, Wikidata reminds us that it is a bona fide drug-like compound, with established roles as bronchodilator and vasodilator. Any attempt to breed cacao with much lower theobromine is therefore not just a flavour tweak, but a change to the pharmacological profile of chocolate.

## Is theobromine used in any drug products?

We showed that theobromine has roles in human medicine. Is it present as an active principle in any drug? 

In Wikidata, which medications or drug products list theobromine as an active ingredient or have it as has use = medication?

[SPARQL query](https://edu.nl/g6qjb)

Theobromine is a pharmacologically active compound with use = medication.Chocolate is a food that contains this active compound – not modelled as a medication itself.

> "Let the food be thy medicine and the medicine be thy food" ~ Hippocrates 


## Recent publications about theobromine (via Wikidata + PubMed IDs)

Now we can research what are the most recent articles whose main subject is theobromine, and for which a PubMed ID is known.

[SPARQL query](https://edu.nl/g9mf6)

This gives you titles + PubMed IDs, which you can turn into URLs like
https://pubmed.ncbi.nlm.nih.gov/<pmid>/

This is a nice example of “semantic pivot”: from **compound → article metadata → PubMed.

## Products and link to LOTUS : Is theobromine a precursor of other natural products?

We can make sure that theobromine is not a precursor of important natural products that are important qualities in cacao. 

First, we run a query to see all of the otehr metabolites taht are present in the theobromine pathwyas 

[SPARQL query](https://edu.nl/4x7yh)

Which of these metabolites from theobromine pathways are in LOTUS/Wikidata?

[SPARQL query](https://edu.nl/m86d6)

In PlantMetWiki, theobromine is described as a direct biosynthetic precursor of caffeine in the methylxanthine pathway. 

> This is a nice example of a gap that PlantMetWiki could fill: despite caffeine and theobromine are biosynthetically linked, this precursor–product relation is not yet represented as an explicit triple in Wikidata or LOTUS.

So by following the reaction “theobromine → caffeine” in PlantMetWiki and linking it out to
Wikidata/LOTUS, we can argue that selecting for *theobromine-reduced* cacao could
simultaneously give us **caffeine-reduced chocolate** — attractive both for pet safety
and for caffeine-sensitive consumers.

## Finding theobromine precursors : Immediate substrates for reactions producing theobromine (who feeds into theobromine?)

What are the substrates for theobromine production, and what is their InChiKey? 

[SPARQL query](https://bit.ly/45ye9Mt)

Here we ask: **what are the direct substrates of reactions that produce theobromine**, in which species, and what are their InChIKeys?

The query above scans all PlantMetWiki pathways that contain a metabolite labelled
“theobromine”, then finds reactions where:

- `wp:target` = theobromine  
- `wp:source` = its immediate precursor(s)
  
and reports:

- the **pathway ID** (`?pwID`)
- the **species** (`?species`)
- the **PlantCyc reaction ID** (`?rxnId`)
- the **precursor label** (`?precursorLabel`)
- the **precursor InChIKey** (`?precursorInChIKey`)

[SPARQL query](https://edu.nl/jntax)

For example, we see that:

- in *Camellia irrawadiensis* and *Camellia ptilophylla*,  
  the reaction `RXN-7598` converts **7-methylxanthine**  
  (InChIKey `PFWLFWPASULGAN-UHFFFAOYSA-N`)  
  into **theobromine**.

- in *Camellia ptilophylla*,  
  the reaction `RXN-7603` converts **3-methylxanthine**  
  (InChIKey `GMSNIKWWOQHZGF-UHFFFAOYSA-N`)  
  into **theobromine**.

Together with the “theobromine → caffeine” step, this reveals the familiar
*methylxanthine ladder*:

> xanthine → 3- or 7-methylxanthine → **theobromine** → caffeine

In other words, PlantMetWiki lets us enumerate the **immediate precursors** of
theobromine as concrete chemical entities (with InChIKeys), not just generic
“methylated xanthines”.

## Enzymes synthetizing and consuming theobromine : identify genetic "knobs" you can turn 

Chemistry and toxicology tell us why we care about theobromine.
PlantMetWiki tells us how plants make it: which pathways, enzymes, and genes are involved in its biosynthesis in cacao and related species.

This query:

	1.	Looks for theobromine in Theobroma cacao pathways.
	2.	Finds reactions that produce or consume theobromine.
	3.	Gets the enzymes/genes catalysing those reactions.
	4.	Checks all other pathways anywhere in PlantMetWiki where the same enzyme participates, so you can spot possible “off-target” effects.

[SPARQL query](https://edu.nl/dfrda)


Theobromine sits in the methylxanthine pathway, with steps like: xanthosine → 7-methylxanthine → theobromine → caffeine 

PlantMetWiki’s pathway view + gene annotations show: the N-methyltransferases that carry out those steps and which steps are shared vs unique.

Here’s where PlantMetWiki’s gene/enzyme info shines.

Use your local queries to:

	1.	List all enzymes that produce theobromine (reactions with wp:target ?theobromine).
	2.	List all enzymes that consume theobromine (reactions with wp:source ?theobromine).
	3.	For each enzyme: retrieve gene identifiers, EC numbers, 

In cacao, we find a small set of methyltransferase genes that eitehr take theobromine from 7-methylxanthine or onvert theobromine onwards to caffeine. 

These genes are our breeding knobs:

	•	down-regulate or knock out theobromine-forming enzymes
	•	up-regulate enzymes that convert theobromine further to other methylxanthines
	•	or redirect flux into less bitter, less toxic analogues.

This might lower flux into theobromine and possibly increase other flavor components (polyphenols, sugars, aroma volatiles) that are not toxic to dogs.

The `otherPathways` column is especially important for breeding or engineering:
if it is empty, the enzyme appears only in the theobromine/caffeine pathway;
if it contains additional pathway IDs, the same enzyme also participates in
other parts of plant metabolism.

This allows us to distinguish:

- **“clean” knobs** – enzymes specific to methylxanthine biosynthesis, and
- **“pleiotropic” knobs** – enzymes that also act elsewhere, where editing them
  might have broader effects on plant physiology.

In the context of dog-safer, caffeine-reduced chocolate, the ideal targets are
enzymes that:

1. **produce theobromine** in cacao,
2. have few or no roles in other pathways, and
3. sit between upstream precursors and theobromine → caffeine step.

Those genes are our best candidates for lowering theobromine (and likely
caffeine) in cacao beans while minimizing unintended changes in other
metabolic networks.

## What is the SMILES string from theobromine? 

Now you want to draw theobromine in Chemdraw or any chemical sketiching tool. To do that you query PlantMetWiki to find the SMILE of theobromine: paste the SMILE on [PubChem Sketcher](https://pubchem.ncbi.nlm.nih.gov/edit2/index.html) to see the compound structure. 

[SPARQL query](https://edu.nl/r4e8m)

# Follow-up : A potential breeding strategy supported by PlantMetWiki 

To carry out a successful breeding strategy one could for instance use species diversity as a natural experiment: you could use the species from PlantMetWiki where theobromine is found to study identify allelic variants or expression patterns associated with lower theobromine, starting from the species where fully annotated pathways are found. 

Using PlantMetWiki’s information and PlantCyc gene annotations, we can:

	•	pick methylxanthine pathway genes as marker candidates for breeding
	•	search for natural variants associated with lower theobromine
	•	or design plant engineering or gene editing targets for fine-tuning flux through the pathway.

That’s a perfect “breeding strategy” hook: borrow nature’s existing variation.

Can use ortholog inference algorithms, such as OrthoFinder, SonicParanoidOrthoMCL, or larger databases like eggNOG / Ensembl Compara if they already contain your species.

# Takeaways 

This investigation strategy of theobromine via PlantMetWiki identifies: 

	•	Pathways in which the selected molecules occur
	•	Plant species associated with those pathways
	•	Chemical identifiers and labels retrieved from Wikidata

> Starting from only chemical identifiers (InChIKeys), PlantMetWiki enables the identification of plant pathways and species involved in the biosynthesis or utilization of these molecules.

This demonstrates the reverse traversal of classical pathway analysis: molecule → pathway → species


## Linking molecules to broader biochemical context

Because metabolites in PlantMetWiki are part of explicitly modeled pathways, we can now place individual molecules into a biochemical and biological context, including:

	•	upstream and downstream reactions,
	•	shared substrates and products,
	•	related metabolites across pathways.

This allows researchers to move beyond isolated compound lists and reason about biosynthetic logic.

## Enriching natural products with external chemical knowledge

PlantMetWiki supports federated SPARQL queries, enabling live integration with external resources such as Wikidata.

This allows us to retrieve additional chemical annotations (e.g. ingredient classifications, identifiers) without duplicating data.

Example: linking metabolites to Wikidata ingredient information


## From molecules to hypotheses

This molecule-centric approach enables new research questions, such as:

	•	Which plant species produce chemically similar compounds?
	•	Are related molecules produced via shared or divergent pathways?
	•	Can missing biosynthetic steps be inferred from chemically related compounds in other species?
	•	How do chemical classifications align with biosynthetic pathway structure?

Because PlantMetWiki integrates biosynthesis, species, and chemistry as linked data, these questions can be addressed programmatically.

## Takeaways 

This example illustrates how PlantMetWiki supports natural product-driven hypothesis generation:

By starting from molecular identifiers, researchers can traverse plant metabolic knowledge across pathways, species, and external chemical databases, enabling integrative analysis of biosynthesis and chemistry.


In this tutorial, we demonstrated how PlantMetWiki can be used to:

	•	Start from natural product molecules
	•	Identify associated pathways and producing species
	•	Integrate biosynthetic context with chemical knowledge
	•	Enrich results via federated queries to Wikidata

This workflow is particularly powerful for:

	•	natural products discovery,
	•	food and ingredient research,
	•	toxicology,
	•	comparative metabolomics.

Together with pathway-centric analyses, this molecule-centric perspective highlights the flexibility and power of PlantMetWiki as a linked open data platform for plant metabolism.

