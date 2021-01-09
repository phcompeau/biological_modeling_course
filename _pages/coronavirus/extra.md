---
permalink: /coronavirus/extra
title: "VMD Plugin: NAMD Energy"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Protein shape determines binding affinity (from structure intro but possibly just delete this)

Now that we understand the importance of shape in determining how proteins interact with molecules in their environment, we will spend some time discussing how these interactions are modeled.

The simplest model of protein interactions is Emil Fischer’s **lock and key** model, which dates back to 1894 [^Fischer]. This model considers a protein that is an **enzyme**, which serves as a catalyst for a reaction involving another molecule called a **substrate**, and we think of the substrate as a key fitting into the enzyme lock. If the substrate does not fit into the active site of an enzyme, then the reaction will not occur.

However, proteins are flexible, a fact that we will return to when we discuss the binding of the coronavirus spike protein to a human enzyme in a later lesson. Because of this flexibility, Daniel Koshland introduced a modified model called the **induced fit** model in 1958.[^Koshland] In this model, the enzyme and substrate may not fit perfectly, nor are they rigid like a lock and key. Rather, the two molecules may fit inexactly, changing shape as they bind to mold together more tightly. That having been said, if an enzyme and substrate's shape do not match well with each other, then they will not bind. For an overview of the induced fit model, please check out this excellent video from Khan Academy.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/8lUB2sAQkzw" frameborder="0" allowfullscreen></iframe>

## Extra ab initio

* Somewhere need trimer of dimers for spike protein

* Models published before crystallography can be found here: [SSGCID Models](https://www.ssgcid.org/cttdb/molecularmodel_list/?target__icontains=BewuA)

* Table based on side chains

![image-center](../assets/images/AminoAcidChart.png){: .align-center}
A chart of the twenty amino acid grouped by chemical properties. The side chain of each amino acid is highlighted in blue. Source: OpenStax, Biology. OpenStax CNX. Sep 15, 2020 http://cnx.org/contents/185cbf87-c72e-48f5-b51e-f14f21b5eabd@14.1
{: style="font-size: medium;"}

* For a simple analogy of an energy landscape, imagine a ball on the top of a hill:

![image-center](../assets/images/EnergyCartoon.png){: .align-center}
A ball on top of a hill represents a high energy system. The ball is unstable and will spontaneously roll down the hill before coming to a stop at the bottom of a valley, representing a low energy system. The ball is now stable and will not move on its own.
{: style="font-size: medium;"}

## Extra 2 ab initio -- stuff on energy

* In protein structure, need force field = energy function somewhere (?)

* Should also be clear before we get here that total = electrostatic + van der Waals

* From Chris: For more information of how to calculate the energies and the functions for potential energy, click <a href="https://www.ks.uiuc.edu/Research/namd/2.9/ug/node22.html" target="_blank">here</a>.

## Extra

* Need to have already specified that spike is a homotrimer before we appeal to the tutorial.

* We need to make sure that the specification of the .pdb file type comes back somewhere before we give the results. It may be a perfect place to do so in this lesson.

* Missing definition of domain -- need to do this concisely earlier. Perhaps explain that spike has S1 and S2.

* We may need a discussion of what NTD and RBD do at some point.

* Perhaps something about how **threading** works. Fact is that even if a protein doesn't have a homologous protein in a database, most proteins will still have a protein of very similar structure.

* Unfortunately, there are occasions where no identified proteins have notable sequence similarities. The alternative is to use threading, or fold recognition. In this case, rather than comparing the target sequence to sequences in the database, this method compares the target sequence to structures themselves. The biological basis of this method is that in nature, protein structures tend to be highly conservative and unique structural folds are therefore limited.

* A simple explanation of the general threading algorithm is that structure predictions are created by placing or “threading” each amino acid in the target sequence to template structures from a non-redundant template database, and then assessing how well it fits with some scoring function[^score]. Then, the best-fit templates are used to build the predicted model. The scoring function varies per algorithm, but it typically takes secondary structure compatibilities, gap penalties during alignment, and other terms that depend on amino acids that are bought into contact by the alignment.

* Each software has its own algorithms and method of assembly, such as how to decide which templates to use, how to use the templates, and how to fill in blurry areas (no good matches with templates). Nevertheless, the three softwares essentially build the models by assembling varying fragments from templates. If you would like to learn more about the intricacies of each software, you can follow these linkes: [Robetta](https://www.rosettacommons.org/docs/latest/application_documentation/structure_prediction/RosettaCM), [Galaxy]( https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3462707/), [SWISS-MODEL](https://swissmodel.expasy.org/docs/help).
