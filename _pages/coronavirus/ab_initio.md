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

In this case, our search space is the collection of all possible conformations of a given protein. Where in the chemotaxis example we think of the concentration of attractant at a point, here we think of the energy of a given conformation. And we can imagine our optimization problem as "exploring" this space of all conformations in order to find the conformation of lowest energy. This analogy is illustrated in the figure below, which imagines the energy function as corresponding to an elevation at each of the search space; our goal, then, is to find the lowest point in this space.

![image-center](../assets/images/energy_landscape.png){: .align-center}
We can imagine each conformation of a given protein as occupying a point in a landscape, in which the elevation of a point corresponds to the energy of the conformation at that point. Courtesy: David Beamish.
{: style="font-size: medium;"}

## A local search algorithm for *ab initio* structure prediction

Now that we have thought about finding the most stable protein structure as exploring a search space, our next question is how to develop an algorithm to explore this space. Continuing the analogy to chemotaxis, our idea is to adapt *E. coli*'s clever exploration [exploration algorithm](chemotaxis/home_conclusion) from a previous lesson to our purposes. That is, at every step, sense the "direction" in which the energy function decreases by the most, and then move in this direction.

Adapting this exploration algorithm to protein structure prediction requires us to develop a notion of what it means to consider the points "near" a given conformation in a protein search space. To this end, many *ab initio* algorithms will start at an arbitrary initial conformation and then make a variety of minor modifications to that structure (i.e., nearby points in the space), updating the current conformation to the modification that produces the greatest decrease in free energy. These algorithms then iterate the process of moving in the greatest of greatest energy decrease until we reach a conformation for which no nearby points reduce the free energy. Such an approach for structure prediction falls into a broad category of optimization algorithms called **local search algorithms**.

Yet, returning to the bacterial analogy, imagine what happens if we were to place many small sugar cubes and one large sugar cube into a bacterial environment. A bacterium will sense the gradient not of the large sugar cube but of its *nearest* attractant. As a result, because the smaller food sources outnumber the larger food source, the bacterium will likely not move to the point of greatest attractant concentration. In terms of bacterial exploration, this is a feature, not a bug; if the bacterium exhausts one food source, then it will just move to another. But in terms of protein structure prediction, we should be worried of winding up in such a **local minimum**, or a point of our search space for which no "neighboring" points have better score.

**STOP:** Do you see any ways in which we could improve our local search approach for structure prediction?
{: .notice--primary}

Since our algorithm may get stuck in a local minimum, we are looking for an algorithm that is in a sense more intelligent than the one devised by bacteria for exploring their environment. Fortunately, we can modify our local search algorithm in a variety of ways. First, because the initial conformation chosen has a huge influence on the final conformation that we return, we could run the algorithm multiple times with different starting conformations. This is analogous to allowing multiple bacteria to explore their environment at different starting points. Second, by allowing ourselves to move to a conformation with *greater* free energy (i.e., a worse conformation) with some probability, we would give our local search algorithm a chance to "bounce" out of a local minimum. In an approach called **simulated annealing**, which is borrowed from metallurgy, we reduce the probability of increasing the free energy over time, so that the likelihood of bouncing out of a local minimum decreases over time, and eventually we will settle into a final conformation. Once again, we see the benefit of randomness for solving practical problems!

## New section




## CAPS, Rosetta, and QUARK

* QUARK is one of the leading *ab initio* protein structure prediction available. As mentioned before, currently employed *ab initio* modeling is not truly *ab initio*. We still utilize data from previously determined structures in some form. However, the database that is used is not required to contain structures that are similar in global structure. Very short fragments of the known structures are used to construct the model rather than using entire proteins as templates. In addition, extensive physiochemical knowledge is needed. Here is a flow chart of QUARK:

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
