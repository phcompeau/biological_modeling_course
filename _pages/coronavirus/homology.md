---
permalink: /coronavirus/homology
title: Homology Modeling for Protein Structure Prediction
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Homology modeling uses an existing structure to reduce the search space

In the previous lesson, we saw that *ab initio* structure prediction of a long protein like the SARS-Cov-2 spike protein would take a long time as well as be susceptible to inaccuracies. As we mentioned in the [introduction to structure prediction](structure_intro), however, researchers have entered over 160,000 structure entries into the PDB.  With every new structure that we identify, we gain a little more information about nature's magic protein folding algorithm. Our goal is to use the information of existing structures to help us predict the shape of proteins with unknown structure.

One of the many PDB entries is a structure of the SARS spike protein, published in 2003 at the time of the first SARS outbreak. We know that the *sequence* of this protein is 96% similar to the sequence of the SARS-CoV-2 spike protein. We have seen earlier in this module that proteins serving the same purpose, called **homologous proteins**, may have very similar structures even if they have evolved significant mutations.

Assuming that the structure of the two coronavirus spike proteins will be similar, we will therefore use the structure of the SARS spike protein as a guide when assembling the SARS-CoV-2 spike protein. In other words, we know that the search space of all conformations of SARS-CoV-2 spike protein is enormous, so why not reduce the runtime of our algorithms --- and improve accuracy --- by narrowing the search space to the collection of structures that are "nearby" the shape of the SARS spike protein?

This idea serves as the foundation of **homology modeling** for protein structure prediction (also called **comparative modeling**). By using the known protein structure of one or more "homologous" proteins (i.e., proteins serving the same purpose in a different organism) as a template, we are able to improve the accuracy of protein structure prediction for a protein with unknown structure.

## How does homology modeling work?

* Don't need anything fancy in this case but if we haven't identified what template protein we want to use, then one way we could do so is by using a sequence query algorithm like BLAST.

* We have a sequence alignment along with an existing template. Then the goal is what to do with this information.

* (Show sequence alignment.)

![image-center](../assets/images/spike_protein_similarity.jpg){: .align-center}
Protein structures of the PDB entry (isi4) for human hemoglobin subunit alpha along with five *ab initio* models of this protein. We can see how close all five models are to the experimentally verified structure, as shown in the superimposition of all six structures at right.
{: style="font-size: medium;"}

* One way to do this is to identify the conserved regions of the alignment first and assuming that these sequences correspond to an essentially identical structure in the template. Then we rely on "fragment libraries", or known substructures from a variety of proteins, to fill in the non-conserved regions and produce a final 3-D structure. Or the entire protein can be constructed with the help of these fragment libraries.

* Another way to do this is to essentially refine our energy function so that we combine both physicochemical properties of the protein and the template structure; that is, in addition to determining the free energy of a structure, we also penalize it with an increase in the energy function if it is significantly different from the template. (Oversimplification.)

* Then

In the following tutorial, we will use model the SARS-CoV-2 spike protein using three publicly available homology modeling servers (SWISS-MODEL, Robetta, and GalaxyWEB). By using three different homology approaches, we will be able to have confidence that if the results are similar, then our structure prediction is reasonably *robust*. Furthermore, comparing the results of multiple different approaches may give us more insights into structure prediction.

[Visit tutorial](tutorial_homology){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Applying homology modeling to SARS-CoV-2

The results of the three predicted models for the SARS-CoV-2 spike protein are found below. Feel free to download these, because we will compare them in the next lesson. How similar are these predictions to each other, and how similar are they to the SARS spike protein?

|Structure Prediction Server|Results|
|:--------------------------|:------|
|SWISS-MODEL (S protein)|[SWISS-MODEL Results](../_pages/coronavirus/files/SWISS_Model.zip)|
|Robetta (Single-Chain S protein)|[Robetta Results](../_pages/coronavirus/files/Robetta_Model.zip)|
|GalaxyWEB|[GalaxyWEB Results](../_pages/coronavirus/files/GalaxyWEB_Models.zip)|

[Next lesson](accuracy){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Extra

* Something about how **threading** works. Fact is that even if a protein doesn't have a homologous protein in a database, most proteins will still have a protein of very similar structure.

* Unfortunately, there are occasions where no identified proteins have notable sequence similarities. The alternative is to use threading, or fold recognition. In this case, rather than comparing the target sequence to sequences in the database, this method compares the target sequence to structures themselves. The biological basis of this method is that in nature, protein structures tend to be highly conservative and unique structural folds are therefore limited.

* A simple explanation of the general threading algorithm is that structure predictions are created by placing or “threading” each amino acid in the target sequence to template structures from a non-redundant template database, and then assessing how well it fits with some scoring function[^score]. Then, the best-fit templates are used to build the predicted model. The scoring function varies per algorithm, but it typically takes secondary structure compatibilities, gap penalties during alignment, and other terms that depend on amino acids that are bought into contact by the alignment.

* The following stuff on threading is from wikipedia: The construction of a structure template database: Select protein structures from the protein structure databases as structural templates. This generally involves selecting protein structures from databases such as PDB, FSSP, SCOP, or CATH, after removing protein structures with high sequence similarities.

The design of the scoring function: Design a good scoring function to measure the fitness between target sequences and templates based on the knowledge of the known relationships between the structures and the sequences. A good scoring function should contain mutation potential, environment fitness potential, pairwise potential, secondary structure compatibilities, and gap penalties. The quality of the energy function is closely related to the prediction accuracy, especially the alignment accuracy.

Threading alignment: Align the target sequence with each of the structure templates by optimizing the designed scoring function. This step is one of the major tasks of all threading-based structure prediction programs that take into account the pairwise contact potential; otherwise, a dynamic programming algorithm can fulfill it.

Threading prediction: Select the threading alignment that is statistically most probable as the threading prediction. Then construct a structure model for the target by placing the backbone atoms of the target sequence at their aligned backbone positions of the selected structural template.

[^score]: Movaghar, A. F., Launay, G., Schbath, S., Gibrat, J. F., & Rodolphe, F. 2012. Statistical significance of threading scores. Journal of computational biology : a journal of computational molecular cell biology, 19(1), 13–29. https://doi.org/10.1089/cmb.2011.0236

[^tasser]: Roy, A., Kucukural, A., Zhang, Y. 2010. I-TASSER: a unified platform for automated protein structure and function prediction. Nat Protoc, 5(4), 725-738. https://doi.org/10.1038/nprot.2010.5.
