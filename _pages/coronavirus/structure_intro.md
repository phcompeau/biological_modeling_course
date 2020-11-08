---
permalink: /coronavirus/structure_intro
title: An Inntroduction to Protein Structure Prediction
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## From DNA to RNA to protein

Proteins are one of the most important groups of macromolecules in living organisms, contributing to essentially all functions within them. Recall that in our [../motifs/transcription](introduction to transcription) in a previous module, we introduced the "central dogma" of molecular biology, in which DNA is transcribed into RNA, which is then translated into protein. This process is represented in the figure reproduced below.

![image-center](../assets/images/Central_Dogma_of_Molecular_Biochemistry_with_Enzymes.jpg){: .align-center}
The central dogma of molecular biology states that molecular information flows from DNA in the nucleus, into the RNA that is transcribed from DNA, and then into proteins that are translated from RNA. Image courtesy: Dhorpool, Wikimedia commons user.
{: style="font-size: medium;"}

In this earlier module, we focused on how master regulators called transcription factors could affect the rates at which a given gene could be transcribed into RNA and translated into protein. In this module, we investigate what happens after translation.

Before continuing, we should be a bit more precise about what we mean by "protein". The ribosome converts triplets of RNA nucleotides into a chain of amino acids called a **polypeptide**. This chain of amino acids will then "fold" into a three-dimensional shape; this folding happens without any outside influence as the polypeptide settles into the most stable three-dimensional structure. Regardless of the organism or cell type, and even if the polypeptide is unfolded, it will always fold back into the same three-dimensional shape.

This brings us to our first biological problem of interest: what is the shape of a given protein? This **structure prediction problem**, which our work in the first part of this module will focus on, is simple to state but deceptively difficult. In fact, it has been an active area of biological research for several decades.

## Four levels of protein structure

Before discussing the  

<!--

These amino acids are linked together to form a protein as a amino acid chain, or polypeptide chain, that will typically undergo folding to obtain a 3D structure. In the first figure below, the general shape of the amino acid is shown: a central alpha-Carbon, a carboxyl group, an amino group, and finally one of 20 side-groups that differentiate the amino acids. Each amino acid is linked to the next by a peptide bond, and it is this connection and alpha-Carbon that make up the protein backbone, as shown in the second figure. The side groups of each amino acid are responsible for amino acid's chemical properties. These chemical properties allow the amino acids to interact with each other and fold into the 3D structure.

We stated that the overall 3D structure (tertiary structure) of the protein is dictated by the interactions of the side chains. Even when we unfold, or denature, a protein, it will eventually fold back into essentially the same shape because of these interactions.

![image-center](../assets/images/AminoAcid.png){: .align-center}
{: style="font-size: medium;"}

![image-center](../assets/images/Backbone.png){: .align-center}
{: style="font-size: medium;"}

-->

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

## Protein shape determines binding affinity

The first question that we might ask about protein structure prediction is why we care about the shape of a given protein. Understanding the shape of a protein is essential to determining the function of a given protein as well as understanding how that protein interacts with other proteins or ligands.

The simplest model of protein interactions is Emil Fischer’s **lock and key** model, which dates all the way back to 1894. This model considers a protein that is an **enzyme** serving as a catalyst for a reaction involving another molecule called a **substrate**, and we think of the substrate as a key fitting into the enzyme lock. If the substrate does not fit into the active site of an enzyme, then no reaction will happen.

However, proteins are rather flexible molecules, a fact that we will return to soon when we discuss the binding of the coronavirus spike protein to a human enzyme. As a result of this flexibility, Daniel Koshland introduced a modified model called the **induced fit** model in 1958. In this model, the enzyme and substrate may not fit perfectly, nor are they rigid like a lock and key. Rather, the two molecules may fit imperfectly and change shape as they bind in order to mold together more tightly. That having been said, if an enzyme and substrate have a shape that does not match well with each other, then they will not bind. For an overview of the induced fit model, please check out this excellent video from Khan Academy.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/8lUB2sAQkzw" frameborder="0" allowfullscreen></iframe>

