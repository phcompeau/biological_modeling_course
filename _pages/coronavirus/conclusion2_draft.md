---
permalink: /coronavirus/conclusion2_draft
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

The next step is to construct a **Kirchhoff matrix**, represented by the symbol ** $$ \Gamma $$ **, such that:

$\Gamma_{ij} = \begin{cases} & $-1$ \indent \text{if $i \neq j$ and $R_{ij} \leq r_c$}\\ &  0 \indent \text{ if $i \neq j$ and $R_{ij} > r_c$} \end{cases}$

$$ \Gamma_{ii} = -\sum_j \Gamma_{ij} $$

<!--

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\Gamma_{ij}&space;=&space;\begin{cases}&space;&&space;$-1$&space;\indent&space;\text{if&space;$i&space;\neq&space;j$&space;and&space;$R_{ij}&space;\leq&space;r_c$}\\&space;&&space;0&space;\indent&space;\text{&space;if&space;$i&space;\neq&space;j$&space;and&space;$R_{ij}&space;>&space;r_c$}&space;\end{cases}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\Gamma_{ij}&space;=&space;\begin{cases}&space;&&space;$-1$&space;\indent&space;\text{if&space;$i&space;\neq&space;j$&space;and&space;$R_{ij}&space;\leq&space;r_c$}\\&space;&&space;0&space;\indent&space;\text{&space;if&space;$i&space;\neq&space;j$&space;and&space;$R_{ij}&space;>&space;r_c$}&space;\end{cases}" title="\Gamma_{ij} = \begin{cases} & $-1$ \indent \text{if $i \neq j$ and $R_{ij} \leq r_c$}\\ & 0 \indent \text{ if $i \neq j$ and $R_{ij} > r_c$} \end{cases}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\Gamma_{ii}&space;=&space;-\sum_j&space;\Gamma_{ij}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\Gamma_{ii}&space;=&space;-\sum_j&space;\Gamma_{ij}" title="\Gamma_{ii} = -\sum_j \Gamma_{ij}" /></a>
-->
