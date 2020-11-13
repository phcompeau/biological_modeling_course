---
permalink: /coronavirus/structure_intro
title: An Introduction to Protein Structure Prediction
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## From DNA to RNA to protein

Proteins are one of the most important groups of macromolecules in living organisms, contributing to essentially all functions within them. Recall that in our [introduction to transcription](../motifs/transcription) in a previous module, we introduced the "central dogma" of molecular biology, in which DNA is transcribed into RNA, which is then translated into protein. This process is represented in the figure reproduced below.

![image-center](../assets/images/Central_Dogma_of_Molecular_Biochemistry_with_Enzymes.jpg){: .align-center}
The central dogma of molecular biology states that molecular information flows from DNA in the nucleus, into the RNA that is transcribed from DNA, and then into proteins that are translated from RNA. Image courtesy: Dhorpool, Wikimedia commons user.
{: style="font-size: medium;"}

In this earlier module, we focused on how master regulators called transcription factors could affect the rates at which a given gene could be transcribed into RNA and translated into protein. In this module, we investigate what happens after translation.

Before continuing, we should be a bit more precise about what we mean by "protein". The ribosome converts triplets of RNA nucleotides into a chain of amino acids called a **polypeptide**. This chain of amino acids will then "fold" into a three-dimensional shape; this folding happens without any outside influence as the polypeptide settles into the most stable three-dimensional structure. Regardless of the organism or cell type, and even if a polypeptide chain is unfolded, it will always fold back into essentially the same 3-D structure. This means that nature is employing a "magic algorithm" that produces the structure of a protein from its sequence of amino acids.

The problem is that deciphering the magic algorithm for protein folding has mystified biologists for decades.

This problem

This brings us to our first biological problem of interest: what is the shape of a given protein? This **structure prediction problem**, which our work in the first part of this module will focus on, is simple to state but deceptively difficult. In fact, it has been an active area of biological research for several decades.

You may be wondering why we care about the shape of a given protein. Knowing a protein's shape is essential to determining the function of that protein as well as how it interacts with other proteins or ligands. And understanding protein interactions underlies a huge amount of biological research. To take one example, a human disease may be caused by a diseased protein, in which case we are looking for a drug (i.e., some other substance) that binds to the protein and causes some change of interest in that protein. For another example of why shape is vital, consider the following video of a ribosome (which is a complex of RNA and proteins) translating a messenger RNA into protein. For translation to succeed, the ribosome needs to have a very precise shape including a "slot" that the messenger RNA strand can fit into and be read.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/TfYf_rPWUdY" frameborder="0" allowfullscreen></iframe>

In the next section, we will discuss how interactions involving proteins are typically modeled.

## Protein shape determines binding affinity

The simplest model of protein interactions is Emil Fischer’s **lock and key** model, which dates all the way back to 1894. This model considers a protein that is an **enzyme**, which serves as a catalyst for a reaction involving another molecule called a **substrate**, and we think of the substrate as a key fitting into the enzyme lock. If the substrate does not fit into the active site of an enzyme, then the reaction will not occur.

However, proteins are rather flexible molecules, a fact that we will return to when we discuss the binding of the coronavirus spike protein to a human enzyme in a later lesson. As a result of this flexibility, Daniel Koshland introduced a modified model called the **induced fit** model in 1958. In this model, the enzyme and substrate may not fit perfectly, nor are they rigid like a lock and key. Rather, the two molecules may fit inexactly, changing shape as they bind to mold together more tightly. That having been said, if an enzyme and substrate's shape do not match well with each other, then they will not bind. For an overview of the induced fit model, please check out this excellent video from Khan Academy.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/8lUB2sAQkzw" frameborder="0" allowfullscreen></iframe>

As we have seen throughout this course, molecular interactions are ruled by probability. Any two molecules may *interact*, but their rate of *dissociation* will be much higher if they do not fit together well. Furthermore, even if two molecules do fit together, they need to not only collide with each other but also have the correct orientation with respect to each other.

So we can think about identifying the structure of a collection of proteins as cataloging the enormously varied shapes that different proteins can have. For example, the figure below shows each "protein of the month" in 2019 named by the **Protein Data Bank** (**PDB**). But the fact remains that proteins are submicroscopic; so how is it that researchers are able to determine these shapes?

![image-center](../assets/images/different_protein_shapes.jpg){: .align-center}
Each "protein" of the month in 2019 named by the PDB. Note how different the shapes are of all these proteins, which accomplish a wide variety of cellular tasks.
{: style="font-size: medium;"}

## Laboratory methods for determining protein structure

There are a few existing methods for accurately determining protein structure. In this section, we will pause to discuss two of them.

