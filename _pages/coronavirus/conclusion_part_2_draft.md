---
permalink: /coronavirus/conclusion_part_2_draft
title: "Conclusion: From Static Protein Analysis to Molecular Dynamics"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

<!--
The following parts come directly after "Modeling protein bonds using tiny springs", before the tutorial.
-->

## Molecular dynamics analysis using GNM

Performing GNM analysis on a protein gives us a fairly accurate understanding of how the protein is structured, particularly on the flexibility of the proteinand how each residue moves relative to the rest. In this section, we will revisit the human hemoglobin (<a href="https://www.rcsb.org/structure/1a3n" target="_blank">1A3N.pdb</a>) to peform GNM analysis. Recall that in GNM, the target molecule is represented using ENM. Therefore, the first step in GNM analysis is to convert hemoglobin into a system of nodes and springs. As mentioned above, this can be easily done by stripping the protein to only alpha carbons and connecting alpha carbons that are within a threshold distance. Generally, the threshold distance for GNM is set between 7 to 8 Å.

<!--
Study by Kundu et al. showing that 7.3 Å being the optimal cutoff across a set of 113 proteins.
-->

![image-center](../assets/images/hemoglobin_enm.png){: .align-center}
Conversion of human hemoglobin (left) to an elastic network model with cutoff distance of 7.3 Å (right).
{: style="font-size: medium;"}

Each node in the model is subject to **Gaussian fluctuations** that cause it to deviate in position from its equilibrium. As a direct consequence, the distance between nodes will also undergo Gaussian fluctuations. For a given node *i* and node *j*, the equilibrium position is represented by the equilibrium position vector $$ R_i^0 $$ and $$ R_j^0 $$. The fluctuation for node *i* and node *j* is represented by instantaneous fluction vectors $$ \Delta R_i $$ and $$ \Delta R_j $$. The distance between node *i* and node *j* at equilibrium is represented by the equilibrium distance vector $$ R_{ij}^0 $$, and the distance between nodes *i* and *j* in fluctuation is represented by the instantaneous distance vector $$ R_{ij} $$. Finally, we can calculate the fluctionation in the distance, $$ \Delta R_{ij} = R_{ij} - R_{ij}^0 = \Delta R_j - \Delta R_i $$.

![image-center](../assets/images/gaussian_fluctuations.png){: .align-center}
Schematic showing gaussian fluctuations between two nodes. Equilibrium positions of node *i* and node *j* are represented by distance vectors $$ R_i^0 $$ and $$ R_j^0 $$. The equilibrium distance between the nodes is labelled $$ R_{ij}^0 $$. The instantaneous fluction vectors, are labelled $$ \Delta R_i $$ and $$ \Delta R_j $$ and the instantaneous distance vector is labeled $$ \Delta R_{ij} $$. Image courtesy of Ahmet Bakan.
{: style="font-size: medium;"}

The next step is to construct a **Kirchhoff matrix**, or connectivity matrix, represented by the symbol $$ \Gamma $$ . The Kirchhoff matrix is the matrix representation of which pairs of residues are connected such that we can use it in later calculations. The matrix is constructed as follows:

$$ \Gamma_{ij} = \begin{cases} & -1 \text{ if $i \neq j$ and $R_{ij} \leq r_c$}\\ &  0 \text{ if $i \neq j$ and $R_{ij} > r_c$} \end{cases} $$

$$ \Gamma_{ii} = -\sum_j \Gamma_{ij} $$

where $$r_c$$ is the threshold distance. Simply put, if residue i and residue j are connected, then the value of position i,j in the matrix will be -1. If they are not connected, the the value will be 0. The values of the diagonals, i.e. position i,i, correspond to the total number of connections of residue i. 

![image-center](../assets/images/kirchhoff_example.png){: .align-center}
Toy structure and the corresponding Kirchhoff matrix.
{: style="font-size: medium;"}

One of the most common analysis using GNM is on the coordinated movement between residues as the protein fluctuates. More specifically, we want to see how each residue will move relative to other residues, or the **cross-correlation** between the residues. The cross-correlation between some residue *i* and residue *j* can be mathmatically calculated using the Kirchhoff matrix from before:

