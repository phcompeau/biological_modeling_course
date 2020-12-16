---
permalink: /coronavirus/NAMD
title: "Quantifying the Interaction Energy Between the SARS-CoV-2 Spike Protein and ACE2"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Energy functions measure quality of protein binding

In the previous lesson, we visualized three regions of conformational differences between the RBD-ACE2 complex formed by each of SARS-CoV and SARS-CoV-2. We now turn our attention to quantifying how much each of these regions affect binding affinity.

In part 1 of this module, we looked for the 3-D structure that best "explained" an amino acid sequence by finding the structure with lowest potential energy, i.e., that is the most stable. In part, this means that we were looking for the structure that has many attractive bonds present between amino acids and few repulsive bonds.

To quantify whether two molecules such as a spike protein and ACE2 bond well, we can apply the same analysis of the potential energy of the complex. To do so, we need a **force field**, i.e., an energy function with a collection of parameters to determine the energy of a given structure based on the positional relationships between atoms [^charmm]. There are many different force fields depending on the specific type of system being studied (e.g. DNA, RNA, lipids, proteins). There are many different approaches for generating a force field; for example, *<a href=" https://www.charmm.org/" target="_blank">Chemistry at Harvard Macromolecular Mechanics (CHARMM)</a>* offers a popular collection of force fields for molecular dynamics.

In the following tutorial, we will compute the energy of our bound spike protein-ACE2 complex for the two viruses and see how the three regions we identified in the previous lesson contribute to this energy. To do so, we will employ <a href="https://www.ks.uiuc.edu/Research/namd/" target="_blank">NAMD</a>, a molecular dynamics program that was designed for high-performance large biomolecular system simulations and is most commonly used with VMD via a plugin called <a href="https://www.ks.uiuc.edu/Research/vmd/plugins/namdenergy/" target="_blank">NAMD Energy</a>. This plugin will allow us to isolate a specific region to evaluate how much this local region contributes to the overall energy of the complex.

[Visit tutorial](tutorial_NAMD){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Differences in interaction energy with ACE2 between SARS and SARS-CoV-2

Recall from the previous lesson that we found three regions of interest that differ between SARS-CoV and SARS-CoV-2 in terms of their spike proteins' binding with ACE2: a loop site in the ACE2 binding ridge, hotspot 31, and hotspot 353.

Using the methods described in the tutorial, we calculated the interaction energies for each of the three regions as well as for the overall complexes for both SARS-CoV and SARS-CoV-2. The results are shown in the table below.

![image-center](../assets/images/NAMDEnergy.png){: .align-center}
ACE2 interaction energies of SARS-CoV-2 RBD and SARS RBD. The PDB files contain two biological assemblies, or instances, of the corresponding structure. The first instance includes Chain A (ACE2) and Chain E (RBD), and the second instance includes Chain B (ACE2) and Chain F (RBD). The overall interactive energies between the RBD and ACE2 are shown in green. Then, the individual interaction energies of the important residues from the loop site (yellow), hotspot 31 (red), and hotspot 353 (grey) are shown. Total energy is computed as the sum of electrostatic interactions and van der Waals forces (vdw).
{: style="font-size: medium;"}

We can see in the table that the overall attractive interaction energy between the RBD and ACE2 is lower for SARS-CoV-2 than for SARS-CoV, which supports previous studies that have found the SARS-CoV-2 spike protein to have higher affinity with ACE2.

Furthermore, all of the three regions of interest correspond to a lower energy in SARS-CoV-2 than in SARS-CoV. Of these, hotspot 31 (red) has the greatest negative contribution, followed by hotspot 353 (blue), and then the loop site (yellow). We now have evidence that the conformational changes in the three sites do indeed increase the binding affinity between the spike protein and ACE2.

We now have concrete evidence that SARS-CoV-2 is more effective at binding to human cells than SARS. This having said, we should be careful; it would require additional experimental work to demonstrate that the improved binding of SARS-CoV-2 translates into greater infectiousness.

Part of the reason for our cautiousness is that proteins are not fixed objects but rather dynamic structures whose shape is subject to small changes. After all, life is about change. In the module's conclusion, we will therefore learn how to analyze the dynamics of a protein's movements as it changes within its environment.

[Next lesson](conclusion){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Extra

* Move following to first linked tutorial

NAMD needs to utilize the information in the force field to calculate the potential energy of a protein. To do this, it needs a **protein structure file (PSF)**. A PSF, which is molecule-specific, contains all the information required to apply a force field to a molecular system [^PSF]. Fortunately, there are programs that can generate a PSF given the force field and protein structure (PDB file).

In the following tutorial, we will show how to calculate the interaction energy between the SARS-CoV-2 (Chimeric) RBD and ACE using the PDB entry <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a> and the NAMD Energy plugin within VMD. To do this, we will be using a force field from CHARMM that is included in VMD package as well as the AutoPSF plugin to generate the necessary PSF file. Finally, we will see how much the loop site contributes to the interaction energy between SARS-CoV-2 RBD and ACE2.

* In protein structure, need force field = energy function

* Should be clear that total = electrostatic + van der Waals

* From Chris: For more information of how to calculate the energies and the functions for potential energy, click <a href="https://www.ks.uiuc.edu/Research/namd/2.9/ug/node22.html" target="_blank">here</a>.

* Move the below to protein structure section

When we are talking about the energy of a system or molecule, we are referring to its potential energy.  More specifically, **potential energy** is defined as the energy stored within an object due to its position, state, and arrangement. In molecular mechanics, the potential energy is made up of the sum of bonded energy (energy related to covalent bonds) and nonbonded energy (energy from interactions not from covalent bonds) for all atoms in the molecule.

Bonded energy can be broken down into three terms: bond, angle, and dihedral. We will explain here what each term represents as additional information, but this is beyond the scope of this module as it gets extremely complicated. The bond term describes the potential between a pair of covalently bonded atoms and is represented by harmonic potential, or in simpler terms, bond stretching. The angle term describes the potential between a triplet of atoms covalently bonded like a ‘V’ shape and is represented by angle vibration. Finally, the dihedral term describes the potential between a consecutively bonded quadruplet of atoms and are represented by the angular spring between the plane formed by the first three atoms and the plane formed by the last three atoms [^TCBG].

Nonbond energy can be broken down into two terms: electrostatic interactions (Coulomb potential) and van der Waals interactions (Lennard-Jones potential). **Electrostatic interaction** refers to the attraction and repulsion force from the electric charge of the atoms. To calculate the total electrostatic interaction, we need to consider this interaction between every atom pair within the molecule. Just like proteins, atoms are dynamic systems. The electrons are constantly circling around the nucleus and at any given time, they could be localized on one side. This will cause the atom to have a temporary negative charge on one side and positive charge on the other side. In addition, nearby atoms can also influence the electron cloud. These temporary charges are referred to as **induced dipoles**. **Van der Waals** interaction refers to the attraction and repulsion force between atoms from these induced dipoles.

[^TCBG]: https://www.ks.uiuc.edu/Research/namd/2.9/ug/node22.html

[^charmm]: https://www.charmmtutorial.org/index.php/The_Energy_Function

[^PSF]: https://www.ks.uiuc.edu/Training/Tutorials/namd/namd-tutorial-unix-html/node23.html
