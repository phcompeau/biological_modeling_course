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

Although it may seem that atomic movements are frantic and random, the movements of protein atoms are in fact often heavily coordinated, owing to the evolution of the proteins to perform replicable tasks. As a result, the oscillations of these particles are often highly structured and can be summarized using a combination of functions explaining them, or "modes". (For those familiar with Fourier analysis, this is comparable to the observation that a function under certain conditions can be written as a sum of sine and cosine waves.) The paradigm resulting from the insight of breaking down oscillations into a comparatively small number of modes that summarize them is called **normal mode analysis (NMA)** and powers the elastic model that ProDy implements.

We will say more about NMA later in this lesson, but for now we note that the idea of representing a complex system with a small number of variables is one that we will return to in a later module, but the details rely on some advanced linear algebra and are too technical for our treatment here. For those interested, a full treatment of the mathematics of GNMs can be found in the chapter at [https://www.csb.pitt.edu/Faculty/bahar/publications/b14.pdf](https://www.csb.pitt.edu/Faculty/bahar/publications/b14.pdf).

<!-- NMA of proteins is based on the theory that the lowest frequency vibrational normal modes are the most functionally relevant, describing the largest movement within the protein [^Skjaerven].-->

By running molecular dynamics simulations, we obtain another way to compare two proteins. If two proteins have different patterns of fluctuation under perturbation, then we have a clear indication that their structure is different. With this in mind, we will use ProDy in the following tutorial to perform NMA calculations as a final method of comparing the SARS-CoV-2 and SARS-CoV spike proteins. We also will use ProDy to compute a contact map, if you are interested in doing this after our discussion of contact maps in a [previous lesson](multiseq#contact-maps-and-qres). When we return from the tutorial, we will explain each of the analyses that we perform in the tutorial.

[Visit tutorial](tutorial_GNM){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Molecular dynamics analyses of SARS-CoV and SARS-CoV-2 spike proteins using GNM

In the tutorial, we used ProDy to generate visualizations of how the SARS-CoV-2 spike protein fluctuates compared to that of SARS-CoV. Here, we will explain how to interpret the results and compare them to analyze the similarities between the two proteins.

### Cross-Correlation

Much as a contact map indicated which amino acids in a protein structure are close to each other, we will use a **cross correlation map** to show whether the *movements* of different amino acids are coordinated as the protein flexes. A matrix *M* receives a value at *M*(*i*, *j*) equal to the correlation between the movements of the *i*-th and *j*-th amino acids in a protein structure. The values of this matrix are decimals ranging from -1 to 1. *M*(*i*, *j*) is equal to 1 if the movements are completely correlated (both amino acids always move in the same direction), a value of -1 if the movements are completely anticorrelated (both amino acids always move in opposite directions), and a value of 0 if the movements are completely uncorrelated.

Much as the contact map typically has many values equal to 1 near the main diagonal, we commonly see a diagonal of strong cross-correlation values (i.e., either close to -1 or close to 1) because movements in an amino acid will almost always affect nearby amino acids.

Positive correlations near the diagonal represents correlations between contiguous residues and are characteristics of secondary structures (e.g., alpha helices and beta sheets), in which amino acids tend to move together. Correlations and anticorrellations off the diagonal (i.e., for amino acids distant from each other in the protein structure) may potential represent interesting interactions between non-contiguous residues and domains for further study.

From our results, we see that the SARS-CoV-2 and SARS S protein fluctuate similarly, supporting that they not only have similar structures, but similar dynamics as well.

![image-center](../assets/images/CrossCorr.png){: .align-center}
The cross-correlation heat maps of the SARS-CoV-2 spike protein (top-left), SARS-CoV spike protein (top-right), single chain of the SARS-CoV-2 spike protein (bottom-left), and single-chain of the SARS-CoV spike protein (bottom-right). The map shows every residue pair in the structure and the colors represent the correlation in the fluctuations of residues as shown in the spectrum. A value of 1.0 (red) means that the amino acids' movements are perfectly correlated, and a value of -1.0 (dark blue) means that their movements are perfectly anticorrelated.
{: style="font-size: medium;"}

### Slow mode shape and square fluctuations

<!-- NMA is based on the idea that the lowest frequency modes describe the largest movement in the structure. Below is the plot of the lowest frequency (slowest) mode calculated by ProDy.
-->

Above, we pointed out that in NMA, we break down the complex movements of a protein in terms of a few simpler component functions called "modes". The mode having the greatest contribution to these fluctuations (called the "slowest" mode) is charted in the figure below, called a **slow mode shape plot**, for the SARS-CoV-2 and SARS-CoV spike proteins. The amino acid positions are across the x-axis, and the direction/magnitude of movement is shown on the y-axis. Positive and negative values correspond to opposite directions of movement, and the farther a value is from zero, the more this position moves with respect to the given mode.

In this figure, we can see that the protein region between positions 200 and 500 of the spike protein is the most mobile. This region overlaps with the RBD region, found between residues 331 to 524. This analysis indicates that the RBD is a relatively mobile part of the spike protein, which matches our intuition that the RBD might need to be flexible in order to "catch" the moving target of an ACE2 enzyme and latch onto it.

![image-center](../assets/images/SlowMode.png){: .align-center}
Slow mode plots of the SARS-CoV-2 spike protein (top-left), SARS-CoV spike protein (top-right), single chain of the SARS-CoV-2 spike protein (bottom-left), and single chain of the SARS-CoV spike protein (bottom-right). The x-axis represents the amino acid positions along the protein, and the y-axis represents the relative fluctuations at each amino acid position. From the single-chain plots for both SARS-CoV-2 and SARS, we see that the residues between 200 – 500 fluctuate the most. The plots between SARS-CoV-2 and SARS-CoV are very similar, indicating similar protein fluctuations for this mode.
{: style="font-size: medium;"}

A related plot called a slow mode **square fluctuations plot** is similar to the slow mode shape plot, except that its values are produced by multiplying the square of the slow mode by the variance along the mode. In this case, all the values will be positive, and larger amplitudes represent regions of greater fluctuation. As with the slow mode plots, the square fluctuations plots for SARS-CoV-2 and SARS-CoV shown below indicate that the RBD is highly mobile compared with the rest of the spike protein.

![image-center](../assets/images/SqFlucts.png){: .align-center}
Plots of the slow mode square fluctuation of the SARS-CoV-2 spike protein (top-left), SARS-CoV spike protein (top-right), a single chain of the SARS-CoV-2 spike protein (bottom-left), and a single chain of the SARS-CoV spike protein (bottom-right). The x-axis represents the amino acid positions along the protein, and the y-axis is proportional to the square of the fluctuations at each amino acid position. The plots between SARS-CoV-2 and SARS-CoV are very similar, indicating similar protein fluctuations for this mode.
{: style="font-size: medium;"}

### Comparing Results

From all of these results, we can see that the SARS-CoV-2 and SARS-CoV spike proteins are not only very similar in terms of structure, but they are similar in terms of dynamics as well. This result is perhaps not a surprise since they both target the ACE2 enzyme, and it drives home the fact that proteins can seem almost identical and yet one can have very subtle changes that turns an outbreak into a pandemic.

## ANM Analysis of the RBD

The anisotropic counterpart to GNM, where direction does matter, is called **anisotropic network model (ANM)**. In ANM, the direction of the fluctuations are also considered. Although ANM includes directionality, ANM typically performs worse than GNM when compared with experimental data [^Yang]. Nonetheless, ANM calculations are useful because of the added directionality. In fact, we can use it to create animations depicting the range of motions and fluctuations of the protein.

In this tutorial, we will use <a href="http://prody.csb.pitt.edu/nmwiz/" target="_blank">NMWiz</a>, short for "normal mode wizard", a GUI for ProDy that is available as a plugin for VMD, to perform ANM calculations and create the animation of the SARS-CoV-2 (chimeric) RBD using the PDB entry <a href="http://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a>.

* Focus on animating the structures

[Visit tutorial](tutorial_ANM){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

From the tutorial, we were able to generate the cross-correlation map and square fluctuation of the SARS-CoV-2 RBD. The interpretation of these results are identical to the GNM analysis above. Following the same steps, we performed ANM analysis on the SARS RBD using the PDB entry SARS RBD (<a href="http://www.rcsb.org/structure/2ajf" target="_blank">2ajf</a> for comparison.

![image-center](../assets/images/ANMResults.png){: .align-center}
This figure shows the cross-correlation map (top) and the square fluctuation plot (bottom) of SARS-CoV-2 and SARS RBD using ANM. The y-axis of the square fluctuation plot represents how much the residues fluctuate in relative units. Like the results from the GNM analysis, the map and plot are very similar between the two RBDs, indicating that they are structurally similar.
{: style="font-size: medium;"}

Perhaps unsurprisingly, the maps and plots show very small differences between SARS-CoV-2 and SARS RBD, just like in the GNM calculations for the S proteins. This indicates that the two RBDs are structurally similar.

Using NMWiz and VMD, we also created animations of the protein fluctuation calculated through ANM analysis. The following animations are of the SARS-CoV-2 RBD/SARS RBD (purple) and ACE2 (green). Important residues from the three sites of conformational differences from the previous lessens are also colored.

*It is important to note that fluctuation calculated by ANM provides information on possible movement and flexibility, but does not depict actual protein movements.*

### SARS-CoV-2 Spike Chimeric RBD (6vw1)

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

### SARS Spike RBD (2ajf)

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


## Summing Up

* In this module, we discussed the importance of the 3D (tertiary) structure of the protein and the many experimental methods of determining protein structure. Unfortunately, these methods requires time to collect physical samples, to run the complicated and expensive experiment, and to computationally store it into the protein data base. We then went into the complex problem of protein structure prediction, and the two main approaches of obtaining the 3D structure from the amino acid sequence of the protein, *ab initio* and homology modeling. Perhaps due to the severity of the outbreak and global contributions, studies after studies on SARS-CoV-2 were released along with experimentally determined 3D structures of the SARS-CoV-2 S protein.

* With the 3D structures available, we used several protein analysis tools to compare the SARS-CoV-2 S protein with the SARS S protein and visualize the results. We learned about three subtle structural differences within the receptor binding domain (RBD) of the S proteins that appear to increase the binding affinity of the SARS-CoV-2 S protein and ACE2, which may be one of the reasons why SARS-CoV-2 is more infectious.

* Unfortunately, biology is extremely complex. There is so much more to the story than just the protein structure and binding affinity of the S protein. We need to consider things like what happens after the S protein binds to ACE2, how does the virus enter the cell, how does it replicate itself, how does it combat our immune systems. As a conclusion, we will explore how SARS-CoV-2 hides from our immune system.


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
