---
permalink: /coronavirus/ab_initio
title: Ab initio Protein Structure Prediction
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Distributing the work of protein structure prediction around the world

The determination of the SARS-CoV-2 spike protein's structure was remarkable because in many senses it was a community effort, dividing the computational heavy lifting over thousands of volunteers' computers around the world. Two leading structure prediction projects, [Rosetta@home](https://boinc.bakerlab.org) and [Folding@home](https://foldingathome.org), encourage volunteers to download their software and contribute to a gigantic *distributed* effort to predict protein shape. Even with a modest laptop, a user can donate some of their computer's idle resources to contribute to the problem of protein structure prediction. But how does this software work?

Predicting a protein's structure using only its amino acid sequence is called <b><em>ab initio</em> structure prediction</b> (*ab initio* is from the Latin for "from the beginning"). In this lesson, we will explain a little about how *ab initio* structure prediction algorithms work.

As we dive into structure prediction, we should be more precise about two things. First, we will specify what we mean by the "structure" of a protein. Second, although we know that a polypeptide always folds into the same final three-dimensional shape, we have not said anything about *why* a protein folds in a certain way. We will need a better understanding of how the physicochemical properties of amino acids affect a protein's final structure.

## The four levels of protein structure

"Protein structure" is a broad term that encapsulates four different levels of description. A protein's **primary structure** refers to the amino acid sequence of the polypeptide chain. The figure below shows the primary structure of human hemoglobin subunit alpha.

![image-center](../assets/images/hemoglobin_primary_structure.png){: .align-center}
The primary structure of human hemoglobin subunit alpha. Each letter corresponds to one of the twenty amino acids. Source: [https://www.rcsb.org/structure/1SI4](https://www.rcsb.org/structure/1SI4).
{: style="font-size: medium;"}

A protein's **secondary structure** describes its highly regular, repeating substructures that serve as intermediate structures forming before the overall protein structure comes together. The two most common such substructures, shown in the figure below, are **alpha-helices** (left) and **beta-sheets** (right). Alpha-helices occur when nearby amino acids wrap around to form a tube-like structure; beta-sheets occur when nearby amino acids line up side-by-side to form a sheet-like structure.

![image-center](../assets/images/hemoglobin_secondary_structure.png){: .align-center}
General shape of secondary structure alpha-helices (left) and beta-sheets (right). Source: Cornell, B. (n.d.). [https://ib.bioninja.com.au/higher-level/topic-7-nucleic-acids/73-translation/protein-structure.html](https://ib.bioninja.com.au/higher-level/topic-7-nucleic-acids/73-translation/protein-structure.html)
{: style="font-size: medium;"}

A protein's **tertiary structure** describes its final 3D shape after the polypeptide chain has folded and is stable. Throughout this module, when discussing the "shape" or "structure" of a protein, we are almost exclusively referring to its tertiary structure. The figure below shows the tertiary structure of human hemoglobin subunit alpha. Note that for the sake of simplicity, the figure does not show the positions of every atom in the protein but rather represents the protein shape as a composition of secondary structures.

![image-center](../assets/images/hemoglobin_tertiary_structure.png){: .align-center}
The tertiary structure of human hemoglobin subunit alpha. Within the structure are multiple alpha-helix secondary structures. Source: [https://www.rcsb.org/structure/1SI4](https://www.rcsb.org/structure/1SI4).
{: style="font-size: medium;"}

Finally, some proteins have a **quaternary structure**, which describes the protein’s interaction with other copies of itself to form a single functional unit, or a **multimer**. Many proteins do not have a quaternary structure and function as an independent monomer. The figure below shows the quaternary structure of hemoglobin, which is a multimer consisting of two alpha subunits and two beta subunits.

![image-center](../assets/images/hemoglobin_quaternary_structure.png){: .align-center}
The quaternary structure of human hemoglobin, which consists of two alpha subunits (shown in red) and two beta subunits (shown in blue). Source: [https://commons.wikimedia.org/wiki/File:1GZX_Haemoglobin.png](https://commons.wikimedia.org/wiki/File:1GZX_Haemoglobin.png).
{: style="font-size: medium;"}

As for SARS-CoV and SARS-CoV-2, the spike protein is a **homotrimer**, meaning that it is formed of three essentially identical units. Each of these components is a **dimer**, consisting of two subunits called **S1** and **S2**. We will rely on this information throughout the module.

Proteins can often be further subdivided into **protein domains**, which are distinct functional/structural units within the protein that are typically responsible for a specific interaction or function. For example, the SARS-CoV-2 spike protein has a **receptor binding domain (RBD)** located on the S1 subunit that is responsible for interacting with the human ACE2 enzyme; the rest of the protein does not come into contact with ACE2.

## Proteins look for the lowest energy conformation

When we are talking about the energy of a system or molecule, we are referring to its potential energy.  More specifically, **potential energy** is defined as the energy stored within an object due to its position, state, and arrangement. In molecular mechanics, the potential energy is made up of the sum of bonded energy (energy related to covalent bonds) and nonbonded energy (energy from interactions not from covalent bonds) for all atoms in the molecule.

Bonded energy can be broken down into three terms: bond, angle, and dihedral. We will explain here what each term represents as additional information, but this is beyond the scope of this module as it gets extremely complicated. The bond term describes the potential between a pair of covalently bonded atoms and is represented by harmonic potential, or in simpler terms, bond stretching. The angle term describes the potential between a triplet of atoms covalently bonded like a ‘V’ shape and is represented by angle vibration. Finally, the dihedral term describes the potential between a consecutively bonded quadruplet of atoms and are represented by the angular spring between the plane formed by the first three atoms and the plane formed by the last three atoms [^TCBG].

Nonbond energy can be broken down into two terms: electrostatic interactions (Coulomb potential) and van der Waals interactions (Lennard-Jones potential). **Electrostatic interaction** refers to the attraction and repulsion force from the electric charge of the atoms. To calculate the total electrostatic interaction, we need to consider this interaction between every atom pair within the molecule. Just like proteins, atoms are dynamic systems. The electrons are constantly circling around the nucleus and at any given time, they could be localized on one side. This will cause the atom to have a temporary negative charge on one side and positive charge on the other side. In addition, nearby atoms can also influence the electron cloud. These temporary charges are referred to as **induced dipoles**. **Van der Waals** interaction refers to the attraction and repulsion force between atoms from these induced dipoles.

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

In the tutorial linked below, we will use software named [Quark](https://zhanglab.ccmb.med.umich.edu/QUARK/), which has a web interface, to run an *ab initio* structure prediction algorithm. Quark is even more sophisticated than the algorithm discussed in the previous section. For example, this algorithm applies a combination of multiple scoring functions to determine the lowest energy conformation available.

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
