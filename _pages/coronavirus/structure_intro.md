---
permalink: /coronavirus/structure_intro
title: An Introduction to Protein Structure Prediction
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Determining protein structure is fundamental to understanding protein function

Proteins are one of the most important groups of macromolecules in living organisms, contributing to essentially all functions within them. Recall that in our [introduction to transcription](../motifs/transcription) in a previous module, we introduced the "central dogma" of molecular biology, in which DNA is transcribed into RNA, which is then translated into protein. This process is represented in the figure reproduced below.

![image-center](../assets/images/Central_Dogma_of_Molecular_Biochemistry_with_Enzymes.jpg){: .align-center}
The central dogma of molecular biology states that molecular information flows from DNA in the nucleus, into the RNA that is transcribed from DNA, and then into proteins that are translated from RNA. Image courtesy: Dhorpool, Wikimedia commons user.
{: style="font-size: medium;"}

In this earlier module, we focused on how master regulators called transcription factors could affect the rates at which a given gene could be transcribed into RNA and translated into protein. In this module, we investigate what happens after translation.

Before continuing, we should be a bit more precise about what we mean by "protein". The ribosome converts triplets of RNA nucleotides into a chain of amino acids called a **polypeptide**. The polypeptide will then "fold" into a three-dimensional shape; this folding happens without any outside influence as the polypeptide settles into the most stable three-dimensional structure. Even if a polypeptide chain is unfolded, it will almost always fold back into essentially the same 3-D structure in a manner of microseconds. This means that nature is applying a "magic algorithm" that produces the structure of a protein from its sequence of amino acids. But how does this algorithm work?

This brings us to our first biological problem of interest: can we predict the shape of a protein from its amino acid sequence? This **structure prediction problem**, which we will focus on in the first part of this module, is simple to state but deceptively difficult. In fact, it has been an active area of biological research for several decades.

You may be wondering why we care about protein structure. Knowing a protein's shape is essential to starting to determine its function and how it interacts with other proteins or molecules in its environment. (There are still a few thousand human proteins whose function is unknown.) And understanding protein interactions underlies a huge amount of biological research. For example, a disease may be caused by a faulty protein, in which case researchers want to find a drug (i.e., some other chemical substance) that binds to the protein and causes some change of interest in that protein, such as inhibiting its behavior.

For a more visual example of how protein shape affects protein function, consider the following video of a ribosome (which is a complex of RNA and proteins) translating a messenger RNA into protein. For translation to succeed, the ribosome needs to have a very precise shape including a "slot" that the messenger RNA strand can fit into and be read.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/TfYf_rPWUdY" frameborder="0" allowfullscreen></iframe>

As we have seen throughout this course, molecular interactions are ruled by probability. Any two molecules may *interact*, but their rate of *dissociation* will be much higher if they do not fit together well. Furthermore, two interacting molecules need to not only collide with each other but also have the correct orientation in order to fit together.

Because structure prediction is such a fundamental problem, researchers wish to catalog the enormously varied shapes that different proteins can have. For example, the figure below shows each "protein of the month" in 2019 named by the **Protein Data Bank** (**PDB**). But the fact remains that proteins are submicroscopic; so how is it that researchers are able to determine these shapes?

