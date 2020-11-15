---
permalink: /coronavirus/tutorial_homology
title: "Using Homology Modeling to Predict the Structure of the SARS-CoV-2 Spike Protein"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

In this software tutorial, we will apply three popular software resources (SWISS-MODEL, Robetta, and GalaxyWEB) for homology modeling of the SARS-CoV-2 S protein. Recall that the S protein is a homotrimer, meaning that it consists of three identical protein structures, or chains. Therefore, we will predict the sequence of a single chain in what follows.

## SWISS-MODEL
The SWISS-MODEL pipeline comprises four steps.

1. Using BLAST and HHblits, templates for the input protein are identified and stored in the SWISS-MODEL Template Library.
2. Alignment of target sequence and template structure(s) are aligned.
3. Building the predicted models via fragment assembly.
4. Assessing model quality through an approach called qualitative model energy analysis (QMEAN).

First, download the sequence of the spike protein chain: <a href="/multiscale_biological_modeling/_pages/coronavirus/files/CoV2SpikeProteinSeq.txt" download>SARS-CoV-2 spike protein chain</a>.

Next, go to the main <a href="https://swissmodel.expasy.org/" target="_blank">SWISS-MODEL website</a> and click on `Start Modelling`.

![image-center](../assets/images/SWISS1.png){: .align-center}
{: style="font-size: medium;"}

On the next page, copy and paste the sequence into the `Target Sequence(s):` box. Name your project and enter an email address to get a notification of when your results are ready. Finally, click on `Build Model` to submit the job request. Note that you do not need to specify that you want to use the SARS-CoV spike protein as a template because the software will automatically search for a template for you.

![image-center](../assets/images/SWISS2.png){: .align-center}
{: style="font-size: medium;"}

Your results may between an hour and a day to finish depending on how busy the server is. (In the meantime, you should run the remaining software.) When you receive an email notification, follow the link provided and you can download the final models.

When we ran our job, SWISS-MODEL did indeed use the one of the PDB entries of SARS-CoV spike protein as the template (<a href="https://www.rcsb.org/structure/6CRX" target="_blank">6crx</a>) and correctly recognized that the template was a homotrimer. As a result, the software predicted a complete spike protein with all three chains included. An image of our results along with the known structure of the spike protein from the PDB entry <a href="http://www.rcsb.org/structure/6VXX" target="_blank">6vxx</a> can be seen below. You can also <a href="../_pages/coronavirus/files/SWISS_Model.zip" download>download</a> our results.

![image-center](../assets/images/SWISSResults.png){: .align-center}
Structures of the SARS-CoV spike protein (PDB entry 6vxx) and the three models of the SARS-CoV-2 S protein reported by SWISS-MODEL. The superposed structures of all structures is shown on the right.
{: style="font-size: medium;"}

## Robetta
Robetta is a publicly available software resource that uses the same software as the distributed Rosetta@home project that we mentioned earlier in this module. As with SWISS-MODEL, we will provide Robetta a single chain of the SARS-CoV-2 spike protein.

First, if you had not already done so, download the sequence of the chain: <a href="/multiscale_biological_modeling/_pages/coronavirus/files/CoV2SpikeProteinSeq.txt" download>SARS-CoV-2 spike chain sequence</a>.

Next, visit <a href="https://robetta.bakerlab.org/" target="_blank">Robetta</a> and register for an account.

![image-center](../assets/images/Robetta1.png){: .align-center}
{: style="font-size: medium;"}

Then, click on `Structure Prediction > Submit`.

![image-center](../assets/images/Robetta2.png){: .align-center}
{: style="font-size: medium;"}

Create a name for the job, i.e. "SARS-CoV-2 Spike Chain". Copy and paste the downloaded sequence into the `Protein sequence` box. Check `CM only` (for homology modeling), complete the arithmetic problem provided to prove you are human, and then click `Submit`.

You should receive an email notification with a link to results after between an hour and a day. Unlike SWISS-MODEL, Robetta did not deduce that the input protein was a trimer and only predicted a single chain. The structure of the results returned in our own run of Robetta and the real structure of one chain of the SARS spike protein from the PDB entry <a href="http://www.rcsb.org/structure/6VXX" target="_blank">6vxx</a> are visualized in the figure below. You can also <a href="../_pages/coronavirus/files/Robetta_Model.zip" download>download</a> our results if you like.

![image-center](../assets/images/RobettaResults.png){: .align-center}
The structure of the SARS-CoV spike protein (PDB entry 6vxx) as well as homology models produced by Robetta of one of the chains of the SARS-CoV-2 S protein. The superimposition of all structures is shown on the right.
{: style="font-size: medium;"}

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

Your results will be done within a day and you will recieve an email notification. Then, you will be able to download your results. The tertiary structure of our results and the real structure of the S protein RBD from the PDB entry <a href="http://www.rcsb.org/structure/6LZG" target="_blank">6lzg</a> can be seen below. You can also <a href="../_pages/coronavirus/files/GalaxyWEB_Models.zip" download>download</a> our results if you like.
![image-center](../assets/images/GalaxyResults.png){: .align-center}
Tertiary structures of the PDB entry (6lzg) and homology models from GalaxyWEB of SARS-CoV-2 S protein RBD. The superposed structures of all structures is shown on the right.
{: style="font-size: medium;"}



<hr>

In the next lesson, we will learn how to assess the accuracy of our predicted models and compare it to our previous *ab initio* results of human hemoglobin subunit a.


[Return to main text](homology#applying-homology-modeling-to-sars-cov-2){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
