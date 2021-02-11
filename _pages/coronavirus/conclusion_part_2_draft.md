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

## Introduction to GNM

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

The next step is to construct a **Kirchhoff matrix**, also known as the **Laplacian matrix** or **connectivity matrix**, represented by the symbol $$ \Gamma $$. Commonly used in graph theory, the Kirchhoff matrix is essentially a square matrix representation of a graph. By transforming the protein into a set of connected nodes, we are converting the protein into a graph. Therefore, the Kirchhoff matrix can be used to represent the protein, allowing us to go from a biochemistry problem to a linear algebra problem. In this case, the Kirchhoff matrix is the matrix representation of which pairs of residues are connected. There are also some useful properties of the Kirchhoff matrix that we will take advantage of later on. The matrix is constructed as follows:

$$ \Gamma_{ij} = \begin{cases} & -1 \text{ if $i \neq j$ and $R_{ij} \leq r_c$}\\ &  0 \text{ if $i \neq j$ and $R_{ij} > r_c$} \end{cases} $$

$$ \Gamma_{ii} = -\sum_j \Gamma_{ij} $$

where $$r_c$$ is the threshold distance. Simply put, if residue i and residue j are connected, then the value of position i,j in the matrix will be -1. If they are not connected, the the value will be 0. The values of the diagonals, i.e. position i,i, correspond to the total number of connections of residue i. 

![image-center](../assets/images/kirchhoff_example.png){: .align-center}
Toy structure and the corresponding Kirchhoff matrix.
{: style="font-size: medium;"}

One of the most common analysis using GNM is on the coordinated movement between residues as the protein fluctuates. More specifically, we want to see how each residue will move relative to other residues, or the **cross-correlation** between the residues. Recall that we are representing the fluctuations as vectors (see Gaussian Fluctuations). Therefore, for some residue *i* and residue *j*, we are trying to compute how much of the fluctation vector $$ \Delta R_i $$ points in the the same direction as the fluctuation vector $$ \Delta R_j $$. To do this, we need to compute the **inner product** of the vectors, denoted by the angle brackets: $$ \langle \rangle $$, which is a generalization of the dot product. In other words, computing the inner product between the fluctuation vectors is synonomous to computing the cross-correlation between the residues. As such, the cross-correlation between residue *i* and residue *j* is often represented as $$ \langle \Delta R_i \cdot \Delta R_j \rangle $$. It turns out that the inner product is correlated to the inverse of the Kirchhoff matrix, allowing us to simply invert the Kirchhoff matrix. The cross-correlation between some residue *i* and residue *j* can be mathmatically calculated as follows:

$$ \langle \Delta R_i \cdot \Delta R_j \rangle = \frac{3 k_B T}{\gamma} \left[ \Gamma^{-1} \right]_{ij} $$

where $$ k_B $$ is the Boltzmann constant, $$ \gamma $$ is the spring constant (stiffness of the spring), and $$ \left[ \Gamma^{-1} \right]_{ij} $$ is element *ij* in the inverted Kirchhoff matrix. Similarly, we can also calculate the expectation values of the fluctuation for each residue, or the **mean-square fluctuations**, which is the inner product of the fluctuation vector with itself:

$$  \langle \Delta R_i^2  \rangle = \frac{3 k_B T}{\gamma} \left[ \Gamma^{-1} \right]_{ii} $$

From these equations, we can see that the inverse Kirchhoff matrix fully defines both the cross-correlations between residue motions as well as the mean-square fluctions of the residues.

$$ \left[ \Gamma^{-1} \right]_{ij} \sim \langle \Delta R_i \cdot \Delta R_j \rangle $$

$$ \left[ \Gamma^{-1} \right]_{ii} \sim \langle \Delta R_i^2  \rangle $$

However, we run into problems here because cannot simply invert the Kirchhoff matrix. In linear algebra, a matrix is invertible if and only if its determinant is zero. Unfortunately for us, one of the special properties of the Kirchhoff matrix in GNM is that the determinant is zero, and we cannot directly invert the matrix to get $$ \Gamma^{-1} $$. Thankfully, there is a method to compute the values of the inverted matrix by performing eigen decomposition on the matrix.

$$ \Gamma = U \Lambda U^T $$