$$ \langle \Delta R_i \cdot \Delta R_j \rangle = \frac{3 k_B T}{\gamma} \left[ \Gamma^{-1} \right]_{ij} $$

where $$ k_B $$ is the Boltzmann constant and $$ \gamma $$ is the spring constant (stiffness of the spring). Similarly, we can also calculate the expectation values of the fluctuation for residues, or the **mean-square fluctuations**, using the Kirchhoff matrix:

$$  \langle \Delta R_i^2  \rangle = \frac{3 k_B T}{\gamma} \left[ \Gamma^{-1} \right]_{ii} $$

From these equations, we can see that the Kirchhoff matrix fully defines both the cross-correlations between residue motions as well as the mean-square fluctions of the residues.

$$ \left[ \Gamma^{-1} \right]_{ij} \sim \langle \Delta R_i \cdot \Delta R_j \rangle $$

$$ \left[ \Gamma^{-1} \right]_{ii} \sim \langle \Delta R_i^2  \rangle $$

However, because the determinant of the Kirchhoff matrix is 0 in GNM, we cannot directly invert it to get $$ \Gamma^{-1} $$. Instead, we use eigen decomposition on the matrix.

$$ \Gamma = U \Lambda U^T $$

where $$ U $$ is the orthogonal matrix with the $$ k^{th} $$ column, represented by $$ u_k $$, corresponding to the $$ k^{th} $$ eigenvector of $$ \Gamma $$, and $$ \Lambda $$ is the diagonal matrix of eigenvalues, represented by $$ \lambda_k $$. Based on the characteristics of the Kirchhoff matrix (positive semi-definite), the first eigenvalue, $$ \lambda_1 $$, is 0. The remaining $$ N-1 $$ eigenvalues, as well as the eigenvectors in $$ U $$, actually directly describe the modes of motion that discussed earlier in this lesson. The elements of eigenvector $$ u_k $$ describe the distribution of residue displacements, normalized over all the residues, along the $$ k^{th} $$ mode axis. In other words, the motion of the $$ i^{th} $$ residue along the $$ k^{th} $$ mode is described by the $$ i^{th} $$ element in eigenvector $$ u_k $$. The corresponding eigenvalue $$ \lambda_k $$ describes the frequency of the $$ k^{th} $$ mode, where the smallest $$ \lambda $$ value corresponds to the lowest frequency modes, or slowest modes, that make the largest contribution to the overall protein motion.

After eigen decomposition, we can now rewrite the cross-correlation equation as a sum of the N-1 GNM modes:

$$ \langle \Delta R_i \cdot \Delta R_j \rangle = \frac{3 k_B T}{\gamma} \sum_{k=1}^{N-1} \left[ \lambda_k^{-1} u_k u_k^T \right]_{ij} $$

and similarly for mean-square fluctuation:

$$ \langle \Delta R_i^2  \rangle = \frac{3 k_B T}{\gamma} \sum_{k=1}^{N-1} \left[ \lambda_k^{-1} u_k u_k^T \right]_{ii} $$

Now that we can compute the cross-correlation between residues, we can normalize the values and construct a normalized cross-correlation matrix, $$ C^{(n)} $$, such that:

$$ C^{(n)}_{ij} = \frac{\langle \Delta R_i \cdot \Delta R_j \rangle}{\left[ \langle \Delta R_i \cdot \Delta R_i \rangle \langle \Delta R_j \cdot \Delta R_j \rangle \right]^{\frac{1}{2}} $$

where $$ C^{(n)}_{ij} $$ corresponds to the orientational cross-correlation between residue *i* and residue *j*. Because we normalized the values, the range of $$ C^{(n)}_{ij} $$ is $$ [-1,1] $$, where 1 means the residues are fully correlated in motion, and -1 means the residues are fully anti-correlated in motion. Finally, we can visualize the matrix as a **cross-correlation heat map** like the figure below.

![image-center](../assets/images/hemoglobin_cc.png){: .align-center}
Normalized cross-correlation heat map of human hemoglobin of the first 20 slowest normal modes. Red regions indicate correlated residue pairs which move in the same direction; blue regions indicate anti-correlated residue pairs which move in opposite directions. 
{: style="font-size: medium;"}

TO DO: 
* B-factors & mean-square flucts plot
* Individual mode shapes