As we have seen throughout this course, the world of protein interactions is dictated by probability. Any two molecules may *interact*, but their rate of *dissociation* will be much higher if they do not fit together well. Furthermore, even if two molecules do fit together, they need to not only collide with each other but also have the correct orientation.

## Laboratory methods for determining protein structure

The next question that we might ask is what makes structure determination of a protein a difficult problem.

**START HERE**

Even when studying an unknown protein, we can predict its function just by knowing what it looks like. There are several methods used to accurately determine the protein structure, such as X-ray crystallography, NMR spectroscopy, and electron microscopy.

X-ray crystallography (sometimes referred to as macromolecular crystallography, or MX) works by first crystallizing the protein and then subjecting it to an intense x-ray beam. The protein will diffract the beam and create patters that describe the electron distribution in the protein. By creating a map of the electron density, it can be analyzed to determine the position of each atom. Thus, x-ray crystallography can determine the protein structure with extreme precision. Unfortunately, there are many drawbacks. First, the process of crystallization is complex, limiting the types of protein that can be analyzed this way. Second, flexible proteins are difficult to study since x-ray crystallography relies on having large amounts of molecules that need to be aligned in the exact same orientation. Third, x-ray crystallography is very expensive, with services charging around $2000 dollars for a single sample. The following two-part YouTube video series from *"The Royal Institution"* gives a very good explanation on how x-ray cyrstallography works.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/gLsC4wlrR2A" frameborder="0" allowfullscreen></iframe>

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/WJKvDUo3KRk" frameborder="0" allowfullscreen></iframe>

For NMR spectroscopy, proteins are placed in a magnetic field and probed with radio waves. The set of resonance can then be analyzed to produce a list of atomic nuclei that are in close proximity and to characterize the local conformation of the atoms. One of the benefits of this method is that the protein can be studied in solution, offering information of the protein in a more realistic environment. NMR spectroscopy can cost up to $500 dollars and are limited to small or medium proteins.

Electron microscopy (3DEM) directly images the molecule by using a system of electron lenses. Producing 3D images requires imaging thousands of different particles that are preserved in non-crystalline ice (cryo-EM). Improvements in 3DEM have allowed resolution of the images to improve drastically and in some cases, reaching the resolution levels of NMR spectroscopy. However, because electron microscopes are extremely complicated machinery, purchasing one may cost anywhere between $100,000 to the most expensive $27 million (located in Lawrence Berkeley National Lab). *UC San Francisco* has published an informative YouTube video that introduces cryo-electron microscopy.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/Qq8DO-4BnIY" frameborder="0" allowfullscreen></iframe>

Protein structures that have been determined are typically stored in the Protein Data Bank (PDB) and is constantly growing larger and larger. As of 1 April 2020, there are a total of 162,269 entries on the PDB, but is this really a large number?

![image-center](../assets/images/PDBGraph.png){: .align-center}
{: style="font-size: medium;"}

Let’s take a look at the human proteome. In a human proteome study published in 2016, it was estimated that between 0.62 to 6.13 million protein species can exist in humans. Now consider that the natural world is estimated to have 8.7 million species. 162,269 number of protein structures now looks like an incredibly small number, even when protein conservation between species is taken into account. Given the current speed of new entries, we would never be able to record this many protein structures. Another problem is these methods of structure determination they need the actual physical proteins themselves. In microbiology, it is estimated that of all bacterial life, less than 2% of bacteria can be cultured in the lab [^1]. Rather than culturing and isolating bacteria, we can directly study DNA (metagenomics), RNA (metatranscriptomics), and proteins (metaproteomics) found directly in the environment or surrounding biomasses. However, we cannot just hope to find all the different proteins by chance. So, what can we do? One potential strategy is to create algorithms for predicting protein structure directly from the protein sequence.

## Very different proteins can have similar shape

