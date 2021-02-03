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

The next step is to construct a **Kirchhoff matrix**, represented by the symbol ** $$ \Gamma $$ **, such that:

$$ \Gamma_{ij} = \begin{cases} & $-1$ \indent \text{if $i \neq j$ and $R_{ij} \leq r_c$}\\ &  0 \indent \text{ if $i \neq j$ and $R_{ij} > r_c$} \end{cases} $$

$$ \Gamma_{ii} = -\sum_j \Gamma_{ij} $$

<!--
In case latex doesn't work, use these embeds

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\Gamma_{ij}&space;=&space;\begin{cases}&space;&&space;$-1$&space;\indent&space;\text{if&space;$i&space;\neq&space;j$&space;and&space;$R_{ij}&space;\leq&space;r_c$}\\&space;&&space;0&space;\indent&space;\text{&space;if&space;$i&space;\neq&space;j$&space;and&space;$R_{ij}&space;>&space;r_c$}&space;\end{cases}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\Gamma_{ij}&space;=&space;\begin{cases}&space;&&space;$-1$&space;\indent&space;\text{if&space;$i&space;\neq&space;j$&space;and&space;$R_{ij}&space;\leq&space;r_c$}\\&space;&&space;0&space;\indent&space;\text{&space;if&space;$i&space;\neq&space;j$&space;and&space;$R_{ij}&space;>&space;r_c$}&space;\end{cases}" title="\Gamma_{ij} = \begin{cases} & $-1$ \indent \text{if $i \neq j$ and $R_{ij} \leq r_c$}\\ & 0 \indent \text{ if $i \neq j$ and $R_{ij} > r_c$} \end{cases}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\Gamma_{ii}&space;=&space;-\sum_j&space;\Gamma_{ij}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\Gamma_{ii}&space;=&space;-\sum_j&space;\Gamma_{ij}" title="\Gamma_{ii} = -\sum_j \Gamma_{ij}" /></a>
-->

where $$r_c$$ is the threshold distance. Simply put, if residue i and residue j are connected, then the value of position i,j in the matrix will be -1. If they are not connected, the the value will be 0. The values of the diagonals, i.e. position i,i, correspond to the total number of connections of residue i. 

![image-center](../assets/images/kirchhoff_example.png){: .align-center}
Toy structure and the corresponding Kirchhoff matrix.
{: style="font-size: medium;"}

One of the most common analysis using GNM is on the coordinated movement between residues as the protein fluctuates. More specifically, we want to see how each residue will move relative to other residues, or the **cross-correlation** between the residues. The cross-correlation between some residue *i* and residue *j* can be mathmatically calculated as follows:

$$ \langle \Delta R_i \cdot \Delta R_j \rangle = \frac{3 k_B T}{\gamma} \left[ \Gamma^{-1} \right]_{ij} $$





