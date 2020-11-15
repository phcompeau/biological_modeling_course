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

One of the many PDB entries is a structure of the SARS-CoV spike protein, published in 2003 at the time of the first SARS outbreak. We know that the *sequence* of this protein is 96% similar to the sequence of the SARS-CoV-2 spike protein. We have seen earlier in this module that proteins serving the same purpose, called **homologous proteins**, may have very similar structures even if they have evolved significant mutations.

Assuming that the structure of the two coronavirus spike proteins will be similar, we will therefore use the structure of the SARS-CoV spike protein as a guide when assembling the SARS-CoV-2 spike protein. In other words, we know that the search space of all conformations of SARS-CoV-2 spike protein is enormous, so why not reduce the runtime of our algorithms --- and improve accuracy --- by narrowing the search space to the collection of structures that are "nearby" the shape of the SARS-CoV spike protein?

This idea serves as the foundation of **homology modeling** for protein structure prediction (also called **comparative modeling**). By using the known protein structure of one or more "homologous" proteins (i.e., proteins serving the same purpose in a different organism) as a template, we are able to improve the accuracy of protein structure prediction for a protein with unknown structure.

## How does homology modeling work?

In the case of the SARS-CoV-2 spike protein, we already know that we want to use the SARS-CoV spike protein as a template. However, if we do not know which template to use before we begin, then we can use a standard approach for searching a protein sequence against a database, such as BLAST.

Once we have obtained a template structure that we want to use as a guide for prediction of our given protein's structure, the question is how to use the information provided by the template to help construct the structure of our protein. Even very similar species will have slight differences in the structures of homologous proteins, and so it will not suffice to simply report the existing structure as the structure of our protein.

Even though the SARS-CoV and SARS-CoV-2 genomes are 96% similar, be careful in assuming that this means that the 4% differences between these two genomes are uniformly spaced throughout the genome. On the contrary, when we look at genomes from similar species, we expect to see certain **conserved regions** where the species are very similar and other **variable regions** where the species are more different than the average. For example, the spike proteins between SARS-CoV and SARS-CoV-2 are only 76% similar.

The phenomenon of conserved and variable regions even occurs within individual genes. For example, as the following figure indicates, the coronavirus spike protein is made up of two domains (called **S1** and **S2**), and whereas the S2 domain is 90% similar between the two viruses, the S1 domain is only 64% similar. There are even regions of greater or less variability within each of these domains!

![image-center](../assets/images/spike_protein_similarity.png){: .align-center}
Regions of differing similarity over the SARS-CoV and SARS-CoV-2 spike proteins. The S1 domain tends to be more variable, while the S2 domain is more conserved (and even has a small region of 100% similarity). In this figure, "NTD" stands for "N-terminal domain" and "RBD" stands for "receptor binding domain", two subunits of the S1 domain. Source: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7166309/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7166309/)[^Jaimes].
{: style="font-size: medium;"}

Our homology modeling approach for protein structure prediction should be able to take variable and conserved regions into account. One way to do so is first to assume that more conserved regions assume to essentially identical structures in the template protein. We can then use **fragment libraries**, or known substructures from a variety of proteins, to fill in the non-conserved regions and produce a final 3-D structure. This approach to homology modeling is called **fragment assembly**.

Another way that homology modeling can be performed is to  refine our energy function so that we combine similarity to the template structure and physicochemical properties of our proposed structure into a single function. This is an involved process, but we provide a simplified way of thinking of this form of homology modeling. A structure that reduces free energy would decrease this function, but if a structure that is too dissimilar from the template would be penalized by increasing this function.

In the following tutorial, we will use model the SARS-CoV-2 spike protein using different homology modeling software from three publicly available servers (SWISS-MODEL, Robetta, and GalaxyWEB), all of which apply a variant of the fragment assembly approach. By using three different homology approaches, we will be able to have confidence that if the results are similar, then our structure prediction is reasonably *robust*. Furthermore, comparing the results of multiple different approaches may give us more insights into structure prediction.