where $$ U $$ is the orthogonal matrix with the $$ k^{th} $$ column, represented by $$ u_k $$, corresponding to the $$ k^{th} $$ eigenvector of $$ \Gamma $$, and $$ \Lambda $$ is the diagonal matrix of eigenvalues, represented by $$ \lambda_k $$. Based on the characteristics of the Kirchhoff matrix (positive semi-definite), the first eigenvalue, $$ \lambda_1 $$, is 0. The remaining $$ N-1 $$ eigenvalues, as well as the eigenvectors in $$ U $$, actually directly describe the modes of motion that discussed earlier in this lesson. The elements of eigenvector $$ u_k $$ describe the distribution of residue displacements, normalized over all the residues, along the $$ k^{th} $$ mode axis. In other words, the motion of the $$ i^{th} $$ residue along the $$ k^{th} $$ mode is described by the $$ i^{th} $$ element in eigenvector $$ u_k $$. The corresponding eigenvalue $$ \lambda_k $$ describes the frequency of the $$ k^{th} $$ mode, where the smallest $$ \lambda $$ value corresponds to the lowest frequency modes, or slowest modes, that make the largest contribution to the overall protein motion.

After eigen decomposition, we can now rewrite the cross-correlation equation as a sum of the N-1 GNM modes:

$$ \langle \Delta R_i \cdot \Delta R_j \rangle = \frac{3 k_B T}{\gamma} \sum_{k=1}^{N-1} \left[ \lambda_k^{-1} u_k u_k^T \right]_{ij} $$

and similarly for mean-square fluctuation:

$$ \langle \Delta R_i^2  \rangle = \frac{3 k_B T}{\gamma} \sum_{k=1}^{N-1} \left[ \lambda_k^{-1} u_k u_k^T \right]_{ii} $$

Now that we can compute the cross-correlation between residues, we can normalize the values and construct a normalized cross-correlation matrix, $$ C^{(n)} $$, such that:

$$ C^{(n)}_{ij} = \frac{\langle \Delta R_i \cdot \Delta R_j \rangle}{\left[ \langle \Delta R_i \cdot \Delta R_i \rangle \langle \Delta R_j \cdot \Delta R_j \rangle \right]^{\frac{1}{2}}} $$

where $$ C^{(n)}_{ij} $$ corresponds to the orientational cross-correlation between residue *i* and residue *j*. Because we normalized the values, the range of $$ C^{(n)}_{ij} $$ is $$ [-1,1] $$, where 1 means the residues are fully correlated in motion, and -1 means the residues are fully anti-correlated in motion.

### Cross-Correlation Map

Cross-correlation analysis provides useful insight on the structure of the protein. The regions of high correlation coming off the diagonal typically provide information on secondary structures (residues in the same secondary structure will typically move together). On the other hand, high correlation regions not near the diagonal provide information on the tertiary structure of the protein, such as protein domains and clues to which parts of the protein work together. In general, we can observe complex patterns of correlated and anti-correlated movement throughout the protein (both inter- and intrasubunit), which can act like some sort of fingerprint. We can compare the cross-correlation between regions of the same protein or the cross correlation map between two similar proteins to find differences in the correlation patterns. This would then provide clues in where the proteins or protein regions are different structurally and possibly functionally. After calculating the cross-correlation for each residue pair, we can organize the data as a matrix and then visualize it as a **cross-correlation heat map** like the figure below.

![image-center](../assets/images/hemoglobin_cc.png){: .align-center}
Normalized cross-correlation heat map of human hemoglobin (1A3N) using the first 20 slowest normal modes. Red regions indicate correlated residue pairs which move in the same direction; blue regions indicate anti-correlated residue pairs which move in opposite directions. 
{: style="font-size: medium;"}

In the cross-correlation map of human hemoglobin above, we see four squares of positive correlation along the diagonal. This represents the four subunits of hemoglobin, $$ \alpha_1 $$, $$ \beta_1 $$, $$ \alpha_2 $$, and $$ \beta_2 $$ in this order and the intrasubunit correlations. We can differentiate between the two types of subunits by comparing the correlation patterns between the four squares. We see that the same patterns can be seen between the first and third square, and the second and fourth square. Assuming that first square represents $$ \alpha_1 $$, we can deduce that the third square represents $$ \alpha_2 $$, and that the second and fourth square represent $$ \beta $$ subunits. 

