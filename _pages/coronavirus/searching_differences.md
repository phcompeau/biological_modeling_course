---
permalink: /coronavirus/multiseq
title: "Searching for Differences"
sidebar: 
 nav: "coronavirus"
toc: true
toc_sticky: true
---

In part 1 of the module, we explored the different methods of predicting the 3D structure of a protein from its amino acid sequence, and how to assess the accuracy of our predicted structures. Because the protein's structure is critical to its function, we use protein structure prediction to analyze proteins where we have yet to determine its real structure using experimental techniques such as x-ray crystallography. Early SARS-CoV-2 researchers that wanted to study the S protein in January 2020 relied on structure prediction.

Now that research groups had enough time, the actual structure of the S protein have been determined and is available in the Protein Data Bank (PDB). With the structures, we can now producing the 3D visualizations the SARS-CoV-2 S protein and compare it with the SARS S protein to see if we can find some molecular explanation of why this virus is much more infectious than SARS.

## ACE2

In the [introduction](coronavirus_home), we discussed that SARS-CoV-2 and SARS both target the human angiotensin-converting enzyme 2 (ACE2) with their S protein. ACE2 is an enzyme that is present in most human organs and can be found on the surface of cells from various human tissues, including lung alveolar epithelial (outer layer) cells, small intestines eterocytes, arteries, and kidneys [^Hamming]. ACE2 is part of the renin-angiotensin system (RAS), which is critical in the regulation of the cardiovascular system and protective role of the lung alveolar epithelial cells [^Samavati]. 

This interaction of the S protein and ACE2 is an important step for the viral entry of both SARS-CoV-2 and SARS into the human cell. However, SARS-CoV-2 is much more infectious, and its S protein has been found to bind to ACE2 with greater affinity than that of SARS. The receptor binding domain (RBD) is the part of the S protein that interacts with ACE2, and the receptor binding motif (RBM) is the part of the RBD that mediates contact with ACE2. Therefore, we will narrow our focus to the differences in RBM to find out why SARS-CoV-2 binds better with ACE2.

* Add later: "All the analysis will be done using two software: ProDy and VMD. By the end of this module, you will be able to understand more about protein structure prediction and differences in the S proteins that attribute to the higher infectivity of COVID-19."

## Protein Structure Files
We will be using two PDB entries for comparison: <a href="https://www.rcsb.org/structure/2AJF" target="_blank">2ajf</a> and <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a>. 2ajf contains the structure of SARS RBD complexed with ACE2 and 6vw1 contains the structure of SARS-CoV-2 chimeric RBD complexed with ACE2. SARS-CoV-2 chimeric RBD consist of the SARS RBD core and SARS-CoV-2 RBM. The reason that the chimeric RBD was used is because the SARS RBD core helps facilitate crystallization by acting as the crystallization scaffold for X-ray diffraction (x-ray crystallography). Since the functional unit is still SARS-CoV-2 RBM, data from the comparisons should be similar or equivalent to using native SARS-CoV-2 RBD. Using these structures, we can produce 3D visualizations of SARS-CoV-2 RBD and SARS RBD interacting with ACE2 and see if we can determine structural differences between the interactions.

## VMD, Multiseq, and Qres
There are tools that can help us identify where the two structures deviate from each other. The brute force method is to visualize the two RBDs and to rotate them around to see if you can spot any differences. We can do this by using <a href="https://www.ks.uiuc.edu/Research/vmd/" target="_blank">Visual Molecular Dynamics (VMD)</a>, a molecular visualization program that allows users to produce interactiable 3D visualizations of molecules. However, blindly looking for structural differences can waste a lot of time and effort.

