---
permalink: /coronavirus/tutorial_multiseq
title: "VMD and Multiseq"
sidebar:
 nav: "coronavirus"
---

In this tutorial, we will show how to get started with VMD and then calculate Qres of the alignment of SARS-CoV-2 RBD and SARS RBD using the VMD plugin Multiseq and PDB entries of the RBDs: <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a> and <a href="https://www.rcsb.org/structure/2ajf" target="_blank">2ajf</a>, respectively. Afterwards, by locating regions of low Qres, we can identify regions of structural differences between the two.

For this tutorial, please download VMD. The download can be found <a href="https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD" target="_blank">here</a>. The program may prompt you to download additional protein database information during this tutorial. If this occurs, please download the protein database information.

We will need to download the PDB files for 6vw1 and 2ajf. First follow this link to the PDB page for the SARS-CoV-2 RBD <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a>. Click on *Download Files* and select *PDB Format*.

![image-center](../assets/images/Ridge0.png){: .align-center}
{: style="font-size: medium;"}

Next, launch VMD. Three windows will open. *VMD.exe* is the console window, but we do not need to worry about it. *VMD Main* will be where we will be load molecules and change the visualizations. Finally, *OpenGL Display* will display the visualizations. Here, we want to load 6vw1 into VMD. In *VMD Main*, go to *File>New Molecule*. Click on *Browse*, select the molecule (6vw1.pdb) and click *Load*.

![image-center](../assets/images/Ridge1.png){: .align-center}
{: style="font-size: medium;"}
![image-center](../assets/images/Ridge2.png){: .align-center}
{: style="font-size: medium;"}

The molecule should now be listed in *VMD Main* as well as the visualization in the *OpenGL Display*.

![image-center](../assets/images/Ridge3.png){: .align-center}
{: style="font-size: medium;"}

In the *OpenGL Display* window, you can click and drag the molecule to change the orientation. Pressing ‘r’ on the keyboard allows you to rotate the molecule, pressing ‘t’ on the keyboard allows you to translate the molecule, and finally pressing ‘s’ allows you to enlarge or shrink the molecule (or use scroll wheel). Note that left click and right click are different.

Now, we need to load the SARS RBD. Repeat the steps but with <a href="https://www.rcsb.org/structure/2ajf" target="_blank">2ajf</a>. After both molecules are loaded into VMD, start up *Multiseq* by going to *Extensions>Analysis>Multiseq*.

![image-center](../assets/images/Qres1.png){: .align-center}
{: style="font-size: medium;"}

You will see all the chains listed out per file. Both PDB files each contain two biological assemblies of the structureg. The first is made up of Chain A (ACE2) and Chain E (RBD), and the second is Chain B (ACE2) and Chain F (RBD). Because Chain A is identical to Chain B, and Chain E is identical to Chain F, we only need to work with one assembly. Here, we will arbitrarily use the second assembly.

![image-center](../assets/images/Qres2.png){: .align-center}
{: style="font-size: medium;"}

We only want to compare the RBD, so we will only keep chain F of each structure. To remove the other chains, click on the chain and go to *Edit>Cut*.

![image-center](../assets/images/Qres3.png){: .align-center}
{: style="font-size: medium;"}

Go to *Tools>Stamp Structural Alignment* and a new window will open up. Keep all the values and click *OK*.

![image-center](../assets/images/Qres4.png){: .align-center}
{: style="font-size: medium;"}
![image-center](../assets/images/Qres5.png){: .align-center}
{: style="font-size: medium;"}

The structures are now aligned. To see coloring based on *Qres*, go to *View>Coloring>Qres*.

![image-center](../assets/images/Qres6.png){: .align-center}
{: style="font-size: medium;"}

Blue indicates high *Qres* while blue indicates low *Qres*. *OpenGL Display* will now also reflect the color of *Qres* on the aligned structures.

![image-center](../assets/images/Qres7.png){: .align-center}
{: style="font-size: medium;"}

![image-center](../assets/images/Qres8.png){: .align-center}
{: style="font-size: medium;"}

The color blue indicates residues with high Qres, indicated good structural alignment. The color red indicates residues with low Qres, meaning poor structural alignment. In your results, you should see regions of consecutive residues with low Qres. These are the regions where the two RBDs differ structurally.

By finding regions of low Qres, we have identified where the RBDs differ structurally. Now, let's go back to the main text to see which regions are important before we begin to analyze the them.

[Return to main text](multiseq#local-comparison-of-spike-proteins-leads-us-to-a-region-of-interest){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