The rest of the cross-correlation map (regions next to the diagonal squares) provide evidence of high intersubunit correlations between $$ \alpha_1 $$ and $$ \beta_1 $$, $$ \alpha_2 $$ and $$ \beta_2 $$, some correlation between the $$ \alpha_1 $$ and $$ \beta_2 $$ and subunits $$ \alpha_2 $$ and $$ \beta_1 $$, and minimal correlation between the $$ \alpha_1 $$ and $$ \alpha_2 $$, and $$ \beta_1 $$ and $$ \beta_2 $$. This agrees with experimental analysis of human hemoglobin on the interaction of the extensive, cooperative interactions between $$ alpha $$ and $$ \beta $$ subunits, and minimal interactions between $$ \alpha $$ subunits and between $$ \beta $$ subunits [^Garrett].

### Mean-square Fluctuations & B-factor

Just like cross-correlation, we can also visualize the mean-square fluctuations of the residues. This is typically done in two ways. The simplest is to directly plot the values, where the x-axis represent the residues and the y-axis represent the mean-square fluctuation $$ \langle \Delta R_i^2  \rangle $$. The other, more useful, method is to plot the B-factor. When performing crystallography, the displacement of atoms within the protein crystal decreases the intesity of the scattered X-ray, creating uncertainty in the positions of atoms. **B-factor**, also known as **temperature factor** or **Debye-Waller factor** is a measure of this uncertainty, which includes noise from positional variance of thermal protein motion, model errors, and lattice defects. B-factors are reported in addition to the atomic coordinates in the PDB entry. One of the main reason we use B-factors is that they scale with the mean-square fluctuation, such that for atom *i*:

$$ B_i = \frac{8 \pi^2}{3} \langle \Delta R_i^2 \rangle $$

We can calculate the **theoretical B-factors** using the equation and GNM analysis, and the correlation with the **experimental B-factors** that are included in the PDB entry as a simple way to evaluate the GNM analysis. A study in 2009 by Lei Yang et al. compared the experimental and theoretical B-factors of 190 sufficiently different (<50% similarity) protein stuctures from X-ray and found the correlation to be about 0.58 on average [^Yang2]. Below is a plot of the B-factor, synonomous to the mean-square fluctutation, of $$ \alpha_1 $$. Residues with high values are those that fluctuate with greater motion or residues with greater positional uncertainty, and are colored red in the figures. In this case, we see that the residues colored in red are generally at the ends of secondary structures in the outer edges of the protein and loops (segments in between secondary structures). This is expected because protein loops typically contain highly fluctuating residues.

![image-center](../assets/images/hemoglobin_b_factors.png){: .align-center}
(Top): Human hemoglobin colored according to the GNM calculated theoretical B-factors (left) and the experimental B-factors (right). (Bottom): 2D plot comparing the theoretical and experimental B-factors of subunit $$ \alpha_1 $$ (chain A of the protein). $$ \alpha_1 $$ is located at the top left quarter of the protein figure. A correlation coefficient of 0.63 was calculated between the theoretical and experimental B-factors.
{: style="font-size: medium;"}

### Slow Modes

A benefit from decomposing the protein fluctuation into individual normal modes is that we are able to observe the characteristics of slow modes separately, i.e. which residues does it affect and to what degree, or **slow mode shape**. This is typically done by visualizing the modes as 2D plots where the x-axis is the residue sequence and the y-axis is the inverse eigenvalues of the Kirchhoff matrix. Peaks in the plot indicate which region of residues the mode describes, with higher peaks representing greater magnitude of motions. It is also common to observe the plot of the average of multiple modes to see the collective contribution of the modes. Below is an example of slow mode shape using human hemoglobin. 

![image-center](../assets/images/hemoglobin_mode_shape.png){: .align-center}
(Top): Visualization of human hemoglobin colored based on GNM slow mode shape. Red represents regions of high mobility and correspond to peaks in the plot. The first image represents the slowest mode (left) and the second image represents the average of the first 10 slowest modes (right). (Bottom): 2D plot of the slowest mode separate by the four chains of hemoglobin.
{: style="font-size: medium;"}