First, **X-ray crystallography** (sometimes called **macromolecular crystallography**) works by first crystallizing many copies of a protein and then shining an intense x-ray beam at the crystal. When light hits each protein, it is diffracted, creating patterns from which the position of each atom in the protein can be inferred.  If you are interested in learning more about X-ray crystallography, check out the following excellent two-part video series from The Royal Institution.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/gLsC4wlrR2A" frameborder="0" allowfullscreen></iframe>

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/WJKvDUo3KRk" frameborder="0" allowfullscreen></iframe>

Second, we could use **cryo-electron microscopy** (**cryo-EM**). In this approach, we preserve thousands of copies of our protein in non-crystalline ice and then examine them with an electron microscope. Check out the following YouTube video from the University of California San Francisco for a lengthier introduction to cryo-EM.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/Qq8DO-4BnIY" frameborder="0" allowfullscreen></iframe>

Unfortunately, both of these approaches are expensive. X-ray crystallography costs upward of $2,000 per protein; furthermore, Crystallizing a protein is a challenging task, and each copy of the protein must line up in the same way, which does not work for very flexible proteins. As for cryo-EM, an electron microscope is a very complicated machine that may cost anywhere between $100,000 and $27 million (the actual cost of a microscope housed at Lawrence Berkeley National Lab).

Protein structures that have been determined experimentally are typically stored in the PDB, which we mentioned above. As of early 2020, there are over 160,000 entries in this database, which only contained a few thousand proteins at the turn of the century. Can we consider structure determination a solved problem, then?

Before we set aside structure prediction, let’s consider the human proteome (i.e., collection of all proteins). A human proteome study published in 2016 estimated that humans have between 620,000 and 6.13 million protein isoforms (i.e., proteins with different shapes). A database of 160,000 proteins would be insufficient for covering humans, let alone the entire collection of proteins across all organisms on the planet.

Another issue with laboratory methods of structure determination is that they require our ability to sample the actual physical proteins. For example, to produce bacterial proteins, we need to culture bacteria, and yet microbiologists have estimated that less than 2% of bacteria can be cultured in the lab [^1].

