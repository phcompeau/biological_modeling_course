---
permalink: /coronavirus/tutorial_NAMD
title: "VMD Plugin: NAMD Energy"
sidebar: 
 nav: "coronavirus"
toc: true
toc_sticky: true
---

In this tutorial, will use NAMD Energy to calculate the interaction energy between SARS-CoV-2 RBD and ACE2 as well as compute how much interaction energy the loop site contributes. Be sure to have installed VMD and know how to load molecules into the program. If you need a refresher, go to the <a href="tutorial_multiseq" target="_blank">VMD and Multiseq Tutorial</a>. In addition to VMD, make sure to download <a href="https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=NAMD" target="_blank">NAMD</a>. One of the steps may require you to provide the path to *NAMD*.

First, load <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a> into VMD. Afterwards, we will need to create a protein structure file (<a href="https://www.ks.uiuc.edu/Training/Tutorials/namd/namd-tutorial-unix-html/node23.html" target="_blank">PSF</a>) of 6vw1 in order to simulate the molecule. We will be using the VMD plugin *Atomatic PSF Builder* to create the file. From *VMD Main*, go to *Extensions>Modeling>Automatic PSF Builder*.

<img src="../_pages/coronavirus/files/NAMDTutorial/Image1.png">

In the *AutoPSF* window, make sure that the selected molecule is *6vw1.pdb* and the output to be *6vw1_autopsf*. Next click *Load input files*. In step 2, click *Protein* and then *Guess and split chains using current selections*. Afterwards, click *Create chains* and then *Apply patches and finish PSF/PDB*. 

<img src="../_pages/coronavirus/files/NAMDTutorial/Image2.png">

During this process, it is possible to see an error message stating "MOLECULE DESTROYED". If you see this message, click "Reset Autopsf" and repeat the steps again. The selected molecule will change, so make sure that the molecule is *6vw1.pdb* when you start over. Failed molecules remain in VMD, so deleting the failed molecule from *VMD Main* is recommended before each attempt.

<img src="../_pages/coronavirus/files/NAMDTutorial/Image3.png">

If the PSF file is successfully created, you will see a message stating "Structure complete." *VMD Main* also have an additional line.

<img src="../_pages/coronavirus/files/NAMDTutorial/Image4.png">

<img src="../_pages/coronavirus/files/NAMDTutorial/Image5.png">

Now that we have the PSF file, we can proceed to NAMD Energy. In *VMD Main*, go to *Extensions>Analysis>NAMD Energy*.

<img src="../_pages/coronavirus/files/NAMDTutorial/Image6.png">

The *NAMDEnergy* window will show up. First, change the molecule to be the PSF file. We want to calculate the interaction energy between the RBD and ACE2. Recall that the corresponding chain pairs are chain A (ACE2)/chain E (RBD) and chain B (ACE2)/chain F (RBD). Here we will use the chain B/F pair. Put "protein and chain B" and "protein and chain F" for *Selection 1* and *Selection 2*, respectively. Next, we want to calculate the main protein-protein interaction energies, electrostatic and van der Waals. Under *Output File*, enter in the desired name for the results. Next, we need to give NAMDEnergy the parameter file *par_all36_prot.prm*. This should be located at *VMD>plugins>noarch>tcl>readcharmmpar1.3>par_all36_prot.prm*. Finally, click *Run NAMDEnergy*.

<img src="../_pages/coronavirus/files/NAMDTutorial/Image7.png">

The output file will be created in your current working directory, and can be opened with a simple text-editor. The values of the results may vary slightly upon repetitive calculations.

<img src="../_pages/coronavirus/files/NAMDTutorial/Image8.png">

Now, let's calculate the interaction energy between only the SARS-CoV-2 RBD loop site (residues 482 to 486) and ACE2. In the *NAMDEnergy* window, put "protein and chain B" for *Selection 1* and "protein and chain F and (resid 482 to 486)" for *Selection 2*. Keep everything else the same. You should get results similar to this.


<img src="../_pages/coronavirus/files/NAMDTutorial/LoopEnergyOutput.png">
 
You may be curious about why the interaction energy comes out to be a negative number. Just like in physics, the negative value describes the direction of the force. A negative value indicate an attractive force between the two molecules while a positive value indicate a repulsion force. Our results describe the interaction between SARS-CoV-2 RBD and ACE2 as a favorable interaction. The more negative the value, the greater the binding affinity between the two proteins.

Now, let's go back to the main text to interpret our results.

[Return to main text](NAMD){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
