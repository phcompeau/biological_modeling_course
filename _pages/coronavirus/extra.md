---
permalink: /coronavirus/extra
title: "VMD Plugin: NAMD Energy"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Extra (structure intro): Four levels of protein structure

These amino acids are linked together to form a protein as a amino acid chain, or polypeptide chain, that will typically undergo folding to obtain a 3D structure. In the first figure below, the general shape of the amino acid is shown: a central alpha-Carbon, a carboxyl group, an amino group, and finally one of 20 side-groups that differentiate the amino acids. Each amino acid is linked to the next by a peptide bond, and it is this connection and alpha-Carbon that make up the protein backbone, as shown in the second figure. The side groups of each amino acid are responsible for amino acid's chemical properties. These chemical properties allow the amino acids to interact with each other and fold into the 3D structure.

We stated that the overall 3D structure (tertiary structure) of the protein is dictated by the interactions of the side chains. Even when we unfold, or denature, a protein, it will eventually fold back into essentially the same shape because of these interactions.

![image-center](../assets/images/AminoAcid.png){: .align-center}
{: style="font-size: medium;"}

![image-center](../assets/images/Backbone.png){: .align-center}
{: style="font-size: medium;"}



Protein structure are separated into four different levels of description. The most basic description, the **primary structure**, refers the specific amino acid sequence of the polypeptide chain. Below is an example of the human hemoglobin subunit alpha and its primary structure.

![image-center](../assets/images/PrimaryStructureExample.png){: .align-center}
The primary structure, or specific amino acid sequence, of human hemoglobin subunit alpha. Each letter corresponds to one of the twenty amino acids. Protein structure from: https://www.rcsb.org/structure/1SI4.
{: style="font-size: medium;"}

The **secondary structure** describes the highly regular substructures in the protein. Essentially, they are the 3D structures of local amino acids groups within the protein and spontaneously form during the folding process. In a sense, they are the intermediate structures that form before the overall protein structure. The two main substructures, shown in the figure below, are alpha-helices (left) and beta-sheets (right). Alpha-helices are formed when local amino acids fold into a tube-like structure. Beta-sheets are when the local amino acids interact by lining up side-by-side, forming a sheet-like structure. The formation of these secondary structures help with the overall process of folding.

![image-center](../assets/images/SecondaryStructure.png){: .align-center}
General shape of secondary structure alpha-helices (left) and beta-sheets (right). Source: Cornell, B. (n.d.). [https://ib.bioninja.com.au/higher-level/topic-7-nucleic-acids/73-translation/protein-structure.html](https://ib.bioninja.com.au/higher-level/topic-7-nucleic-acids/73-translation/protein-structure.html)
{: style="font-size: medium;"}

The **tertiary structure** describes the overall 3D shape of the protein that results from the fully-folded polypeptide chain. This is what we think of as the "shape" of the protein. In a sense, it is the combination of all the secondary structures and linkages that creates the tertiary structure. Below is the tertiary structure of human hemoglobin subunit alpha.

![image-center](../assets/images/TertiaryStructureExample.png){: .align-center}
Tertiary structure of human hemoglobin subunit alpha. Within the structure are multiple alpha-helices secondary structures. Protein structure from: [https://www.rcsb.org/structure/1SI4](https://www.rcsb.org/structure/1SI4).
{: style="font-size: medium;"}

Finally, some proteins have a **quaternary structure**, which describes the protein’s interaction with other copies of itself to form a single functional unit, or a multimer. Many proteins do not have a quaternary structure and functions as an independent monomer.

Proteins are can often be divided into protein domains. Domains are distinct functional/structural units within the protein and are typically responsible for a specific interaction or function. For example, The Sars-CoV-2 S protein has a Receptor Binding Domain (RBD) that is responsible for interacting with ACE2. The rest of the protein does not come into contact with ACE2.

## Extra ab initio

* Somewhere need trimer of dimers for spike protein

* Models published before crystallography can be found here: [SSGCID Models](https://www.ssgcid.org/cttdb/molecularmodel_list/?target__icontains=BewuA)

* Need to define side chain: The side chain, commonly referred to as the R group or side group, is the part of an amino acid that distinguishes the twenty amino acids that are have identical structures everywhere else. Directly connected to the central, alpha-Carbon, the side chain is fully responsible for the chemical properties of each amino acid.

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

* Move the below to protein structure section

When we are talking about the energy of a system or molecule, we are referring to its potential energy.  More specifically, **potential energy** is defined as the energy stored within an object due to its position, state, and arrangement. In molecular mechanics, the potential energy is made up of the sum of bonded energy (energy related to covalent bonds) and nonbonded energy (energy from interactions not from covalent bonds) for all atoms in the molecule.

Bonded energy can be broken down into three terms: bond, angle, and dihedral. We will explain here what each term represents as additional information, but this is beyond the scope of this module as it gets extremely complicated. The bond term describes the potential between a pair of covalently bonded atoms and is represented by harmonic potential, or in simpler terms, bond stretching. The angle term describes the potential between a triplet of atoms covalently bonded like a ‘V’ shape and is represented by angle vibration. Finally, the dihedral term describes the potential between a consecutively bonded quadruplet of atoms and are represented by the angular spring between the plane formed by the first three atoms and the plane formed by the last three atoms [^TCBG].

Nonbond energy can be broken down into two terms: electrostatic interactions (Coulomb potential) and van der Waals interactions (Lennard-Jones potential). **Electrostatic interaction** refers to the attraction and repulsion force from the electric charge of the atoms. To calculate the total electrostatic interaction, we need to consider this interaction between every atom pair within the molecule. Just like proteins, atoms are dynamic systems. The electrons are constantly circling around the nucleus and at any given time, they could be localized on one side. This will cause the atom to have a temporary negative charge on one side and positive charge on the other side. In addition, nearby atoms can also influence the electron cloud. These temporary charges are referred to as **induced dipoles**. **Van der Waals** interaction refers to the attraction and repulsion force between atoms from these induced dipoles.