[Visit tutorial](tutorial_homology){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Applying homology modeling to SARS-CoV-2

The results of the three predicted models for the SARS-CoV-2 spike protein are found below. If you did not follow the tutorial, feel free to download these, because we will discuss them in the next lesson. In particular, how similar are these predictions to each other, and how similar are they to the SARS-CoV spike protein?

|Structure Prediction Server|Results|
|:--------------------------|:------|
|SWISS-MODEL (S protein)|[SWISS-MODEL Results](../_pages/coronavirus/files/SWISS_Model.zip)|
|Robetta (Single-Chain S protein)|[Robetta Results](../_pages/coronavirus/files/Robetta_Model.zip)|
|GalaxyWEB|[GalaxyWEB Results](../_pages/coronavirus/files/GalaxyWEB_Models.zip)|

To rigorously compare two protein structures, we need to have a way of representing a given protein's structure. To do so, we store the 3-D spatial coordinates of every atom in the protein. (Note that because we know the sequence of amino acids making up the protein, we will also know the identity of every atom in the protein.) The above three models are stored in `.pdb` format, which stores these coordinates in a form that is illustrated in the figure below.

To specify the `.pdb` format further, first recall that the coronavirus spike protein is a trimer. Accordingly, the file will separate the atoms into three **chains** making up this trimer. Each atom is labeled according to several different pieces of information, including:

1. the element from which the atom derives;
2. the amino acid in which the atom is contained;
3. the name of the chain in which this amino acid is found;
4. the position of the amino acid within this chain; and
5. the 3-D coordinates (*x*, *y*, *z*) of the atom in angstroms (10<sup>-10</sup> meters).

![image-center](../assets/images/simplifiedPDB.png){: .align-center}
A simplified diagram showing how the `.pdb` format encodes the 3-D coordinates of every atom while labeling the identity of this atom and the chain on which it is found. Source: [https://proteopedia.org/wiki/index.php/Atomic_coordinate_file](https://proteopedia.org/wiki/index.php/Atomic_coordinate_file).
{: style="font-size: medium;"}

The above information is critical for understanding protein structure but is just part of the information needed to fully represent a protein structure. For example, there are connections between amino acids called **disulfide bonds** that are also described within a `.pdb` file. For more information, check out the [official PDB documentation](http://www.wwpdb.org/documentation/file-format).

[Next lesson](accuracy){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Extra

* Need to have already specified that spike is a homotrimer before we appeal to the tutorial.

* We need to make sure that the specification of the .pdb file type comes back somewhere before we give the results. It may be a perfect place to do so in this lesson.

* Missing definition of domain -- need to do this concisely earlier. Perhaps explain that spike has S1 and S2.

* We may need a discussion of what NTD and RBD do at some point.

* Perhaps something about how **threading** works. Fact is that even if a protein doesn't have a homologous protein in a database, most proteins will still have a protein of very similar structure.

* Unfortunately, there are occasions where no identified proteins have notable sequence similarities. The alternative is to use threading, or fold recognition. In this case, rather than comparing the target sequence to sequences in the database, this method compares the target sequence to structures themselves. The biological basis of this method is that in nature, protein structures tend to be highly conservative and unique structural folds are therefore limited.

* A simple explanation of the general threading algorithm is that structure predictions are created by placing or “threading” each amino acid in the target sequence to template structures from a non-redundant template database, and then assessing how well it fits with some scoring function[^score]. Then, the best-fit templates are used to build the predicted model. The scoring function varies per algorithm, but it typically takes secondary structure compatibilities, gap penalties during alignment, and other terms that depend on amino acids that are bought into contact by the alignment.

* Each software has its own algorithms and method of assembly, such as how to decide which templates to use, how to use the templates, and how to fill in blurry areas (no good matches with templates). Nevertheless, the three softwares essentially build the models by assembling varying fragments from templates. If you would like to learn more about the intricacies of each software, you can follow these linkes: [Robetta](https://www.rosettacommons.org/docs/latest/application_documentation/structure_prediction/RosettaCM), [Galaxy]( https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3462707/), [SWISS-MODEL](https://swissmodel.expasy.org/docs/help).

[^score]: Movaghar, A. F., Launay, G., Schbath, S., Gibrat, J. F., & Rodolphe, F. 2012. Statistical significance of threading scores. Journal of computational biology : a journal of computational molecular cell biology, 19(1), 13–29. https://doi.org/10.1089/cmb.2011.0236

[^tasser]: Roy, A., Kucukural, A., Zhang, Y. 2010. I-TASSER: a unified platform for automated protein structure and function prediction. Nat Protoc, 5(4), 725-738. https://doi.org/10.1038/nprot.2010.5.

[^Jaimes]: Jaimes, J. A., André, N. M., Chappie, J. S., Millet, J. K., & Whittaker, G. R. 2020. Phylogenetic Analysis and Structural Modeling of SARS-CoV-2 Spike Protein Reveals an Evolutionary Distinct and Proteolytically Sensitive Activation Loop. Journal of molecular biology, 432(10), 3309–3325. https://doi.org/10.1016/j.jmb.2020.04.009
