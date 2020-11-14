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

## Applying an *ab initio* algorithm to a protein sequence

In the tutorial linked below, we will use software named [Quark](https://zhanglab.ccmb.med.umich.edu/QUARK/), which has a web interface, to run an *ab initio* structure prediction algorithm. Quark is even more sophisticated than the algorithm discussed in the previous section. For one, it compiles a database of fragments of known protein structures to compare our candidate protein structure against. Furthermore, it applies a combination of multiple scoring functions to determine the lowest energy conformation available.

Despite the sophistication of software like Quark, *ab initio* algorithms are still an active area of research, and we still lack an approach that is both fast and reliable. The larger the protein we use, the longer our query will take, and the more inaccurate the resulting structure may be. Accordingly, many *ab initio* algorithms limit the allowable length of a protein sequence. This is the case for Quark, which limits us to 200 amino acids. Since the SARS-CoV-2 spike protein contains 1281 amino acids, we will instead demonstrate how to use this software on the shorter human hemoglobin subunit alpha.

[Visit tutorial](tutorial_ab_initio){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Toward a faster approach for protein structure prediction

The figure below shows the top five structures produced by Quark for human hemoglobin subunit alpha, along with the protein's experimentally verified structure. Although *ab initio* prediction is not perfect, it is still able to accurately reconstruct a model of this protein from its amino acid sequence.

![image-center](../assets/images/ab_initio_results.png){: .align-center}
Protein structures of the PDB entry (isi4) for human hemoglobin subunit alpha along with five *ab initio* models of this protein. We can see how close all five models are to the experimentally verified structure, as shown in the superimposition of all six structures at right.
{: style="font-size: medium;"}

Yet at the same time, we wonder how we can speed up our structure prediction algorithms so that they will scale to a larger protein like the SARS-CoV-2 spike protein. To this end, we recall the point earlier in this lesson about Quark using a database of known short structures to compare our reconstruction against. In the next lesson, we will learn about another type of protein structure prediction that allows researchers to model large proteins by comparing a protein of unknown structure against a protein with similar sequence having known structure.

**STOP:** What protein should we use to compare the SARS-CoV-2 spike protein against?
{: .notice--primary}

[Next lesson](homology){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

[^1]: Kubelka, J., et. al. 2004. The protein folding ‘speed limit’. Current Opinion in Structural Biology. 14, 76-88. https://doi.org/10.1016/j.sbi.2004.01.013

[^2]: Benkert, P., Schwede, T. & Tosatto, S.C. 2009. QMEANclust: estimation of protein model quality by combining a composite scoring function with structural density information. BMC Struct Biol 9, 35. https://doi.org/10.1186/1472-6807-9-35

## Extra

* Models published before crystallography can be found here: https://www.ssgcid.org/cttdb/molecularmodel_list/?target__icontains=BewuA

* Need to define side chain

* For a simple analogy of an energy landscape, imagine a ball on the top of a hill: <img src="../_pages/coronavirus/files/EnergyCartoon.png">
