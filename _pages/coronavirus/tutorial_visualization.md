---
permalink: /coronavirus/tutorial_visualization
title: "Visualizing Regions and Residues"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

In this tutorial, we will discuss how to change the visualization of a molecule and highlight specific residues. Here we will focus on the site that we identified in the previous lesson where the SARS-CoV-2 RBD and SARS RBD differ structurally. Specifically, we will visualize the site in the SARS-CoV-2 RBD using PDB entry 6vw1, which contains the structure of the SARS-CoV-2 chimeric RBD complexed with ACE2. Be sure to have installed VMD and know how to load molecules into the program. If you need a refresher, go to the <a href="tutorial_multiseq" target="_blank">VMD and Multiseq Tutorial</a>.

First, download the PDB entry <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a> and load it into VMD.

To change the visualization of the molecule or parts of the molecule, click `Graphics > Representation`.

![image-center](../assets/images/Ridge4.png){: .align-center}
{: style="font-size: medium;"}

Here, you can add, delete, or edit the visual representations. Double clicking on a representation will disable/enable it. The file `6vw1.pdb` contains two biological assemblies of the structure. The first is made up of Chain A (ACE2) and Chain E (RBD), and the second is Chain B (ACE2) and Chain F (RBD). We will only focus on the second assembly.

First, we will focus only on visualizing Chain B and excluding the rest. We will add in the other parts of the structure as we go.

* `Selected Atoms` allows you to select specific parts of the molecule. The keyword “all” selects all atoms in the file, so we will need to change it. Replace "all" with 'chain B'. Then, click on `Apply`. (In general, to choose a specific chain, use the keyword 'chain X', where X is the chain of choice. To choose a specific residue, use the keyword 'resid #'. Keywords can be combined using keywords “and” or ”or”, and more complicated selections need parenthesis.)

* We will change the color of chain B to be green. `Coloring Method` allows you to change the coloring method of the selected atoms. This includes coloring based on element, amino acid residue, and many more. To choose a specific color, select `ColorID`. A drop-down list will appear to color selection. Choose "7" to select green.

* `Drawing Method` allows you to change the visualization of the selected atoms. "Lines" (also known as `wireframe`) simply draws a line between atoms to represent bonds. "Tube" will focus only on the backbone of the molecule. "Cartoon/NewCartoon" will show secondary structure of the molecule (protein). "Licorice" will show the backbone and the sidechains of the molecule (protein). We will choose "Tube" to only show the backbone of ACE2.

![image-center](../assets/images/Ridge5.png){: .align-center}
{: style="font-size: medium;"}

At this point, your `OpenGL Display` window should look like this:

![image-center](../assets/images/Ridge6.png){: .align-center}
{: style="font-size: medium;"}

Now, we will add in Chain F (SARS-CoV-2 chimeric RBD) and change the color to purple. Click on `Create Rep`, which will duplicate the previous representation. Then, change `Selected Atoms` to "chain F" and `ColoringID` to "11" for purple. Make sure the choices are as follows:

![image-center](../assets/images/Ridge7.png){: .align-center}
{: style="font-size: medium;"}

You should now see two distinct structures based on color! (Remember that ACE2 is in green and the RBD is in purple.)

![image-center](../assets/images/Ridge8.png){: .align-center}
{: style="font-size: medium;"}

We can also change the visualization to target specific residues (amino acids) within the structure. This is easily done by creating another representation and specifying the residue with the keyword "resid" followed by the residue number. Let's say that we were interested in residue 486 in the RBD. Click on `Create Rep`. In the new representation, change `Selected Atoms` to "chain F and resid 486" and click `Apply`. Then change the `Coloring Method` to "ColorID" and "4". Finally, change the `Drawing Method` to "Licorice".

![image-center](../assets/images/Ridge8-1.png){: .align-center}
{: style="font-size: medium;"}

In `OpenGL Display`, you will now see a new yellow projection coming out of the RBD like the image below. This is residue 486! You may need to rotate the protein around to see it.

![image-center](../assets/images/Ridge8-2.png){: .align-center}
{: style="font-size: medium;"}

Now as an exercise, lets make representations for the following residues: Phe486, Ala475, and Asn487 of the RBD, and Met82, Leu79, Tyr83, Ser19, and Gln24 of ACE2. It is very similar to the previous steps of just adding new representations and changing `Selected Atoms`, `Coloring Method`, and `Drawing Method`. Make the following representations.

![image-center](../assets/images/Ridge9.png){: .align-center}
{: style="font-size: medium;"}

The final visualization should look like this:

![image-center](../assets/images/Ridge10.png){: .align-center}
{: style="font-size: medium;"}

Congratulations! You have now created a more detailed visualization the focuses on our targeted site. You may be wondering why we chose to highlight these specific amino acid residues. These residues actually play a role in increasing the binding affinity of SARS-CoV-2 S protein and ACE2. We will discuss how back in the main text.

[Return to main text](structural_diff){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
