---
permalink: /coronavirus/conclusion
title: "Conclusion: From Static Protein Analysis to Molecular Dynamics"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Modeling protein bonds using tiny springs

In this lesson, we transition from the static study of proteins to the field of **molecular dynamics (MD)**, in which we simulate the movement of proteins' molecules, atoms, along with their interactions as they move.

You may think that simulating the *movements* of proteins with hundreds of amino acids will be a hopeless task. After all, protein structure prediction is difficult enough! Yet what makes structure prediction so challenging is that the "search space" of potential shapes is so enormous. Once we have established the static structure of a protein, its dynamic behavior will not allow it to deviate greatly from this static structure, and so the space of potential structures is automatically narrowed down to those that are similar to the static structure.

A protein's molecular bonds are constantly vibrating, stretching and compressing, much like that of an oscillating mass-spring system like that shown in the figure below. Bonded atoms are held together by sharing electrons, but is held at specific bond length due to the attraction and repulsion forces of the negatively charged electrons and positively charged nucleus. When you bring the atoms closer together than the equilibrium, they will "bounce back" with a greater repulsive force.

![image-center](../assets/images/mass-spring.gif){: .align-center}
A mass-spring system in which a mass is attached to the end of a spring. The more we move the mass from its equilibrium, the more it will be repelled past its equilibrium. Courtesy: [flippingphysics.com](flippingphysics.com).
{: style="font-size: medium;"}

With this model of a protein bond in mind, we will imagine nearby alpha carbons of a protein structure to be connected by springs; this type of model is called an **elastic network model (ENM)**. Because distant atoms will not influence each other, we will only connect two alpha carbons if they are within some threshold distance of each other (the default threshold used by ProDy is 7 angstroms).

Another major strength of ProDy is its capabilities for protein dynamics analysis. Specifically, it implements a **Gaussian network model (GNM)**, an ENM for molecular dynamics. This model is called "Gaussian" because protein bond movements follow normal (Gaussian) distributions around their equilibria. Furthermore, this model is **isotropic**, meaning that it only considers the magnitude of force exerted on the springs between nearby molecules and ignores any global effect on the orientations of these forces.