![image-center](../assets/images/different_protein_shapes_2020.jpg){: .align-center}
Each "molecule of the month" in 2020 named by the PDB. Note how different the shapes are of all these proteins, which accomplish a wide variety of cellular tasks. Note that the SARS-CoV-2 spike protein was the molecule of the month in June 2020. Source: [https://pdb101.rcsb.org/motm/motm-by-date](https://pdb101.rcsb.org/motm/motm-by-date).
{: style="font-size: medium;"}

## Laboratory methods for determining protein structure

There are a few existing methods for accurately determining protein structure. In this section, we will pause to discuss two of them.

First, **X-ray crystallography** (sometimes called **macromolecular crystallography**) works by first crystallizing many copies of a protein and then shining an intense x-ray beam at the crystal. When light hits each protein, it is diffracted, creating patterns from which the position of each atom in the protein can be inferred.  If you are interested in learning more about X-ray crystallography, check out the following excellent two-part video series from The Royal Institution.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/gLsC4wlrR2A" frameborder="0" allowfullscreen></iframe>

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/WJKvDUo3KRk" frameborder="0" allowfullscreen></iframe>

Second, we could use **cryo-electron microscopy** (**cryo-EM**). In this approach, which is rapidly gaining in popularity, researchers preserve thousands of copies of the protein in non-crystalline ice and then examine them with an electron microscope. Check out the following YouTube video from the University of California San Francisco for a lengthier introduction to cryo-EM.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/Qq8DO-4BnIY" frameborder="0" allowfullscreen></iframe>

Unfortunately, both of these approaches are expensive. X-ray crystallography costs upward of $2,000 per protein; furthermore, Crystallizing a protein is a challenging task, and each copy of the protein must line up in the same way, which does not work for very flexible proteins. As for cryo-EM, an electron microscope is a very complicated machine that may cost anywhere between $100,000 and $27 million (the actual cost of a microscope housed at Lawrence Berkeley National Lab).

Protein structures that have been determined experimentally are typically stored in the PDB, which we mentioned above. As of early 2020, there are over 160,000 entries in this database, which only contained a few thousand proteins at the turn of the century. Can we consider structure determination a solved problem, then?

Before we set aside structure prediction, let’s consider the human proteome (i.e., collection of all proteins). A human proteome study published in 2016 estimated that humans have between 620,000 and 6.13 million protein isoforms (i.e., proteins with different shapes). A database of 160,000 proteins would be insufficient for covering humans, let alone the entire collection of proteins across all organisms on the planet.

Another issue with laboratory methods of structure determination is that they require our ability to sample the actual physical proteins. For example, to produce bacterial proteins, we need to culture bacteria, and yet microbiologists have estimated that less than 2% of bacteria can be cultured in the lab.[^Wade]

What, then, can we do? Fortunately, although identifying protein structure is difficult, researchers have spent decades cataloging the genomes (i.e., the sum total of DNA in an organism's nucleus) of thousands of species. Because of the central dogma of molecular biology, we know that much of this DNA winds up being translated into protein. As a result, biologists know the *sequence* of amino acids making up many proteins.

The first whole genome sequence of SARS-CoV-2, isolate *Wuhan-Hu-1*, was released on 10 January 2020 by Wu, F. et. al., and is available in GenBank along with an annotation of the genome[^Genome][^Annotation], shown in the figure below. Upon sequence comparison, SARS-CoV-2 was found to be related to several coronaviruses isolated from bats and distantly related to SARS-CoV-1, the viral strain that caused the 2003 SARS outbreak. In fact, SARS-CoV-2 has a sequence identity of around 96% with bat coronavirus RaTG13, leading us to the hypothesis that the virus originated from bats, which is further supported by the fact that bats are a natural reservoir of SARS-related coronaviruses.

![image-center](../assets/images/SARSCoV2Annotation.png){: .align-center}
An annotated genome of SARS-CoV-2. Accessed from GenBank: [https://go.usa.gov/xfzMM](https://go.usa.gov/xfzMM).
{: style="font-size: medium;"}

Annotating these viral genes is important, but we ultimately need to know the shape of the protein that these genes encode in order to decipher how the virus interacts with the host cell. This is extremely valuable in designing treatments or vaccines that target specific viral proteins.

Recall from the start of this lesson that even if a protein is unfolded into a polypetide, then it always folds back into essentially the same three-dimensional shape. This leads us to an idea: given the sequence of amino acids corresponding to the SARS-CoV-2 spike protein, can we predict the final 3-D structure of this protein? In other words, can we reverse engineer the magic algorithm that nature uses for protein folding?

Unfortunately, as we will see in the next section, predicting protein structure from an amino acid sequence is a very challenging problem.

## What makes protein structure prediction so difficult?

One reason why inferring protein structure from sequence is so difficult is that the number of potential protein shapes is enormous, and small perturbations in the primary structure of a protein can drastically change the protein's shape and even render it useless. This fact might give us hope, that if we look at experimentally verified structures of proteins with known amino acid sequences, then we could reverse engineer the sequence from the structure and start determining how the magic folding algorithm works. But the inverse problem of inferring sequence from structure is very difficult as well because different amino acids can have similar chemical properties, and so some mutations will hardly change the shape of the protein at all. Furthermore, two very different amino acid sequences can fold into proteins with similar shapes and identical function.

For example, the following figure compares both the sequences and structures of hemoglobin subunit alpha from humans (*Homo sapiens*; PDB: [1si4](https://www.rcsb.org/structure/1SI4) shortfin mako sharks (*Isurus oxyrinchus* ; PDB: [3mkb](https://www.rcsb.org/structure/3mkb) and emus (*Dromaius novaehollandia*; PDB: [3wtg](https://www.rcsb.org/structure/3wtg). Hemoglobin is the oxygen-transport protein in the blood, consisting of two alpha "subunit" proteins and two beta subunit proteins that combine into a protein complex; because hemoglobin is well-studied, we will use it as an example throughout this module. The subunit alpha proteins across the three species are markedly different in terms of primary structure, and yet their 3-D structures are essentially identical.

![image-center](../assets/images/SequenceStructureExample.png){: .align-center}
(Top) An amino acid sequence comparison of the first 40 (out of 140) amino acids of hemoglobin subunit alpha for three species: human, mako shark, and emu. A column is colored blue if all three species have the same amino acid, white if two species have the same amino acid, and red if all amino acids are different. Sequence identity calculates the number of positions in two amino acid sequences that share the same character. (Bottom) Side by side comparisons of the 3-D structures of the three proteins. The final figure on the right superimposes the first three structures to highlight their similarities.
{: style="font-size: medium;"}

Another reason why protein structure prediction is so difficult is due to the huge number of conformations that a single polypeptide can take. The polypeptide is very flexible, with the ability to rotate in multiple ways at each amino acid, which means that the polypeptide could fold into a staggering number of different shapes. A good analogy for this flexibility producing many different shapes is the "Rubik's Twist" puzzle, shown below, which consists of a linear chain of flexible blocks that can form a huge number of shapes.

![image-center](../assets/images/rubiks_twist.gif){: .align-center}
An illustration of Rubik's twist forming into a ball. Source: [https://grabcad.com/library/rubik-s-snake-1](https://grabcad.com/library/rubik-s-snake-1).
{: style="font-size: medium;"}

A polypeptide is flexible because the bonds holding amino acids together, called **peptide bonds**, are able to rotate, and this rotation produces **peptide torsion angles** that change the shape of the protein. As shown in the following figure, we are interested in two angles, called the **phi angle (φ)** and **psi angle (ψ)**, which connect the amino acid's "alpha carbon" to its amino group and carboxyl group, respectively. (There is a third omega angle (ω) describing the bond angle of the peptide bond between two amino acids, but is almost always locked at 180°.)

![image-center](../assets/images/torsion_angles.png){: .align-center}
A polypeptide chain of multiple amino acids with the three torsion angles φ, ψ, and ω indicated. Image courtesy: Adam Rędzikowski.
{: style="font-size: medium;"}

Below is an excellent video from Jacob Elmer illustrating how changing φ and ψ at a single amino acid can reorient the conformation of a protein.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/1usemtIYe_s" frameborder="0" allowfullscreen></iframe>

A polypeptide with *n* amino acids will have *n* - 1 peptide bonds, meaning that its shape will be influenced by 99 phi angles and 99 psi angles. Say that each bond has three stable conformations. Then this means that there are 3<sup><em>n</em></sup> total possible protein conformations. As a result, if *n* is just 200 (a reasonably short polypeptide), the number of possible protein strucures we need to consider is more than the number of atoms in the universe! The ability for the protein to reliably find a single conformation using the magic algorithm despite such an enormous number of potential shapes is called **Levinthal's paradox** and is in a sense another example of the principle of "combinatorial explosion" that we saw in the module on bacterial chemotaxis [^Levinthal].

Although protein structure prediction is a difficult problem, this is not to say that it is impossible.  In the next lesson, we will examine how existing software attempts to replicate nature's magic algorithm for folding a polypeptide chain into a 3-D protein structure. We will then place ourselves in the shoes of early SARS-CoV-2 researchers working before the structure of the virus's spike protein had been experimentally validated to see if we can predict this structure and give biologists a head start on fighting the pandemic.

[Next lesson](ab_initio){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}


[^Wade]: Wade W. 2002. Unculturable bacteria--the uncharacterized organisms that cause oral infections. Journal of the Royal Society of Medicine, 95(2), 81–83. https://doi.org/10.1258/jrsm.95.2.81

[^Genome]: Severe acute respiratory syndrome coronavirus 2 isolate Wuhan-Hu-1, complete genome. https://www.ncbi.nlm.nih.gov/nuccore/MN908947

[^Annotation]: Annotated Severe acute respiratory syndrome coronavirus 2 isolate Wuhan-Hu-1, complete genome. https://go.usa.gov/xfzMM

[^Fischer]: Fischer, E. 1894. Einfluss der Configuration auf die Wirkung der Enzyme. Ber. Dtsch. Chem. Ges., 27: 2985-2993. https://doi.org/10.1002/cber.18940270364

[^Koshland]: Koshland D.E. 1958. Application of a Theory of Enzyme Specificity to Protein Synthesis. Proc Natl Acad Sci U S A. 44(2):98-104. doi:10.1073/pnas.44.2.98

[^Levinthal]: Levinthal, C. 1969. How to Fold Graciously. Mossbaur Spectroscopy in Biological Systems, Proceedings of a meeting held at Allerton House, Monticello, Illinois. eds. Debrunner, P., Tsibris, J.C.M., Munck, E. University of Illinois Press Pages 22-24.

[^HPP]: https://hupo.org/human-proteome-project

[^HPP2016]: Omenn, G. S., Lane, L., Lundberg, E. K., Beavis, R. C., Overall, C. M., & Deutsch, E. W. 2016. Metrics for the Human Proteome Project 2016: Progress on Identifying and Characterizing the Human Proteome, Including Post-Translational Modifications. Journal of proteome research, 15(11), 3951–3960. https://doi.org/10.1021/acs.jproteome.6b00511

[^Munnink]: Oude Munnink, B.B., Nieuwenhuijse, D.F., Stein, M. et al. 2020. Rapid SARS-CoV-2 whole-genome sequencing and analysis for informed public health decision-making in the Netherlands. Nat Med 26, 1405–1410. https://doi.org/10.1038/s41591-020-0997-y

[^Islam]: Islam, M.R., Hoque, M.N., Rahman, M.S. et al. 2020. Genome-wide analysis of SARS-CoV-2 virus strains circulating worldwide implicates heterogeneity. Sci Rep 10, 14004. https://doi.org/10.1038/s41598-020-70812-6
