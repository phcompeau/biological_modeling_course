---
permalink: /coronavirus/NAMD
title: "Quantifying the Interaction Energy Between the SARS-CoV-2 Spike Protein and ACE2"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Energy functions measure quality of protein binding

In the previous lesson, we visualized the RBD-ACE2 complexes formed by SARS-CoV and SARS-CoV-2 and examined three regions of conformational differences. We presented *qualitative* explanations from the literature as to why these differences may help SARS-CoV-2 bind more strongly to the human ACE2 enzyme, but a theme of this course is to justify our arguments *quantitatively*. Our question, then, is how to measure the strength of protein binding in a local region of the complex.

In part 1 of this module, we searched for the tertiary structure that best "explains" a protein's primary structure by looking for the structure with the lowest potential energy (i.e., the one that is the most chemically stable). In part, this means that we were looking for the structure that incorporates many attractive bonds present and few repulsive bonds.

To quantify whether two molecules bond well, we will borrow this idea and compute the potential energy of the complex formed by the two molecules. If two molecules bond together tightly, then the complex will have a very low potential energy. In turn, if we compare the SARS-CoV-2 RBD-ACE2 complex against the SARS-CoV RBD-ACE2 complex, and we find that the potential energy of the former is significantly smaller, then we can conclude that it is more stable and therefore bonded more tightly. This result would provide strong evidence for increased infectiousness of SARS-CoV-2.

In the following tutorial, we will compute the energy of the bound spike protein-ACE2 complex for the two viruses and see how the three regions we identified in the previous lesson contribute to the total energy of the complex. To do so, we will employ <a href="https://www.ks.uiuc.edu/Research/namd/" target="_blank">NAMD</a>, a program that was designed for high-performance large system simulations of biological molecules and is most commonly used with VMD via a plugin called <a href="https://www.ks.uiuc.edu/Research/vmd/plugins/namdenergy/" target="_blank">NAMD Energy</a>. This plugin will allow us to isolate a specific region to evaluate how much this local region contributes to the overall energy of the complex.

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

[^TCBG]: https://www.ks.uiuc.edu/Research/namd/2.9/ug/node22.html

[^charmm]: https://www.charmmtutorial.org/index.php/The_Energy_Function