Although it may seem that atomic movements are frantic and random, the movements of protein atoms are in fact often heavily coordinated, owing to the evolution of the proteins to perform replicable tasks. As a result, the movements of these particles are often correlated and can be summarized using a small number of descriptors, or "modes", which stabilize the system. The paradigm resulting from this insight is called **normal mode analysis (NMA)** and powers the elastic model that ProDy implements. The idea of representing a complex system with a small number of variables is one that we will return to in a later module, but the details rely on some advanced linear algebra and are too technical for our treatment here. For those interested, a full treatment of the mathematics of GNMs can be found in the chapter at [https://www.csb.pitt.edu/Faculty/bahar/publications/b14.pdf](https://www.csb.pitt.edu/Faculty/bahar/publications/b14.pdf).

<!-- NMA of proteins is based on the theory that the lowest frequency vibrational normal modes are the most functionally relevant, describing the largest movement within the protein [^Skjaerven].-->

By running molecular dynamics simulations, we obtain another way to compare two proteins. If two proteins have different patterns of fluctuation under perturbation, then we have a clear indication that their structure is different. With this in mind, we will use ProDy in the following tutorial to perform NMA calculations as a final method of comparing the SARS-CoV-2 and SARS-CoV spike proteins. In what follows, we will then discuss the resulting analyses.

[Visit tutorial](tutorial_GNM){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Molecular dynamics analyses of SARS-CoV and SARS-CoV-2 spike proteins

* START HERE

* In this section, we explain the results of the NMA that ProDy performed 

* Also: contact map! Add this to tutorial

In the tutorial, we generated four visualizations of how the SARS-CoV-2 S protein fluctuates. Using ProDy, we performed GNM Calculations on the SARS S protein using the PDB entry(<a href="http://www.rcsb.org/structure/5xlr" target="_blank">5xlr</a>). In addition, we also performed the calculations on a single chain of the S protein for a more thorough comparison. Here, we will explain how to interpret the results and compare them to analyze the differences and similarities between the two proteins.

### Cross-Correlation

Protein residue cross-correlation shows the correlation between the fluctuations/displacement of residues. This graphical representation shows how the residues will move relative to each other. The pair is assigned the value of 1 if the fluctuations are completely correlated (move in the same direction), the value of -1 if the fluctuations are completely anticorrelated (move in opposite directions), and a value of 0 if uncorrelated (movements do not affect each other). It is typical to see a diagonal of strong cross-correlation because movements in the residue will almost always affect its direct neighbors. Positive correlations coming off the diagonal represents correlations between contiguous residues and are characteristics of secondary structures because residues in secondary structures tend to move together. Common patterns for secondary structures are triangular structures for helices and plume structures for strands. Off-diagonal correlation and anticorrellations may potential represent interesting interactions between non-contiguous residues and domains. From our results, we see that the SARS-CoV-2 and SARS S protein fluctuate similarly, supporting that they are similar structures.

![image-center](../assets/images/CrossCorr.png){: .align-center}
This figure shows the cross-correlation heat maps of the SARS-CoV-2 S protein (top-left), SARS S protein (top-right), single-chain of the SARS-CoV-2 S protein (bottom-left), and single-chain of the SARS S protein (bottom-right). The x-axis and y-axis represent the amino acid residues. The map shows every residue pair in the structure and the colors represent the correlation in the fluctuations of residues. A value of 1.0 (red) means that the residues will fluctuate together in the same direction. A value of -1.0 (dark blue) means that the residues will fluctuate together in opposite directions. A value of 0.0 means no relations between the fluctuations of the residues. We see that SARS-CoV-2 and SARS S proteins have very similar maps.
{: style="font-size: medium;"}

### Slow Mode Shape

NMA is based on the idea that the lowest frequency modes describe the largest movement in the structure. Below is the plot of the lowest frequency (slowest) mode calculated by ProDy. Here, the fluctuations are in arbitrary or relative units, but can interpreted as greater amplitudes represent regions of greater fluctuations. The sign of the value represents relative direction of the fluctuation, meaning that the plots can be flipped when comparing between different proteins. In the SARS-CoV-2 Chain A figure, we can see that the protein region between residues 200 and 500 is the most mobile. This region overlaps with where the RBD is located on the chain, between residues 331 to 524. This is important because it indicates the RBD being a mobile part of the S protein. Based on our results, we see that both S proteins have the same regions of great fluctuations, supporting that they have similar structures.

* Note to self: this matches our intuition that the RBD would need to be flexible in order to "catch" a moving ACE2 enzyme. The rest of the protein could be anchored to the surface of the virus, while this one is able to rotate.

![image-center](../assets/images/SlowMode.png){: .align-center}
This figure shows the slow mode plots of the SARS-CoV-2 S protein (top-left), SARS S protein (top-right), single-chain of the SARS-CoV-2 S protein (bottom-left), and single-chain of the SARS S protein (bottom-right). The x-axis represent the amino acid residues and the y-axis represents the fluctuations in relative units.  From the single-chain plots for both SARS-CoV-2 and SARS, we see that the residues between 200 – 500 fluctuate the most. The plots between SARS-CoV-2 and SARS are very similar, indicating similar protein fluctuations.
{: style="font-size: medium;"}

### Square Fluctuation

The slow mode square-fluctuation is calculated by multiplying the square of the slow mode with the variance along the mode. In this case, all the values will be positive, but the interpretation remains the same as the slow mode plot, where greater amplitudes represent regions of greater fluctuations and motions.

![image-center](../assets/images/SqFlucts.png){: .align-center}
This figure shows the plots of the slow mode square fluctuation of the SARS-CoV-2 S protein (top-left), SARS S protein (top-right), single-chain of the SARS-CoV-2 S protein (bottom-left), and single-chain of the SARS S protein (bottom-right). The x-axis represent the amino acid residues and the y-axis represents the fluctuations in relative units. The interpretation is the same as the slow mode plot, but with only positive values. The plots between SARS-CoV-2 and SARS are very similar, indicating similar protein fluctuations.
{: style="font-size: medium;"}

### Comparing Results

From all four results, we see that SARS-CoV-2 and SARS S proteins are structurally very similar. This is, perhaps, not a surprise given that they are similar in sequence and have the same function of targeting ACE2.


## ANM Analysis of the RBD

* GNM typically outperforms ANM but ANM has the benefit of being anisotropic, meaning that it takes the directions of protein dynamics into account. That is, it's not just interested in the magnitude of forces acting on molecules but their directions too.

The anisotropic counterpart to GNM, where direction does matter, is called **anisotropic network model (ANM)**. In ANM, the direction of the fluctuations are also considered. Although ANM includes directionality, ANM typically performs worse than GNM when compared with experimental data [^Yang]. Nonetheless, ANM calculations are useful because of the added directionality. In fact, we can use it to create animations depicting the range of motions and fluctuations of the protein.

<hr>

In this tutorial, we will use NMWiz, a GUI for ProDy and is available as a plugin for VMD, to perform ANM calculations and create the animation of the SARS-CoV-2 (chimeric) RBD using the PDB entry <a href="http://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a>.

[Visit tutorial](tutorial_ANM){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

From the tutorial, we were able to generate the cross-correlation map and square fluctuation of the SARS-CoV-2 RBD. The interpretation of these results are identical to the GNM analysis above. Following the same steps, we performed ANM analysis on the SARS RBD using the PDB entry SARS RBD (<a href="http://www.rcsb.org/structure/2ajf" target="_blank">2ajf</a> for comparison.

![image-center](../assets/images/ANMResults.png){: .align-center}
This figure shows the cross-correlation map (top) and the square fluctuation plot (bottom) of SARS-CoV-2 and SARS RBD using ANM. The y-axis of the square fluctuation plot represents how much the residues fluctuate in relative units. Like the results from the GNM analysis, the map and plot are very similar between the two RBDs, indicating that they are structurally similar.
{: style="font-size: medium;"}

Perhaps unsurprisingly, the maps and plots show very small differences between SARS-CoV-2 and SARS RBD, just like in the GNM calculations for the S proteins. This indicates that the two RBDs are structurally similar.

Using NMWiz and VMD, we also created animations of the protein fluctuation calculated through ANM analysis. The following animations are of the SARS-CoV-2 RBD/SARS RBD (purple) and ACE2 (green). Important residues from the three sites of conformational differences from the previous lessens are also colored.

*It is important to note that fluctuation calculated by ANM provides information on possible movement and flexibility, but does not depict actual protein movements.*

### SARS-CoV-2 Spike Chimeric RBD (6vw1):

|SARS-CoV-2 (Chimeric) RBD|Purple|
|:------------------------|:-----|
|Resid 476 to 486 (Loop)|Yellow|
|Resid 455 (Hotspot 31|Blue|
|Resid 493 (Hotspot 31|Orange|
|Resid 501 (Hotspot 353)|Red|
|--------------------------|-----|
|ACE2|Green|
|Resid 79, 82, 83 (Loop)|Silver|
|Resid 31, 35 (Hotspot 31)|Orange|
|Resid 38, 353 (Hotspot 353)|Red|


<center>
<iframe width="640" height="360" src="../assets/6vw1_B&F.mp4" frameborder="0" allowfullscreen></iframe>
</center>

### SARS Spike RBD (2ajf):

|SARS RBD|Purple|
|:-------|:-----|
|Resid 463 to 472 (Loop)|Yellow|
|Resid 442 (Hotspot 31|Orange|
|Resid 487 (Hotspot 353|Red|
|--------|-----|
|ACE2|Green|
|Resid 79, 82, 83 (Loop)|Silver|
|Resid 31, 35 (Hotspot 31)|Orange|
|Resid 38, 353 (Hotspot 353)|Red|

<center>
<iframe width="640" height="360" src="../assets/2ajf_B&F.mp4" frameborder="0" allowfullscreen></iframe>
</center>

Using both the GNM and ANM approaches for normal mode analysis of SARS-CoV-2 S protein, we saw that it is structurally very similar to the SARS S protein. As we have stated in the <a href="structural_diff">Structural and ACE2 Interaction Differences</a> and <a href="NAMD">Interaction Energy with ACE2</a> lessons, the structural differences can be very subtle yet still contribute greatly with ACE2 binding affinity.


## Overview

* In this module, we discussed the importance of the 3D (tertiary) structure of the protein and the many experimental methods of determining protein structure. Unfortunately, these methods requires time to collect physical samples, to run the complicated and expensive experiment, and to computationally store it into the protein data base. We then went into the complex problem of protein structure prediction, and the two main approaches of obtaining the 3D structure from the amino acid sequence of the protein, *ab initio* and homology modeling. Perhaps due to the severity of the outbreak and global contributions, studies after studies on SARS-CoV-2 were released along with experimentally determined 3D structures of the SARS-CoV-2 S protein.

* With the 3D structures available, we used several protein analysis tools to compare the SARS-CoV-2 S protein with the SARS S protein and visualize the results. We learned about three subtle structural differences within the receptor binding domain (RBD) of the S proteins that appear to increase the binding affinity of the SARS-CoV-2 S protein and ACE2, which may be one of the reasons why SARS-CoV-2 is more infectious.

* Unfortunately, biology is extremely complex. There is so much more to the story than just the protein structure and binding affinity of the S protein. We need to consider things like what happens after the S protein binds to ACE2, how does the virus enter the cell, how does it replicate itself, how does it combat our immune systems. As a conclusion, we will explore how SARS-CoV-2 hides from our immune system.

## Glycans

The surface of viruses and host cells are not smooth, but rather “fuzzy”. This is because the surface is decorated by structures called glycans, which consists of numerous monosaccharides linked together by glycosidic bonds. Although this definition is also shared with polysaccharides, glycans typically refer to the carbohydrate portion of glycoproteins, glycolipids, or proteoglycans [^Dwek]. Glycans have been found to have structural and modulatory properties and are crucial in recognition events, most commonly by glycan-binding proteins (GBPs) [^Varki]. In viral pathogenesis, glycans on host cells act as primary receptors, co-receptors, or attachment factors that are recognized by viral glycoproteins for viral attachment and entry. On the other hand, the immune system can recognize foreign glycans on viral surfaces and target the virus [^Raman]. Unfortunately, some viruses have evolved methods that allow them to effectively conceal themselves from the immune system. One such method is a **glycan shield**. By covering the viral surface and proteins with glycans, the virus can physically shield itself from antibody detection. Because the virus replicates by hijacking the host cells, the glycan shield can consist of host glycans and mimic the surface of a host cell. A notorious virus that utilizes glycan shielding is HIV. In the case of SARS-CoV-2, the immune system recognizes the virus through specific areas, or antigens, along the S protein. The S protein, however, is a glycoprotein, meaning that it is covered with glycans which can shield the S protein antigens from being recognized.

In our last tutorial, we will use VMD to try to visualize the glycans of SARS-CoV-2 S protein.

[Visit tutorial](tutorial_glycans){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

From the visualization we created in the tutorial, we can see that glycans are present all around the S protein. In fact, the glycans cover around 40% of the Spike protein[^Grant]! This raises an important question: If the glycans on the S protein can hide from antibodies, won't it get in the way of binding with ACE2? Such glycosylation does not hinder the Spike protein’s ability to interact with human ACE2 because the Spike protein is able to adopt an open conformation, allowing a large portion of the RBD being exposed. In the figure below, we compared the SARS-CoV-2 Spike in its closed conformation (PDB entry: <a href="https://www.rcsb.org/structure/6vxx" target="_blank">6vxx)</a>) and SARS-CoV-2 Spike in its open conformation (PDB entry: <a href="https://www.rcsb.org/structure/6VYB" target="_blank">6vyb</a>). The presumed glycans are shown in red. Notice how the RBD in the orange chain is much more exposed in the open conformation.

![image-center](../assets/images/GlycanComparison.png){: .align-center}
This figure shows the SARS-CoV-2 S protein in the closed conformation (left) and the protein with an open conformation of one chain (right) using the PDB entries 6vxx and 6vyb, respectively. The protein chains are shown in dark orange, yellow, and green. The presumed glycans are shown in red. Notice how in the open conformation, the RBD of one of the chain is pointed upwards, exposing it for ACE2 interactions.
{: style="font-size: medium;"}

Glycans are generally very flexible and have large internal motions that makes it difficult to get an accurate description of their 3D shapes. Fortunately, molecular dynamics (MD) simulations can be employed to predict the motions and shapes of the glycans. With a combination of MD and visualization tools (i.e. VMD), very nice looking snapshots of the glycans on the S protein can be created.

![image-center](../assets/images/Glycan_Grant.png){: .align-center}
Snapshots from molecular dynamics simulations of the SARS-CoV-2 S protein with different glycans shown in green, yellow, orange, and pink. Source: https://doi.org/10.1101/2020.04.07.030445 [^Grant]
{: style="font-size: medium;"}

## SARS-CoV-2 Vaccine

Much of vaccine development for SARS-CoV-2 has been focused on the S protein given how it facillitates the viral entry into host cells. In vaccine development, it is critical to understand every strategy that the virus employs to evade immune response. As we have discussed, SARS-CoV-2 hides its S protein from antibody recognition through glycosylation, creating a glycan shield around the protein. In fact, the "stalk" of the S protein has been found to be completely shielded from antibodies and other large molecules. In contrast, the "head" of the S protein is vulnerable because of the RBD is less glycosylated and becomes fully exposed in the open conformation. Thus, there is an opportunity to design small molecules that target the head of the protein [^Casalino]. Glycan profiling of SARS-CoV-2 is extremely important in guiding vaccine development as well as improving COVID-19 antigen testing [^Watanabe].

<hr>

This concludes the final lesson of the third module. If you would like to learn more about COVID-19 and SARS-CoV-2, check out this free online course: *<a href="https://sites.google.com/view/sarswars/home" target="_blank">SARS Wars: A New Hope</a>* by <a href="https://www.cs.cmu.edu/~cjl/" target="_blank">Christopher James Langmead</a>.


[Exercises](exercises){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Extra

* Note to self: need to make a point about negative discovery here -- we don't find any big differences but that doesn't mean that it's analysis not worth running.

* Another example of declaring protein structure a "solved" problem just being tip of iceberg.

[^Dwek]: Dwek, R.A. Glycobiology: Toward Understanding the Function of Sugars. Chem. Rev. 96(2),  683-720 (1996). https://doi.org/10.1021/cr940283b

[^Varki]: Varki A, Lowe JB. Biological Roles of Glycans. In: Varki A, Cummings RD, Esko JD, et al., editors. Essentials of Glycobiology. 2nd edition. Cold Spring Harbor (NY): Cold Spring Harbor Laboratory Press; 2009. Chapter 6. https://www.ncbi.nlm.nih.gov/books/NBK1897/

[^Raman]: Raman, R., Tharakaraman, K., Sasisekharan, V., & Sasisekharan, R. 2016. Glycan-protein interactions in viral pathogenesis. Current opinion in structural biology, 40, 153–162. https://doi.org/10.1016/j.sbi.2016.10.003

[^Grant]: Grant, O. C., Montgomery, D., Ito, K., & Woods, R. J. 2020. Analysis of the SARS-CoV-2 spike protein glycan shield: implications for immune recognition. bioRxiv. https://doi.org/10.1101/2020.04.07.030445

[^Casalino]: Casalino, L., Gaieb, Z., Dommer, A. C., Harbison, A. M., Fogarty, C. A., Barros, E. P., Taylor, B. C., Fadda, E., & Amaro, R. E. 2020. Shielding and Beyond: The Roles of Glycans in SARS-CoV-2 Spike Protein. bioRxiv : the preprint server for biology, 2020.06.11.146522. https://doi.org/10.1101/2020.06.11.146522

[^Watanabe]: Watanabe, Y., Allen, J., Wrapp, D., McLellan, J., Crispin, M. Site-specific glycan analysis of the SARS-CoV-2 spike. Science 369, 330-333. https://doi.org/10.1126/science.abb9983

[^Skjaerven]: Skjaerven, L., Hollup, S., Reuter, N. 2009. Journal of Molecular Structure: THEOCHEM 898, 42-48. https://doi.org/10.1016/j.theochem.2008.09.024

[^Yang]: Yang, L., Song, G., Jernigan, R. 2009. Protein elastic network models and the ranges of cooperativity. PNAS 106(30), 12347-12352. https://doi.org/10.1073/pnas.0902159106
