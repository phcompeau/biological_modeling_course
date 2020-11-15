---
permalink: /coronavirus/tutorial_homology
title: "Using Homology Modeling to Predict the Structure of the SARS-CoV-2 Spike Protein"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

Three protein prediction software, SWISS-MODEL, GalaxyWEB, and Robetta were chosen for template-based modeling of the SARS-CoV-2 S protein. Recall that the S protein is a homotrimer, meaning that it consists of three identical protein sturctures, or chains. Therefore, we will use the sequence of a single chain for modeling.

## SWISS-MODEL
<a href="https://swissmodel.expasy.org/" target="_blank">SWISS-MODEL</a> is a well-known structural bioinformatics web-server that specializes in homology modeling. The pipeline is comprised for four steps. 1) Using BLAST and HHblits, templates are identified and stored in the SWISS-MODEL Template Library. 2) The target sequence and template structure(s) are aligned. 3) Building the predicted models through a rigid fragment assembly approach. 4) Qualitiative Model Energy Analysis (QMEAN), a composite scoring function for model quality assessment.

First, download the sequence of the chain: <a href="/multiscale_biological_modeling/_pages/coronavirus/files/CoV2SpikeProteinSeq.txt" download>SARS-CoV-2 S Chain Sequence</a>

Next, head over to main page of <a href="https://swissmodel.expasy.org/" target="_blank">SWISS-MODEL</a> and click on *Start Modelling*.

![image-center](../assets/images/SWISS1.png){: .align-center}
{: style="font-size: medium;"}

In the next page, copy and paste the sequence into the *Target Sequence(s):* box. Give the project a name and enter an email address to get a notification of when your results are ready. Finally, click on *Build Model* to submit the job request.

![image-center](../assets/images/SWISS2.png){: .align-center}
{: style="font-size: medium;"}

Your results may take from an hour to a day depending on how busy the server is. Once you get an email notification saying that your model is ready, follow the link and you can download the models. For our job, SWISS-MODEL used the one of the PDB entries of SARS S protein as the template (<a href="https://www.rcsb.org/structure/6CRX" target="_blank">6crx</a>) and recognized that it was a homotrimer. As a result, the predicted models were of the whole S protein with all three chains included. The tertiary structure of our results and the real structure of the full S protein from the PDB entry <a href="http://www.rcsb.org/structure/6VXX" target="_blank">6vxx</a> can be seen below. You can also download our results if you wish.

![image-center](../assets/images/SWISSResults.png){: .align-center}
Tertiary structures of the PDB entry (6vxx) and homology models from SWISS-MODEL of the full SARS-CoV-2 S protein. The superposed structures of all structures is shown on the right.
{: style="font-size: medium;"}

<a href="/multiscale_biological_modeling/_pages/coronavirus/files/SWISS_Model.zip" download>SWISS-MODEL Results</a>

## Robetta
<a href="https://robetta.bakerlab.org/" target="_blank">Robetta</a> is a web-server that provides comparative and *ab initio* modeling of protein domains by utilizing the Rosetta fragment insertion method and de novo protocols. It allows for custom sequence alignments and can model multi-chain complexes. Robetta was chosen to model a single chain of the SARS-CoV-2 Spike protein. We will also be modeling a single chain of the SARS-CoV-2 S protein here.

First, if you had not already done so, download the sequence of the chain: <a href="/multiscale_biological_modeling/_pages/coronavirus/files/CoV2SpikeProteinSeq.txt" download>SARS-CoV-2 S Chain Sequence</a>

Go to <a href="https://robetta.bakerlab.org/" target="_blank">Robetta</a> and register for an account.

![image-center](../assets/images/Robetta1.png){: .align-center}
{: style="font-size: medium;"}

After you are done, go to *Structure Prediction>Submit*.

![image-center](../assets/images/Robetta2.png){: .align-center}
{: style="font-size: medium;"}

Create a name for the job, i.e. "SARS-CoV-2 Spike Chain". Copy and paste the sequence into the *Protein sequence* box. Check *CM only* (for comparative/homology modeling), complete the simple arithmetic problem and finally click *Submit* to submit the job. Your results may take between an hour to a day. You will get an email notification after the job is complete, and you will be able to download the results. The tertiary structure of our results and the real structure of one chain of the S protein from the PDB entry <a href="http://www.rcsb.org/structure/6VXX" target="_blank">6vxx</a> can be seen below. You can also download our results if you wish.

![image-center](../assets/images/RobettaResults.png){: .align-center}
Tertiary structures of the PDB entry (6vxx) and homology models from Robetta of one of the chains of the SARS-CoV-2 S protein. The superposed structures of all structures is shown on the right.
{: style="font-size: medium;"}

<a href="/multiscale_biological_modeling/_pages/coronavirus/files/Robetta_Model.zip" download>Robetta Results</a>

## GalaxyWEB
<a href="http://galaxy.seoklab.org/" target="_blank">GalaxyWEB</a> is a web-server with many available services including protein structure prediction, structure refinement, protein interaction prediction, and GPCR applications. GalaxyTBM (the template-based modeling service) uses *<a href="https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-019-3019-7" target="_blank">HHsearch</a>* to identify up to 20 templates, then matches the core sequence with the templates using *<a href="http://prodata.swmed.edu/promals3d/info/promals3d_help.html" target="_blank">PROMALS3D</a>*. Next, models are generated using *<a href="https://pubmed.ncbi.nlm.nih.gov/19089941/" target="_blank">MODELLERCSA</a>*.

Because GalaxyWEB as a sequence limit of 1000 amino acids, we cannot use the S protein chain. Instead, we can model the Receptor Binding Domain (RBD) of the S protein instead.

First, download sequence:
<a href="/multiscale_biological_modeling/_pages/coronavirus/files/CoV2SpikeRBDSeq.txt" download>CoV-2 RBD Sequence</a>

Go to <a href="http://galaxy.seoklab.org/" target="_blank">GalaxyWEB</a>. At the top, go to *Services>TBM*.

![image-center](../assets/images/Galaxy1.png){: .align-center}
{: style="font-size: medium;"}

Enter a job name, i.e. SARS-CoV-2 RBD. Enter an email address and then copy and paste the sequence into the *SEQUENCE* box. Finally, click *Submit* to submit the job request.


![image-center](../assets/images/Galaxy2.png){: .align-center}
{: style="font-size: medium;"}

Your results will be done within a day and you will recieve an email notification. Then, you will be able to download your results. The tertiary structure of our results and the real structure of the S protein RBD from the PDB entry <a href="http://www.rcsb.org/structure/6LZG" target="_blank">6lzg</a> can be seen below. You can also download our results if you wish.
![image-center](../assets/images/GalaxyResults.png){: .align-center}
Tertiary structures of the PDB entry (6lzg) and homology models from GalaxyWEB of SARS-CoV-2 S protein RBD. The superposed structures of all structures is shown on the right.
{: style="font-size: medium;"}

<a href="/multiscale_biological_modeling/_pages/coronavirus/files/GalaxyWEB_Models.zip" download> GalaxyWEB Results</a>

<hr>

In the next lesson, we will learn how to assess the accuracy of our predicted models and compare it to our previous *ab initio* results of human hemoglobin subunit a.


[Return to main text](homology#applying-homology-modeling-to-sars-cov-2){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
