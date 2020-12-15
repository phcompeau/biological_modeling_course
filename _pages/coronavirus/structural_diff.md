---
permalink: /coronavirus/structural_diff
title: "Structural and ACE2 Interaction Differences"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---
## Visualizing the Region of Structural Differences
In the previous lesson, we identified a region between residues 476 and 485 of the SARS-CoV-2 spike protein that correspond to a structural difference between the SARS-CoV-2 and SARS-CoV RBMs. In this lesson, we will zoom in on this region of the RBM and determine whether the differences we have found affect binding affinity with the human ACE2 enzyme.

We will first use VMD to focus on the region of interest and highlight the amino acids that differ in SARS-CoV-2. If you are interested in doing so, please follow the following tutorial, which we will consult throughout the rest of this lesson.

[Visit tutorial](tutorial_visualization){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Three Sites of Conformational Differences

SARS-CoV-2 chimeric RBD complexed with ACE2 (PDB entry <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a>).


In the tutorial, we created the visualization of 6vw1 and highlighted specific amino acid residues in region we identified previously. But does this region actually affect ACE2 binding?

This region is actually one of three sites with significant conformational differences between the SARS-CoV-2 RBD and SARS RBD identified by Shang et. al. Each of the three sites are believed to contribute to SARS-CoV-2 S protein’s increased affinity with ACE2 [^Shang].

### Loop in ACE2-binding ridge

A marked difference was found on one of the loops in the ACE2-binding ridge. This is the region that we visualized in the tutorial. SARS contains a three residue motif, proline-proline-alanine, at residues 468 to 471. In contrast, SARS-CoV-2 contains a four residue motif, glycine-valine-glutamate-glycine, at the corresponding residues 482 to 485. The two bulky residues and two flexible glycines causes a conformational change in SARS-CoV-2 RBM. Below is a visual comparison of this region between SARS-CoV-2 and SARS.

**STOP:** See if you can spot the major difference between the figures below before reading what actually happens. *Hint: Look at the yellow residue!*?
{: .notice--primary}

![image-center](../assets/images/Ridge.png){: .align-center}
These are the 3D visualization of the loop in the ACE2-binding ridge that is conformationally different between SARS-CoV-2 (top figures) and SARS (bottom). The RBD is shown in purple while ACE2 is shown in green. The differences in structure cause certain amino acid residues (highlighted in various colors) to behave differently between the two interactions. In SARS-CoV-2, the differences allow RBD residue Ala475 (red) to form a hydrogen bond with ACE2 residue Ser19 (cyan). The ACE2 residues Met82, Leu79, and Tyr83 (silver) create a hydrophobic pocket. For SARS-CoV-2, the RBD residue Phe486 (yellow) inserts itself into the pocket, favorably interacting with ACE2. These interactions do not happen with SARS, and its corresponding RBD residue Leu472 (yellow), is not inserted into the pocket.
{: style="font-size: medium;"}

The most noticeable difference is between SARS-CoV-2 Phe486 and SARS Leu472. In SARS-CoV-2, Phe486 (yellow) is points towards the hydrophobic pocket (silver). Due to phenylalanine's hydrophobic properties, this is a favorable interaction that may improve SARS-CoV-2 affinity with ACE2. In contrast, Leu472 (Yellow) in SARS does not appear to approach the hydrophobic pocket. Here is a list of what the structural changes in SARS-CoV-2 result in:

* A main-chain hydrogen bond between Asn487 and Ala475 that creates a more compact ridge conformation, pushing the loop containing Ala475 to be closer to ACE2. This allows for the N-terminal residue Ser19 of ACE2 to form a hydrogen bond with the main chain of Ala475.
* Gln24 of the N-terminal helix of ACE2 forms a new contact with the RBM.
* Compared to the corresponding Leu472 of SARS, Phe486 of SARS-CoV-2 points in a different direction and is inserted into a hydrophobic pocket of Met82, Leu79, and Tyr83 in ACE2.

### Hotspot 31

Hotspot 31 is the second site of marked conformational differences between SARS-CoV-2 and SARS.

**STOP:** Again, see if you can spot the differences, it should be more obvious this time around.
{: .notice--primary}

![image-center](../assets/images/Hotspot31.png){: .align-center}
These are the 3D visualizations of hotspot 31 between SARS-CoV-2 (top) and SARS (bottom). The RBD is shown in purple and ACE2 is shown in green. In SARS, the RBD residue Tyr442 (yellow) stabilizes the salt bridge bond between ACE2 residues Lys31 and Glu35 (red). In SARS-CoV-2, the corresponding RBD residue Leu455 (yellow) is unable to support the ACE2 bond. This causes the salt bridge to break and allow the ACE residues Lys31 and Glu35 to interact and form new hydrogen bonds with RBD residue Gln493 (blue).
{: style="font-size: medium;"}

This figure shows how the interaction between Lys31 and Glu35 (Red) of ACE2 changes drastically between SARS-CoV-2 and SARS. In SARS, the two residues appear to directly point towards each other. This is because in SARS RBM, Tyr442 supports the salt bridge between Lys31 and Glu35 of ACE2. In contrast to Tyr442 in SARS, the corresponding residue in SARS-CoV-2 is the less bulky Leu455, which provides less support to Lys31 of ACE2. This causes the salt bridge to break and results in Lys31 and Glu35 of ACE2 to point almost in parallel towards RBD residue Gln493. This change allows Lys31 and Glu35 to form hydrogen bonds with Gln493 in SARS-CoV-2.

### Hotspot 353

Finally, hotspot 353 is the third site of marked confromational differences between SARS-CoV-2 and SARS. Here, the difference between the residues is amazingly subtle, so much so that it takes a keen eye to even find them. See if you can find it.


![image-center](../assets/images/Hotspot353.png){: .align-center}
These are the 3D visualizations of hotspot 353 between SARS-CoV-2 (top) and SARS (bottom). The RBD is shown in purple and ACE2 is shown in green. In SARS, the RBD residue Thr487 (yellow) stabilizes the salt bridge between ACE2 residues Lys 353 and Asp38 (red). In SARS-CoV-2, the corresponding RBD residue Asn501 (yellow) provides less support, causing ACE2 residue Lys353 (red residue on the left) to be in a slightly different conformation and form a new hydrogen bond with the RBD.
{: style="font-size: medium;"}

In SARS, the methyl group of Thr487 supports the salt bridge between Lys353 and Asp38 of ACE2, and the side-chain hydroxyl group of Thr487 forms a hydrogen bond with the RBM main chain. The corresponding SARS-CoV-2 residue Asn501 also forms a hydrogen bond between its side chain and RBM main chain. However, similar to what happened in hotspot 31, Asn501 provides less support to the salt bridge, causing Lys353 of ACE2 to be in a different conformation. Lys353 is then able to form an extra hydrogen bond with the main chain of SARS-CoV-2 RBM while maintaining the salt btidge with Asp38 of ACE2.

<hr>

We have identified three sites between SARS and SARS-CoV-2 when it comes to interacting with ACE2. Although we can see the differences visually, we still do not know how these differences actually contribute to SARS-CoV-2's increased affinity with ACE2. In the next section, we will try to quantitatively explain how these differences affect binding affinity with ACE2.

[Next lesson](NAMD){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

[^Hamming]: Hamming, I., Timens, W., Bulthuis, M., Lely, A., Navis, G., Goor, H. 2004. Tissue distribution of ACE2 portein, the functional receptor for SARS coronavirus. A first step in understanding SARS pathogenesis. J Pathol 203(2), 631-637. https://doi.org/10.1002/path.1570

[^Samavati]: Samavati, L., Uhal, B. 2020. ACE2, Much more than just a receptor for sars-cov-2. Front. Cell. Infect. Microbiol 10. https://doi.org/10.3389/fcimb.2020.00317

[^Shang]: Shang, J., Ye, G., Shi, K., Wan, Y., Luo, C., Aijara, H., Geng, Q., Auerbach, A., Li, F. 2020. Structural basis of receptor recognition by SARS-CoV-2. Nature 581, 221–224. https://doi.org/10.1038/s41586-020-2179-y