What, then, can we do? Fortunately, although identifying protein structure is difficult, researchers have spent decades cataloging the genomes (i.e., the sum total of DNA in an organism's nucleus) of thousands of species. Because of the central dogma of molecular biology, we know that much of this DNA winds up being translated into protein. As a result, biologists know the *sequence* of amino acids making up many proteins.

The first whole genome sequence of SARS-CoV-2, isolate *Wuhan-Hu-1*, was released on 10 January 2020 by Wu, F. et. al., and is available in GenBank along with an annotation of the genome[^2][^3], shown in the figure below. Upon sequence comparison, SARS-CoV-2 was found to be related to several coronaviruses isolated from bats and distantly related to SARS-CoV-1, the viral strain that caused the 2003 SARS outbreak. In fact, SARS-CoV-2 has a sequence identity of around 96% with bat coronavirus RaTG13, leading us to the hypothesis that the virus originated from bats, which is further supported by the fact that bats are a natural reservoir of SARS-related coronaviruses.

![image-center](../assets/images/SARSCoV2Annotation.png){: .align-center}
An annotated genome of SARS-CoV-2. Accessed from GenBank: [https://go.usa.gov/xfzMM](https://go.usa.gov/xfzMM).
{: style="font-size: medium;"}

Recall from the start of this lesson that even if a protein is unfolded into a polypetide, then it always folds back into essentially the same three-dimensional shape. This leads us to an idea: given the sequence of amino acids corresponding to the SARS-CoV-2 spike protein, can we predict the final 3-D structure of this protein? In other words, can we reverse engineer the magic algorithm that nature uses for protein folding?

Unfortunately, as will see in the next section, predicting protein structure from an amino acid sequence is a very challenging problem.

## What makes protein structure prediction so difficult?

One reason why inferring protein structure from sequence is so difficult is that the number of potential protein shapes is enormous, and small perturbations in the primary structure of a protein can drastically change the protein's shape and even render it useless. This fact might give us hope, that if we look at experimentally verified structures of proteins with known amino acid sequences, then we could reverse engineer the sequence from the structure and start determining how the magic folding algorithm works. But the inverse problem of inferring sequence from structure is very difficult as well because different amino acids can have similar chemical properties, and so some mutations will hardly change the shape of the protein at all. Furthermore, two very different amino acid sequences can fold into proteins with similar shapes and identical function.

For example, the following figure compares both the sequences and structures of hemoglobin subunit alpha from humans (*Homo sapiens*; PDB: [1si4](https://www.rcsb.org/structure/1SI4) shortfin mako sharks (*Isurus oxyrinchus* ; PDB: [3mkb](https://www.rcsb.org/structure/3mkb) and emus (*Dromaius novaehollandia*; PDB: [3wtg](https://www.rcsb.org/structure/3wtg). Hemoglobin is the oxygen-transport protein in the blood, consisting of two alpha "subunit" proteins and two beta subunit proteins that combine into a protein complex; because hemoglobin is well-studied, we will use it as an example throughout this module. The subunit alpha proteins across the three species are markedly different in terms of primary structure, and yet their 3-D structures are essentially identical.

![image-center](../assets/images/SequenceStructureExample.png){: .align-center}
(Top) An amino acid sequence comparison of the first 40 (out of 140) amino acids of hemoglobin subunit alpha for three species: human, mako shark, and emu. A column is colored blue if all three species have the same amino acid, white if two species have the same amino acid, and red if all amino acids are different. Sequence identity calculates the number of positions in two amino acid sequences that share the same character. (Bottom) Side by side comparisons of the 3-D structures of the three proteins. The final figure on the right superimposes the first three structures to highlight their similarities.
{: style="font-size: medium;"}

Another reason why structure prediction is difficult is the sheer amount of details that are required for describing and computationally storing a protein structure.

Structures that have been determined are typically uploaded into the PDB as a .pdb file. Many entries are on the quaternary structure of the protein or depict a protein system of multiple proteins or ligands. Each macromolecule is stored as a **chain** in the PDB structure. For example, the SARS-CoV-2 S protein is a trimer made up of three identical chains. The .pdb file is extremely dense as it holds all the details about the protein and chains, from the very basic primary structure of the protein all the way to the position of every single atom. The simplest way to think about how the entire protein is stored is to first represent atoms as points on a 3D plane with each atom having its 3D orthogonal coordinates (X,Y,Z) in the unit of angstroms ($$ 10^{-10} $$ meter). This is the atomic coordinates of the protein. A simplified view of the atomic coordinates section is shown in the figure below.

![image-center](../assets/images/simplifiedPDB.png){: .align-center}
{: style="font-size: medium;"}

Because the structure of the 20 amino acids are well studied, we know which atoms are connected within each residue. The atomic bonds that link amino acids in sequence are easy to deduce from the primary structure of the protein. However, the angles of these connections also need to be explicit to describe the 3D structures. A very good analogy would be to think of the polypeptide chain as a *<a href="https://ruwix.com/twisty-puzzles/rubiks-snake-twist/" target="_blank">Rubik's Twist</a>*, shown below. Each block is able to twist, changing the angle and shape of the toy. In order to make a specific shape, every block must be twisted to a specific angle.

![image-center](../assets/images/rubiks_twist.gif){: .align-center}
{: style="font-size: medium;"}

![image-center](../assets/images/RubiksTwist.png){: .align-center}
{: style="font-size: medium;"}

These angles are referred to as peptide torsion angles, shown in the figure below. The two bonds connecting the alpha carbon of an amino acid are able rotate, allowing a peptide chain to be able to fold into many different possible conformations. Phi (φ) refers to the bond angle connecting to the amino group and psi (ψ) refers to the bond angle connecting to the carboxyl group. The specific set of phi and psi angles of the protein helps describe the structure of the protein. Omega (ω) describes the bond angle of the peptide bond between two amino acids, but is almost always locked at 180°.

![image-center](../assets/images/psiphi.png){: .align-center}
{: style="font-size: medium;"}

Below is an excellent video from Jacob Elmer illustrating the dihedral angles for a given amino acid.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/1usemtIYe_s" frameborder="0" allowfullscreen></iframe>

There exists connections between residues that cannot be infered from the primary structure (e.g. disulfide bonds) which are also described within the file. This is just scratching the tip of the iceberg of the information required to represent a protein structure. For more information, check out the <a href="http://www.wwpdb.org/documentation/file-format" target="_blank">official PDB documentation</a>.

Another reason why protein structure prediction is so difficult is due to the huge number of conformations that a single polypeptide can take. Finding the correct shape is very much like finding a needle in a haystack.

* Levinthal’s Paradox. Large number of degrees of freedom in a polypeptide chain. Given a chain with 100 residues, there will be 99 peptide bonds, resulting in 198 phi and psi bond angles. If each bond has three stable conformations, then there are a maximum of $$ 3^{198} $$ different possible conformations. Will take longer than the age of the universe to sample all conformation to find the correct native form. Paradox is that most natural protein folding occurs spontaneously, typically within the timescale of milliseconds. The fastest within a couple of microseconds [^1].

TIE THIS BACK TO COMBINATORIAL EXPLOSION
  * Local residues form stable interactions an act as nucleation points (protein folding intermediates and partial-folded transition states), facilitation folding speed.
  * Proposed funnel-like energy landscapes (not really the case, the energy landscape is more like a caldera).
  * Main point: need A LOT of computing
  * Nature has a magic algorithm for protein folding. Can we find it?

In order find this magic algorithm, we need a very good understanding of all the small interactions that occur between atoms during protein folding, including bonding energy, attraction/repulsion forces from electrical charges between molecules (electrostatic interactions and van der Waals interactions), thermodynamics; all which are subject to alterations depending on the environment. Regardless of its difficulty, protein structure prediction is a very important problem to solve given its potential for many, many applications.


<!--

## Four levels of protein structure



These amino acids are linked together to form a protein as a amino acid chain, or polypeptide chain, that will typically undergo folding to obtain a 3D structure. In the first figure below, the general shape of the amino acid is shown: a central alpha-Carbon, a carboxyl group, an amino group, and finally one of 20 side-groups that differentiate the amino acids. Each amino acid is linked to the next by a peptide bond, and it is this connection and alpha-Carbon that make up the protein backbone, as shown in the second figure. The side groups of each amino acid are responsible for amino acid's chemical properties. These chemical properties allow the amino acids to interact with each other and fold into the 3D structure.

We stated that the overall 3D structure (tertiary structure) of the protein is dictated by the interactions of the side chains. Even when we unfold, or denature, a protein, it will eventually fold back into essentially the same shape because of these interactions.

![image-center](../assets/images/AminoAcid.png){: .align-center}
{: style="font-size: medium;"}

![image-center](../assets/images/Backbone.png){: .align-center}
{: style="font-size: medium;"}



Protein structure are separated into four different levels of description. The most basic description, the **primary structure**, refers the specific amino acid sequence of the polypeptide chain. Below is an example of the human hemoglobin subunit alpha and its primary structure.

![image-center](../assets/images/PrimaryStructureExample.png){: .align-center}
{: style="font-size: medium;"}

The **secondary structure** describes the highly regular substructures in the protein. Essentially, they are the 3D structures of local amino acids groups within the protein and spontaneously form during the folding process. In a sense, they are the intermediate structures that form before the overall protein structure. The two main substructures, shown in the figure below, are alpha-helices (left) and beta-sheets (right). Alpha-helices are formed when local amino acids fold into a tube-like structure. Beta-sheets are when the local amino acids interact by lining up side-by-side, forming a sheet-like structure. The formation of these secondary structures help with the overall process of folding.

![image-center](../assets/images/SecondaryStructure.png){: .align-center}
{: style="font-size: medium;"}

The **tertiary structure** describes the overall 3D shape of the protein that results from the fully-folded polypeptide chain. This is what we think of as the "shape" of the protein. In a sense, it is the combination of all the secondary structures and linkages that creates the tertiary structure. Below is the tertiary structure of human hemoglobin subunit alpha.

![image-center](../assets/images/TertiaryStructureExample.png){: .align-center}
{: style="font-size: medium;"}

Finally, some proteins have a **quaternary structure**, which describes the protein’s interaction with other copies of itself to form a single functional unit, or a multimer. Many proteins do not have a quaternary structure and functions as an independent monomer.

Proteins are can often be divided into protein domains. Domains are distinct functional/structural units within the protein and are typically responsible for a specific interaction or function. For example, The Sars-CoV-2 S protein has a Receptor Binding Domain (RBD) that is responsible for interacting with ACE2. The rest of the protein does not come into contact with ACE2.

**Note to self:** also need hemoglobin ongoing example

Now that we have a pretty good understanding of protein structure, we need to explain why the 3D structure is so important.

-->

## Good section on similar proteins from different sequences



We can compare our algorithm for structure prediction against known 3-D structures (SARS1 before the SARS2 3-D structure was known experimentally, and both viruses afterward). Researchers would do this to see how good our algorithm is at reproducing a known structure as a proof of concept for the approach when we don’t have funds for X-ray crystallography but want a reliable representation of a newly sequenced protein’s structure.


[Next lesson](ab_initio){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Citations

[^1]: Wade W. 2002. Unculturable bacteria--the uncharacterized organisms that cause oral infections. Journal of the Royal Society of Medicine, 95(2), 81–83. https://doi.org/10.1258/jrsm.95.2.81

[^2]: Severe acute respiratory syndrome coronavirus 2 isolate Wuhan-Hu-1, complete genome. https://www.ncbi.nlm.nih.gov/nuccore/MN908947

[^3]: Annotated Severe acute respiratory syndrome coronavirus 2 isolate Wuhan-Hu-1, complete genome. https://go.usa.gov/xfzMM
