---
permalink: /coronavirus/multiseq
title: "Searching for Local Differences in Two Similar Protein Structures"
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

 * We could do this just with d(s_i, s_j)^2

A good starting point would be to use the VMD plugin*<a href="https://www.ks.uiuc.edu/Research/vmd/plugins/multiseq/" target="_blank">Multiseq</a>*, a bioinformatics analysis environment that provides tools such as sequence and structural alignment. *Multiseq* is able to calculate structural conservation within aligned molecules by computing **Q per residue (Qres)**. After aligning, for example, protein A and protein B, Qres describes the similarity of a particular residue's structural environment in protein A compared to the aligned residue's structural environment in protein B. This is done by comparing the alpha carbon distances between the residue and all other aligned residues, excluding nearest neighbors, of the aligned proteins. The formal definition of Qres is as follows[^Qres]:

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;Q_{res}^{(i,n)}&space;=&space;N&space;\sum^{proteins}_{m\neq&space;n}&space;\sum^{residues}_{j\neq&space;i-1,i,i&plus;1}&space;exp[-\frac{(r_{ij}^{(n)}-r^{(m)}_{i'j'})^2}{2\sigma^2_{ij}}]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;Q_{res}^{(i,n)}&space;=&space;N&space;\sum^{proteins}_{m\neq&space;n}&space;\sum^{residues}_{j\neq&space;i-1,i,i&plus;1}&space;exp[-\frac{(r_{ij}^{(n)}-r^{(m)}_{i'j'})^2}{2\sigma^2_{ij}}]" title="Q_{res}^{(i,n)} = N \sum^{proteins}_{m\neq n} \sum^{residues}_{j\neq i-1,i,i+1} exp[-\frac{(r_{ij}^{(n)}-r^{(m)}_{i'j'})^2}{2\sigma^2_{ij}}]" /></a>

where: 
* $$Q_{res}^{(i,n)}$$ is the structural similarity of the $$i^{th}$$ residue in the $$n^{th}$$ protein
* $$r_{ij}^{(n)}$$ is the distance between the alpha carbons of residues $$i$$ and $$j$$ in protein $$n$$
* $$r_{i'j'}^{(m)}$$ is the distance between alpha carbons of the residues in protein $$m$$ that correspond to residues $$i$$ and $$j$$ in protein $$n$$
* Variance $$sigma_{ij}^2 = |i-j|^{0.15}$$, which corresponds to the sequence separation between residues $$i$$ and $$j$$
* Normalization $$N = \frac{1}{(N_{seq}-1)(N_{res}-k}$$, where $$N_{seq}$$ is the number of proteins, $$N_{res}$$ is the number of residues in protein $$n$$, and $$k=2$$ when residue $$i$$ is the N- or C-terminus and $$k=3$$ otherwise.

The formal definition may be quite complicated, but thankfully the result is much easier to interpret. The calculation for Qres will result in a value between 0 and 1, with lower scores representing low similarity and higher scores representing high similarity.

Multiseq aligns the structures by using the Structural Alignment of Multiple Proteins (STAMP) tool. The algorithm minimizes the distance between alpha carbons of the aligned residues for each protein or molecule by applying rigid-body rotations and translations. If the structures do not have common structures, then STAMP will fail. For more details on the STAMP algorithm, click <a href="http://www.compbio.dundee.ac.uk/manuals/stamp.4.4/stamp.pdf" target="_blank">here</a>.
<hr>

In this tutorial, we will use Multiseq to align the SARS-CoV-2 (chimeric) RBD and SARS RBD using the PDB entries <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a> and <a href="https://www.rcsb.org/structure/2AJF" target="_blank">2ajf</a>, respectively. Then, we will locate areas of structural differences be computing Qres.

[Visit tutorial](tutorial_multiseq){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Local comparison of spike proteins leads us to a region of interest

In the tutorial, we identified a 13-column region of the sequence alignment of the SARS-CoV and SARS-CoV-2 RBMs for which the Qres values are significantly lower than they are elsewhere in the RBD. This region corresponds to positions 476 to 485 in the SARS-CoV-2 spike protein and is shown in the figure below.

![image-center](../assets/images/QresResult.png){: .align-center}

![image-center](../assets/images/QresResult_cropped.png){: .align-center}
(Top) A snapshot of the sequence alignment between the SARS-CoV RBD (above) and the SARS-CoV-2 chimeric RBD (below). Columns are colored along a spectrum from blue (high Qres) to red (low Qres), with positions that correspond to an inserted or deleted amino acid colored red. (Bottom) Zooming in on a region of the alignment with low Qres, which corresponds to amino acids at positions 476 to 485 in SARS-CoV-2.
{: style="font-size: medium;"}

Because Multiseq is a VMD plugin, we can create 3-D visualizations of the structures and color them based on Qres. The figure below shows the superimposed structures of both the SARS and SARS-CoV-2 RBD bound with ACE2, shown in green. The same color-coding of columns of the multiple alignment in the figure above is used to highlight differences between the SARS-CoV and SARS-CoV-2 structures; that is, blue represents regions of high *Qres*, and red represents regions of low *Qres*. The region of the RBM alignment in the above figure with low Qres is outlined in the below figure.

![image-center](../assets/images/QresVMD.png){: .align-center}
This is a visualization showing the superposed structures of SARS-CoV-2 chimeric RBD and SARS RBD in blue and red based on Qres. Blue indicates high Qres and red indicates low Qres. ACE2 is shown in green. The highlighted region is one of the four regions of potential structural differences. Because it is adjacent to ACE2, it is likely that the structural difference here will affect ACE2 interactions.
{: style="font-size: medium;"}

Finding this highlighted region in the RBM where the structures of the SARS-CoV and SARS-CoV-2 spike proteins differ is an exciting development. In the next lesson, we would like to focus solely on this tiny region and see how the handful of mutations influence the binding affinity of the spike protein with the ACE2 enzyme.

[Next lesson](structural_diff){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

[^Hamming]: Hamming, I., Timens, W., Bulthuis, M. L., Lely, A. T., Navis, G., & van Goor, H. 2004. Tissue distribution of ACE2 protein, the functional receptor for SARS coronavirus. A first step in understanding SARS pathogenesis. The Journal of pathology, 203(2), 631–637. https://doi.org/10.1002/path.1570

[^Samavati]: Samavati, L., & Uhal, B. D. 2020. ACE2, Much More Than Just a Receptor for SARS-COV-2. Frontiers in cellular and infection microbiology, 10, 317. https://doi.org/10.3389/fcimb.2020.00317

[^Qres]: https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.367.6652&rep=rep1&type=pdf

[^Jaimes]: Jaimes, J. A., André, N. M., Chappie, J. S., Millet, J. K., & Whittaker, G. R. 2020. Phylogenetic Analysis and Structural Modeling of SARS-CoV-2 Spike Protein Reveals an Evolutionary Distinct and Proteolytically Sensitive Activation Loop. Journal of molecular biology, 432(10), 3309–3325. https://doi.org/10.1016/j.jmb.2020.04.009

## Extra

* Cite the RBM alignment as taken from this article: https://jvi.asm.org/content/jvi/94/7/e00127-20.full.pdf

* Is there anything that we can say about the five blue residues in the multiple alignment figure? Perhaps we can just make our own alignment here since it's public data.

* Fix Q-res discussion

* Good exercise later: compute Q scores for the protein structure comparison that we performed at the end of part 1.
