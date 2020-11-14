---
permalink: /coronavirus/ab_initio
title: Ab initio Protein Structure Prediction
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Distributing the work of protein structure prediction around the world

In this lesson, we will discuss how to determine the structure of a protein from its amino acid sequence. This problem was of utmost importance in early 2020 as biologists raced to find any information that they could about the novel coronavirus and its spike protein.

What makes this story remarkable is that in many senses it was a community effort, not just because it enlisted so many researchers around the world, but because the computational heavy lifting was divided over thousands of volunteers from around the world. Two leading software projects, [Rosetta@home](https://boinc.bakerlab.org) and [Folding@home](https://foldingathome.org), encourage volunteers to download this software and contribute to a gigantic *distributed* effort to predict protein shape. That is, even with a modest laptop, a user can donate some of their computer's idle resources to working on the problem of protein structure prediction. But how does this software work?

Predicting structure from sequence using only the physicochemical properties of proteins is called <b><em>ab initio</em> structure prediction</b>, where *ab initio* is from the Latin for "from the beginning". To perform this type of structure prediction, we need to incorporate many different aspects of molecular interactions, including bonding energy, attraction/repulsion forces from electrical charges between molecules (electrostatic interactions and van der Waals interactions), and thermodynamics. Furthermore, all of these variables are subject to change depending on the environment.

## Modeling *ab initio* structure prediction as an exploration problem

Although a host of different algorithms have been developed for *ab initio* protein structure through the years, these algorithms all find themselves solving a similar problem. A central theme of the previous module on bacterial chemotaxis was that regardless of how complicated a system of chemical reactions might be, the system moves toward equilibrium. The same principle is true of the magic algorithm underlying protein folding in the cell; when a protein folds into its final structure, it is obtaining a conformation of minimum "free energy", meaning that the structure is as chemically stable as possible.

For example, nine of the twenty commonly occurring amino acids in proteins are hydrophobic, meaning that their side chains tend to be repelled by water. A protein having many hydrophobic amino acids on its exterior would therefore be less stable, and as a result we tend to find these amino acids sheltered from the external environment on the interior of the protein.

Much biochemical research has contributed to the development of scoring functions that compute the free energy of a candidate protein shape. As a result, for a given scoring function, we can think of *ab initio* structure prediction as solving an optimization problem: given a sequence of amino acids, find the 3-D structure for this polypeptide having minimum energy.

This formulation of protein structure may not strike you as similar to anything that we have done before in this course. However, consider a bacterium exploring an environment for food, as we did in the previous module on chemotaxis. Every point in the bacterium's "search space" is characterized by a concentration of attractant at that point, and the bacterium's goal was to reach the point of greatest attractant concentration.

In this case, our search space is the collection of all possible conformations of a given protein. Where in the chemotaxis example we think of the concentration of attractant at a point, here we think of the energy of a given conformation. And we can imagine our optimization problem as "exploring" this space of all conformations in order to find the conformation of lowest energy, as illustrated in the figure below.

![image-center](../assets/images/energy_landscape.png){: .align-center}
Courtesy: David Beamish
{: style="font-size: medium;"}



We would like to

When we assign the

**STOP:** Do you see any ways in which we could improve this local search heuristic?
{: .notice--primary}





Place a bunch of small food sources and a single large big food source, odds are that the bacterium will simply move to its nearest food source and remain there. Local minimum.



* In protein structure prediction, the path to the best model is not a straight line. Common approaches often have steps for model refinement. When models are first generated, they are assessed and used to create better models. Although time consuming and computationally heavy, the more times we can repeat this cycle, the better the final product. Approaches for ab initio modeling used are not dissimilar from the biased random walk approach that E. coli uses to explore its space for food. In this case, the “search space” is not a physical space but rather the set of all legal structures of the protein for this structure. The “food” is not the current concentration of an attractant but the current score of a candidate structure. And “nearby” objects are not points in space but rather 3-d protein structures that correspond to making slight changes to the current structure.

* High-level description of the general algorithm:
  * Start with an arbitrary protein structure.
  * Consider all its possible neighbors. Neighbors are conformations that differ by a single change, such as rotation of a single bond. Is there one with a score better than the current structure?
  * If yes, update the current structure to the highest scoring neighbor and repeat the previous step.
  * If not, return the current structure as the best one found

**STOP:** Are there any drawbacks you see with this approach for predicting a protein’s structure?
{: .notice--primary}



* The most glaring weakness is that the initial structure chosen can have a huge influence on the final structure that we produce.  As a result, we can get stuck in a “local optimum”, in this case a protein structure that is higher scoring than its neighbors but that doesn’t score very well compared to all possible structures. One resolution to this issue is to run the algorithm multiple times with different starting positions, choosing the solution that has the best score over all these runs. Another solution is to not necessarily always take the best-scoring neighbor as the next current protein structure, but rather to choose a neighbor randomly, where higher-scoring neighbors have a higher probability of selection.

Randomness appears once again!

## CAPS, Rosetta, and QUARK



* QUARK is one of the leading *ab initio* protein structure prediction available. As mentioned before, currently employed *ab initio* modeling is not truly *ab initio*. We still utilize data from previously determined structures in some form. However, the database that is used is not required to contain structures that are similar in global structure. Very short fragments of the known structures are used to construct the model rather than using entire proteins as templates. In addition, extensive physiochemical knowledge is needed. Here is a flow chart of QUARK:

<img src="../_pages/coronavirus/files/QuarkFlowChart.png">

* As mentioned in the figure, many potential models, or decoys, are created. To select the best performing model, a scoring function is used. Many different scoring functions are used, some combining multiple types of scoring, and generally fall into one of two categories: consensus or clustering. Simply put, consensus follows the idea that the more common the predicted conformations are, the more likely it is to be correct as opposed to structural patterns that are rarely found. On the other hand, clustering methods are much more complicated. Commonly used clustering basis includes stereochemical plausibility of the models, environmental compatibility of the residues, and energy-based calculations such as physics-based functions and knowledge-based statistical potentials [^2].

Current *ab initio* algorithms are constantly being improved. Due to the complexity of protein structure prediction, we are not at the level where we can perfectly predict a protein's tertiary structure from just the primary structure. As a result, *ab initio* structure predictions can end up being inaccurate. The larger the protein, the more inaccurate the model may become. As such, many of the algorithms limit the sequence length in order to preserve accuracy.

To see an example of *ab initio* structure prediction using *QUARK*, go to the following tutorial.

[Visit tutorial](tutorial_ab_initio){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

<hr>

From this section, we learned that current *ab initio* algorithms are limited. In the tutorial, we wanted to use *QUARK*, one of the leading *ab initio* methods, to model the SARS-CoV-2 S protein. However, in order to preserve accuracy, the *QUARK* restricted the length of the input sequence to only 200 amino acids. This meant that we could not model the S protein nor the RBD of the S protein. Instead, we had to use a smaller protein, the human hemoglobin subunit alpha, as the example. In the next lesson, we will learn about another type of protein structure prediction that allows researchers to create more accurate results and model large proteins. Using this type of structure prediction, we will be able to predict the structure of the S protein and create models.

[Next lesson](homology){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Extra

* Models published before crystallography can be found here: https://www.ssgcid.org/cttdb/molecularmodel_list/?target__icontains=BewuA

* Need to define side chain

* For a simple analogy of an energy landscape, imagine a ball on the top of a hill: <img src="../_pages/coronavirus/files/EnergyCartoon.png">

## Citations

[^1]: Kubelka, J., et. al. 2004. The protein folding ‘speed limit’. Current Opinion in Structural Biology. 14, 76-88. https://doi.org/10.1016/j.sbi.2004.01.013

[^2]: Benkert, P., Schwede, T. & Tosatto, S.C. 2009. QMEANclust: estimation of protein model quality by combining a composite scoring function with structural density information. BMC Struct Biol 9, 35. https://doi.org/10.1186/1472-6807-9-35
