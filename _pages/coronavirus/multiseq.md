---
permalink: /coronavirus/multiseq
title: "Searching for Differences"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

In part 1 of this module, we used a variety of existing software resources to predict the structure of the SARS-CoV-2 spike protein from its amino acid sequence. We then discussed how to compare our predicted structures against the experimentally confirmed structure of the protein.

Now begins part 2 of the module, in which we assume that we have this validated structure of the spike protein and ask a very simple question: how does it compare against the SARS-CoV spike protein from the 2003 outbreak? Can we find any clues lurking in the structures of the spike protein that would indicate why the two viruses behave differently in humans? In particular, why did SARS-CoV fizzle out while SARS-CoV-2 was infectious enough to cause a pandemic?

## Focusing on a variable region of interest in the spike protein

We already know from our work in part 1 of this module that when we compare the SARS-CoV and SARS-CoV-2 genomes, the Spike protein is much more variable than other regions. We even see variable and conserved regions within the spike protein, as the following figure (reproduced from the section on [homology modeling](homology)) indicates.

![image-center](../assets/images/spike_protein_similarity.png){: .align-center}
Variable and conserved regions in the SARS-CoV and SARS-CoV-2 spike proteins. The S1 domain tends to be more variable, while the S2 domain is more conserved (and even has a small region of 100% similarity). Source: Jaimes et al. 2020[^Jaimes].
{: style="font-size: medium;"}

The most variable region between the two viruses in the spike protein is the **receptor binding motif (RBM)**, part of the receptor binding domain (RBD) whose structure we predicted using GalaxyWEB in the [homology modeling] tutorial(tutorial_homology). The RBM is the component of the RBD that mediates contact with ACE2, as the following simplified video illustrates.

<center>
<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/e2Qi-hAXdJo" frameborder="0" allowfullscreen></iframe>
</center>

Given that the RBM is so critical to the virus's ability to bond to the target human enzyme, the fact that it has mutated so much from SARS-CoV to SARS-CoV-2 makes it a fascinating area of focus for our study. Do the mutations that SARS-CoV-2 has accumulated make it easier for the virus to bond to human cells? Could this be why SARS-CoV-2 is more infectious than SARS-CoV?

As we hone in on the RBM, we provide an alignment of the 70 amino acid long RBM region from SARS-CoV and SARS-CoV-2 (as well as two animal viruses) in the figure below.

![image-center](../assets/images/RBM_alignment.png){: .align-center}
A multiple alignment of the RBM (colored amino acids) across the human SARS-CoV virus (first row), a version of the virus isolated in a palm civet (second row), a virus isolated in a bat in 2013 (third row), and the SARS-CoV-2 virus (fourth row). Beneath each column, an asterisk denotes full conservation, a period denotes a slight mutation, and a colon indicates high variability.
{: style="font-size: medium;"}

All this having been said, we know from part 1 of this module that just because the sequence of a protein has been greatly mutated does not mean that the structure of that protein has changed much. Therefore, in this lesson, we will start a comparative analysis of the SARS-CoV and SARS-CoV-2 spike proteins at the structural level. All of the analysis will be performed using the software resources ProDy and VMD, which we briefly introduced in part 1. By the end of this module, our goal is to understand whether these mutations in the RBM really have contributed to higher infectiousness.

## Identifying local dissimilarities between protein structures

Not only did researchers experimentally verify the structure of the spike protein of the two viruses, they were able to determine the structure of the RBD complexed with ACE2 in both SARS-CoV (PDB entry: <a href="https://www.rcsb.org/structure/2AJF" target="_blank">2ajf</a>) and SARS-CoV-2 (PDB entry: <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a>). To be more precise, the SARS-CoV-2 structure is actually a *chimeric* protein formed of the SARS-CoV RBD in which the RBM has the sequence from SARS-CoV-2. A chimeric RBD was used for complex technical reasons to ensure that the crystallization process during X-ray crystallography could be borrowed from that used for SARS-CoV.

Because we have these known structures of the bound complexes, we can produce 3-D visualizations of the two different complexes and see if we can find structural differences involving the RBM. In the tutorial linked from this lesson, we will use VMD to produce this visualization, rotating the structures around to examine potential differences. However, we should be wary of simply trusting our eyes to guide us; can we use a computational approach to tell us where to look?

In the previous lesson on assessing the accuracy of a predicted structure, we introduced a metric called root mean square deviation (RMSD) for quantifying the difference between two protein structures. RMSD offered an excellent method for a *global* comparison (i.e., a comparison across all structures), but we are interested in the *local* regions where the SARS-CoV and SARS-CoV-2 complexes differ. To this end, we will need an approach that examines individual amino acids in similar protein structures to find where they differ.

**Transition to Q Score**

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

[^Jaimes]: Jaimes, J. A., André, N. M., Chappie, J. S., Millet, J. K., & Whittaker, G. R. 2020. Phylogenetic Analysis and Structural Modeling of SARS-CoV-2 Spike Protein Reveals an Evolutionary Distinct and Proteolytically Sensitive Activation Loop. Journal of molecular biology, 432(10), 3309–3325. https://doi.org/10.1016/j.jmb.2020.04.009

## Extra

* Cite the RBM alignment as taken from this article: https://jvi.asm.org/content/jvi/94/7/e00127-20.full.pdf

* Is there anything that we can say about the five blue residues in the multiple alignment figure? Perhaps we can just make our own alignment here since it's public data.
