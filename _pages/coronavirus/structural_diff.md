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

Our region of interest in the RBM is one of three sites with significant conformational differences between the SARS-CoV-2 and SARS-CoV spike proteins that were identified by Shang et al.[^Shang]. We will now discuss each of these three locations and see how they affect binding affinity between the spike protein and ACE2.

<!--

SARS-CoV-2 chimeric RBD complexed with ACE2 (PDB entry <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a>).

-->

### Loop in ACE2-binding ridge

The first location is found within the RBM region of interest from the previous lesson and is found on a loop in the ACE2-binding ridge.

The structural differences are challenging to visualize with a 2-D image, but if you followed the preceding tutorial, then we encourage you to view the 3-D representation of the protein using VMD. Instructions on how to rotate a molecule and zoom in and out within VMD were given in our [tutorial on finding local protein differences](tutorial_multiseq).

**STOP:** See if you can identify the major structural difference between the proteins in the figure below. Hint: look at the yellow residue.
{: .notice--primary}

![image-center](../assets/images/Ridge.png){: .align-center}
A visualization of the loop in the ACE2-binding ridge that is conformationally different between SARS-CoV-2 (top) and SARS-CoV (bottom). The coronavirus RBD is shown in purple, and ACE2 is shown in green. The differences in structure cause certain amino acid residues (highlighted in various colors) to behave differently between the two interactions, which are discussed in the main text.
{: style="font-size: medium;"}

The most noticeable difference between SARS-CoV-2 and SARS-CoV relates to a "hydrophobic pocket", or three hydrophobic ACE2 residues at positions 82, 79, and 83 (methionine, leucine, and tyrosine). This pocket, which is colored silver in the above figure, is hidden away from the outside of the protein to keep these amino acids separate from water. In SARS-CoV-2, the RBD phenylalanine residue at position 486 (yellow) inserts itself into the pocket, favorably interacting with ACE2. These interactions do not happen with SARS-CoV, and its corresponding RBD residue, a leucine at position 472 (yellow), is not inserted into the pocket.

In what follows, we use a three-letter identifier for an amino acid followed by a number to indicate the identity of that amino acid followed by its position within the protein sequence. For example, the phenylalanine at position 486 of the SARS-CoV-2 spike protein would be called Phe486.

Although the interaction with the hydrophobic pocket is the most critical difference between SARS-CoV-2 and SARS-CoV, there are two other key differences that we would highlight.

* In SARS-CoV-2, a main-chain hydrogen bond forms between Asn487 and Ala475 (shown in red in the above figure), which creates a more compact ridge conformation, pushing the loop containing Ala475 closer to ACE2. This repositioning allows for the N-terminal residue Ser19 of ACE2 (shown in cyan in the above figure) to form a hydrogen bond with the main chain of Ala475.

* Gln24 of the N-terminal helix of ACE2 forms a new contact with the RBM.

### Hotspot 31

Hotspot 31 is the second site of marked conformational differences between SARS-CoV-2 and SARS-CoV.

**STOP:** Again, see if you can spot the differences between SARS-CoV-2 and SARS-CoV.
{: .notice--primary}

![image-center](../assets/images/Hotspot31.png){: .align-center}
These are the 3D visualizations of hotspot 31 between SARS-CoV-2 (top) and SARS-CoV (bottom). The RBD is shown in purple and ACE2 is shown in green. In SARS-CoV, the RBD residue Tyr442 (yellow) stabilizes the salt bridge bond between ACE2 residues Lys31 and Glu35 (red). In SARS-CoV-2, the corresponding RBD residue Leu455 (yellow) is unable to support the ACE2 bond. This causes the salt bridge to break and allow the ACE residues Lys31 and Glu35 to interact and form new hydrogen bonds with RBD residue Gln493 (blue).
{: style="font-size: medium;"}

This figure shows how the interaction between Lys31 and Glu35 (Red) of ACE2 changes drastically between SARS-CoV-2 and SARS-CoV. In SARS-CoV, the two residues appear to directly point towards each other. This is because in SARS-CoV RBM, Tyr442 supports the salt bridge between Lys31 and Glu35 of ACE2. In contrast to Tyr442 in SARS-CoV, the corresponding residue in SARS-CoV-2 is the less bulky Leu455, which provides less support to Lys31 of ACE2. This causes the salt bridge to break and results in Lys31 and Glu35 of ACE2 to point almost in parallel towards RBD residue Gln493. This change allows Lys31 and Glu35 to form hydrogen bonds with Gln493 in SARS-CoV-2.

### Hotspot 353

Finally, hotspot 353 is the third site of marked conformational differences between SARS-CoV-2 and SARS-CoV. Here, the difference between the residues is amazingly subtle, so much so that it takes a keen eye to even find them.

![image-center](../assets/images/Hotspot353.png){: .align-center}
These are the 3D visualizations of hotspot 353 between SARS-CoV-2 (top) and SARS-CoV (bottom). The RBD is shown in purple and ACE2 is shown in green. In SARS-CoV, the RBD residue Thr487 (yellow) stabilizes the salt bridge between ACE2 residues Lys 353 and Asp38 (red). In SARS-CoV-2, the corresponding RBD residue Asn501 (yellow) provides less support, causing ACE2 residue Lys353 (red residue on the left) to be in a slightly different conformation and form a new hydrogen bond with the RBD.
{: style="font-size: medium;"}

In SARS-CoV, the methyl group of Thr487 supports the salt bridge between Lys353 and Asp38 of ACE2, and the side-chain hydroxyl group of Thr487 forms a hydrogen bond with the RBM main chain. The corresponding SARS-CoV-2 residue Asn501 also forms a hydrogen bond between its side chain and RBM main chain. However, similar to what happened in hotspot 31, Asn501 provides less support to the salt bridge, causing Lys353 of ACE2 to be in a different conformation. Lys353 is then able to form an extra hydrogen bond with the main chain of SARS-CoV-2 RBM while maintaining the salt btidge with Asp38 of ACE2.

We have identified three sites between SARS-CoV and SARS-CoV-2 when it comes to interacting with ACE2. Although we can see the differences visually, we still do not know how these differences actually contribute to SARS-CoV-2's increased affinity with ACE2. In the next section, we will try to quantitatively explain how these differences affect binding affinity with ACE2.

[Next lesson](NAMD){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Extra

* Something about we know from the previous lesson how a tiny change in bonding parameters can produce a big difference. These are subtle but may be the difference in the ability of the virus to "stick" to ACE2 and spread vs. fizzling.

[^Hamming]: Hamming, I., Timens, W., Bulthuis, M., Lely, A., Navis, G., Goor, H. 2004. Tissue distribution of ACE2 portein, the functional receptor for SARS coronavirus. A first step in understanding SARS pathogenesis. J Pathol 203(2), 631-637. https://doi.org/10.1002/path.1570

[^Samavati]: Samavati, L., Uhal, B. 2020. ACE2, Much more than just a receptor for sars-cov-2. Front. Cell. Infect. Microbiol 10. https://doi.org/10.3389/fcimb.2020.00317

[^Shang]: Shang, J., Ye, G., Shi, K., Wan, Y., Luo, C., Aijara, H., Geng, Q., Auerbach, A., Li, F. 2020. Structural basis of receptor recognition by SARS-CoV-2. Nature 581, 221–224. https://doi.org/10.1038/s41586-020-2179-y