Small perturbations in the primary structure of a protein can drastically change its shape and can even render the protein useless. For this reason, we might form the hypothesis that if we know the shape of a protein, then we will be able to infer its primary structure. But this is far from the truth. Because different amino acids can have similar chemical properties, some mutations will hardly change the shape of the protein at all; furthermore, two very different amino acid sequences can fold into proteins with similar shapes and identical function.

For example, the following figure compares both the sequences and structures of hemoglobin subunit alpha from humans (*Homo sapiens*; PDB: <a href="https://www.rcsb.org/structure/1SI4" target="_blank">1si4</a>) shortfin mako sharks (*Isurus oxyrinchus* ; PDB: <a href="https://www.rcsb.org/structure/3mkb" target="_blank">3mkb</a>) and emus (*Dromaius novaehollandia*; PDB: <a href="https://www.rcsb.org/structure/3wtg" target="_blank">3wtg</a>). These three protein subunits are markedly different in terms of primary structure, and yet their 3-D structures are essentially identical.

![image-center](../assets/images/SequenceStructureExample.png){: .align-center}
(Top) An amino acid sequence comparison of the first 40 (out of 140) amino acids of hemoglobin subunit alpha for three species: human, mako shark, and emu. A column is colored blue if all three species have the same amino acid, white if two species have the same amino acid, and red if all amino acids are different. Sequence identity calculates the number of positions in two amino acid sequences that share the same character. (Bottom) Side by side comparisons of the 3-D structures of the three proteins. The final figure on the right superimposes the first three structures to highlight their similarities.
{: style="font-size: medium;"}

## Protein Structure Prediction

The first whole genome sequence of SARS-CoV-2 isolate *Wuhan-Hu-1* was released on 10 January 2020 by Wu, F. et. al., and is available in GenBank along with the genome annotations [^2][^3]. Perhaps due to the SARS 2003 outbreak, many different types of coronaviruses have been sequenced and studied. Upon sequence comparison, SARS-CoV-2 was found to be related to several coronaviruses isolated from bats and distantly related to SARS-CoV-1. In fact, SARS-CoV-2 has a sequence identity of around 96% with bat coronavirus RaTG13, leading to the hypothesis that the virus originated from bats, which is further supported by the fact that bats are natural reservoir hosts of SARS-related coronaviruses.

![image-center](../assets/images/SARSCoV2Annotation.png){: .align-center}
{: style="font-size: medium;"}

The question is whether we can predict the structure of the Spike protein directly form the sequence of its gene in DNA?

The point of this question is that we can compare our algorithm for structure prediction against known 3-D structures (SARS1 before the SARS2 3-D structure was known experimentally, and both viruses afterward). Researchers would do this to see how good our algorithm is at reproducing a known structure as a proof of concept for the approach when we don’t have funds for X-ray crystallography but want a reliable representation of a newly sequenced protein’s structure.

Unfortunately, protein structure prediction from sequence is a *extremely difficult* problem. One reason why is the sheer amount of details that are required for describing and computationally storing a protein structure.

Structures that have been determined are typically uploaded into the PDB as a .pdb file. Many entries are on the quaternary structure of the protein or depict a protein system of multiple proteins or ligands. Each macromolecule is stored as a **chain** in the PDB structure. For example, the SARS-CoV-2 S protein is a trimer made up of three identical chains. The .pdb file is extremely dense as it holds all the details about the protein and chains, from the very basic primary structure of the protein all the way to the position of every single atom. The simplest way to think about how the entire protein is stored is to first represent atoms as points on a 3D plane with each atom having its 3D orthogonal coordinates (X,Y,Z) in the unit of angstroms ($$ 10^{-10} $$ meter). This is the atomic coordinates of the protein. A simplified view of the atomic coordinates section is shown in the figure below.

![image-center](../assets/images/simplifiedPDB.png){: .align-center}
{: style="font-size: medium;"}