Similar to cross-correlation, analyzing slow mode shapes will give us insight on the structure of the protein and comparing the slow mode shapes can reveal differences between protein structures. From the shape of the slowest mode of all four chains (subunits), we can see that the shape for the four subunits of hemoglobin are quite similar. However, it is important to realize that the slowest mode only captures the largest movements of the protein. Therefore, we cannot say with certainty that the four subunits are as structurally similar as the slow mode shape, although from the cross-correlation map patterns and experimental studies, we know that subunit $$ \alpha $$ and subunit $$ \beta $$ are similar but have structural differences. As mentioned before, we can also view the average shape of the modes. Below is the slow mode plot of the slowest ten modes of hemoglobin. Here, we can see a stark difference between two groups of subunits/chains, where the $$ \alpha $$ subunits share a very similar slow mode shape while the $$ \beta $$ subunits share a different, yet similar, slow mode shape as well. 

![image-center](../assets/images/hemoglobin_mode_shape_avg.png){: .align-center}
The average mode shape of the slowest ten modes of human hemoglobin using GNM.
{: style="font-size: medium;"}

There are two more commonly used plots used in mode analysis. The first is called the **frequency dispersion** of the modes, which is the plot of representing the frequency of each mode. The y-axis represents the reciprocal of the corresponding eigenvalue of the mode, where a higher value indicates a slow mode with low frequency, which are expected to be highly related to biological functions. 

![image-center](../assets/images/hemoglobin_frequency.png){: .align-center}
The frequency dispersion of modes in human hemoglobin. Higher values indicates low frequency, slower modes that are likely to be highly relative to biological functions.
{: style="font-size: medium;"}

The **degree of collectivity** is the measure of the extent of structural elements, in this case residues, that move together for each mode. The degree of collectivity of the $$ k^{th} $$ mode is calculated by the following equation:

$$ Collectivity_k = \frac{1}{N} e^{- \sum^N_i \Delta R_i^2 ln \Delta R_i^2} $$

where N is the total number of residues. A high degree of collectivity indicates that the mode is highly cooperative and engages in a large portion of the structure. Low degree of collectivity indicates that the mode only affects a small region. Modes of high degree of collectivity are generally believed to be functionally relevant nodes and are usually found at the low frequency end of the mode spectrum.

![image-center](../assets/images/hemoglobin_collectivity.png){: .align-center}
The degree of collectivitiy of modes in human hemoglobin. Higher values indicate modes that describe a large portion of the protein while low values indicate modes that describe small local regions.
{: style="font-size: medium;"}

### ANM

<!--
Following paragraph taken from conclusion_part_2
-->

The anisotropic counterpart to GNM, in which the direction of fluctuations is also considered, is called **anisotropic network model (ANM)**. The main difference in ANM analysis is that a Hessian matrix, $$ H $$, is used in place of the Kirchhoff matrix. Each element $$ H_{ij} in the matrix is a 3x3 matrix that contain anisotropic information about the orientation of node *i* and node *j*. The calculations proceeds similarly to GNM, where eigen decomposition is used to calculate cross correlation and mean square fluctuations. Although ANM includes directionality, it typically performs worse than GNM when compared with experimental data [^Yang]. However, this model offers the benefit of creating animations depicting the range of motions and fluctuations of the protein because of the inclusion of orientation. We will not go in depth regarding the intricacies of ANM calculations in this module, but we will use ANM for the purpose of creating animations to visualize protein fluctuations.

![image-center](../assets/images/hemoglobin_anm_2.gif){: .align-center}
Collective motions of the slowest mode in human hemoglobin from ANM calculations using DynOmics.
{: style="font-size: medium;"}

For those interested, a full treatment of the mathematics of GNMs can be found in the chapter at <a href="https://www.csb.pitt.edu/Faculty/bahar/publications/b14.pdf" target="_blank">https://www.csb.pitt.edu/Faculty/bahar/publications/b14.pdf</a>.

* Link to DynOmics Tutorial

* SARS-CoV-2 Analysis? Showing that the two proteins are very similar. Correlation of B-factor is bad... Protein too large to get accurate results? Possibly scrap the SARS-CoV analysis. More of a bonus?

[^Yang]: Yang, L., Song, G., Jernigan, R. 2009. Protein elastic network models and the ranges of cooperativity. PNAS 106(30), 12347-12352. https://doi.org/10.1073/pnas.0902159106

[^Yang2]: Yang, L., Song, G., & Jernigan, R. L. 2009. Comparisons of experimental and computed protein anisotropic temperature factors. Proteins, 76(1), 164–175. https://doi.org/10.1002/prot.22328

[^Garrett]: Garrett, R. H., Grisham, C. M., 2010. *Biochemistry*, 4th ed. Brooks/Cole, Cengage Learning. 