A good starting point would be to use the VMD plugin*<a href="https://www.ks.uiuc.edu/Research/vmd/plugins/multiseq/" target="_blank">Multiseq</a>*, a bioinformatics analysis environment that provides tools such as sequence and structural alignment. *Multiseq* is able to calculate structural conservation within aligned molecules by computing Q per residue. **Q-score**, or **Q**, qualitative measure of structure similarity by considering both the alignment length and RMSD. Q = 1 represents that the structures are identical, while a low Q-score (e.g. 0.1) represents low structure similarity. Q decreases with increasing RMSD or decreasing alignment length. Recognize that we can always lower RMSD by decreasing alignment length or increase alignment length with the expense of increasing RMSD. This contradictory requirement of low RMSD and high alignment length is eliminated by using the ratio of alignment length and RMSD. Below is the equation for Q [^Krissinel].

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\large&space;Q&space;=&space;\frac{N_{align}^2}{(1&plus;(RMSD/R_0)^2)*N_{res1}*N_{res2}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\large&space;Q&space;=&space;\frac{N_{align}^2}{(1&plus;(RMSD/R_0)^2)*N_{res1}*N_{res2}}" title="\large Q = \frac{N_{align}^2}{(1+(RMSD/R_0)^2)*N_{res1}*N_{res2}}" /></a>

where:  
$$0<Q\leq1$$
* $$N_{align}$$ = number of aligned residues
* $$R_0$$ = the emprical parameter, set to 3Å
* $$N_{res1}$$ = number of residues in protien 1
* $$N_{res2}$$ = number of residues in protien 2

 
**Q per residue (Qres)** is the measure of contribution of each residue to the overall Q value of the aligned structures. This is very useful for finding specific regions where the aligned proteins differ structurally from each other. To find these regions, we just need to locate regions where many residues have low Qres.

Multiseq aligns the structures by using the Structural Alignment of Multiple Proteins (STAMP) tool. The algorithm minimizes the distance between alpha carbons of the aligned residues for each protein or molecule by applying rigid-body rotations and translations. If the structures do not have common structures, then STAMP will fail. For more details on the STAMP algorithm, click <a href="http://www.compbio.dundee.ac.uk/manuals/stamp.4.4/stamp.pdf" target="_blank">here</a>.
<hr>

In this tutorial, we will use Multiseq to align the SARS-CoV-2 (chimeric) RBD and SARS RBD using the PDB entries <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a> and <a href="https://www.rcsb.org/structure/2AJF" target="_blank">2ajf</a>, respectively. Then, we will locate areas of structural differences be computing Qres.

[Visit tutorial](tutorial_multiseq){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

<hr>

In the tutorial, we calculated Qres between SARS-CoV-2 RBD and SARS RBD and identified four regions of structural differences. Because *Multseq* is a VMD plugin, we can create 3D visualizations of the structure and color them based on Qres. Below is the visualization of the imposed structures of SARS (*2ajf*) and SARS-CoV-2 (*6vw1*) RBD with ACE2 (in green). Blue represents regions of high *Qres* while red represents regions of low *Qres*. Because we want to find structural differences that cause SARS-CoV-2 RBD to bind to ACE2 with greater affinity, it is a good idea to focus on regions in or next to the binding site such as the highlighted region.


![image-center](../assets/images/QresVMD.png){: .align-center}
This is a visualization showing the superposed structures of SARS-CoV-2 chimeric RBD and SARS RBD in blue and red based on Qres. Blue indicates high Qres and red indicates low Qres. ACE2 is shown in green. The highlighted region is one of the four regions of potential structural differences. Because it is adjacent to ACE2, it is likely that the structural difference here will affect ACE2 interactions.
{: style="font-size: medium;"}

![image-center](../assets/images/QresResult.png){: .align-center}
This is a small snapshot of the sequence alignment between SARS RBD (above) and SARS-CoV-2 chimeric RBD (below) with the color blue representing high Qres and red representing low Qres. The region of low Qres corresponds to the highlighted region in the previous figure. More specifically, this region corresponds to SARS-CoV-2 residues 476 to 485.
{: style="font-size: medium;"}

From this analysis, we now identified a region that is structurally different between SARS-CoV-2 RBD and SARS RBD and is near the binding site of ACE2. In the next lesson, we will see what the specific differences are and how they affect the binding affinity.

[Next lesson](structural_diff){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

[^Hamming]: Hamming, I., Timens, W., Bulthuis, M. L., Lely, A. T., Navis, G., & van Goor, H. 2004. Tissue distribution of ACE2 protein, the functional receptor for SARS coronavirus. A first step in understanding SARS pathogenesis. The Journal of pathology, 203(2), 631–637. https://doi.org/10.1002/path.1570

[^Samavati]: Samavati, L., & Uhal, B. D. 2020. ACE2, Much More Than Just a Receptor for SARS-COV-2. Frontiers in cellular and infection microbiology, 10, 317. https://doi.org/10.3389/fcimb.2020.00317

[^Krissinel]: Krissinel E, Henrick K. 2004. Secondary-structure matching (SSM), a new tool for fast protein structure alignment in three dimensions. Acta Crystallogr D Biol Crystallogr, 60(Pt 12 Pt1), 2256-68. doi: 10.1107/S0907444904026460.