Because the structure of the 20 amino acids are well studied, we know which atoms are connected within each residue. The atomic bonds that link amino acids in sequence are easy to deduce from the primary structure of the protein. However, the angles of these connections also need to be explicit to describe the 3D structures. A very good analogy would be to think of the polypeptide chain as a *<a href="https://ruwix.com/twisty-puzzles/rubiks-snake-twist/" target="_blank">Rubik's Twist</a>*, shown below. Each block is able to twist, changing the angle and shape of the toy. In order to make a specific shape, every block must be twisted to a specific angle.

![image-center](../assets/images/RubiksTwist.png){: .align-center}
{: style="font-size: medium;"}

These angles are referred to as peptide torsion angles, shown in the figure below. The two bonds connecting the alpha carbon of an amino acid are able rotate, allowing a peptide chain to be able to fold into many different possible conformations. Phi (φ) refers to the bond angle connecting to the amino group and psi (ψ) refers to the bond angle connecting to the carboxyl group. The specific set of phi and psi angles of the protein helps describe the structure of the protein. Omega (ω) describes the bond angle of the peptide bond between two amino acids, but is almost always locked at 180°.

![image-center](../assets/images/psiphi.png){: .align-center}
{: style="font-size: medium;"}

There exists connections between residues that cannot be infered from the primary structure (e.g. disulfide bonds) which are also described within the file. This is just scratching the tip of the iceberg of the information required to represent a protein structure. For more information, check out the <a href="http://www.wwpdb.org/documentation/file-format" target="_blank">official PDB documentation</a>.

Another reason why protein structure prediction is so difficult is due to the huge number of conformations that a single polypeptide can take. Finding the correct shape is very much like finding a needle in a haystack.

* Levinthal’s Paradox. Large number of degrees of freedom in a polypeptide chain. Given a chain with 100 residues, there will be 99 peptide bonds, resulting in 198 phi and psi bond angles. If each bond has three stable conformations, then there are a maximum of $$ 3^{198} $$ different possible conformations. Will take longer than the age of the universe to sample all conformation to find the correct native form. Paradox is that most natural protein folding occurs spontaneously, typically within the timescale of milliseconds. The fastest within a couple of microseconds [^1].
  * Local residues form stable interactions an act as nucleation points (protein folding intermediates and partial-folded transition states), facilitation folding speed.
  * Proposed funnel-like energy landscapes (not really the case, the energy landscape is more like a caldera).
  * Main point: need A LOT of computing
  * Nature has a magic algorithm for protein folding. Can we find it?

In order find this magic algorithm, we need a very good understanding of all the small interactions that occur between atoms during protein folding, including bonding energy, attraction/repulsion forces from electrical charges between molecules (electrostatic interactions and van der Waals interactions), thermodynamics; all which are subject to alterations depending on the environment. Regardless of its difficulty, protein structure prediction is a very important problem to solve given its potential for many, many applications. Perhaps one of the most important applications is in structure-based drug design. When developing a drug, knowing its 3D structure not only helps us understand how the drug will interact with the target or potential off-targets, it will also help us test more potential drug candidates, refine and create better drugs, and ultimately speed the process up. The Soviets founded a research institute to solve this protein structure prediction problem once and for all in the 1960s. The Soviet Union ended 3 decades ago but the research institute lives on (<a href="http://www.ibch.ru/en/about" target="_blank">here</a>). The difficult of biology forever endures.

[Next lesson](ab_initio){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Citations

[^1]: Wade W. 2002. Unculturable bacteria--the uncharacterized organisms that cause oral infections. Journal of the Royal Society of Medicine, 95(2), 81–83. https://doi.org/10.1258/jrsm.95.2.81

[^2]: Severe acute respiratory syndrome coronavirus 2 isolate Wuhan-Hu-1, complete genome. https://www.ncbi.nlm.nih.gov/nuccore/MN908947

[^3]: Annotated Severe acute respiratory syndrome coronavirus 2 isolate Wuhan-Hu-1, complete genome. https://go.usa.gov/xfzMM
