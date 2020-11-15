---
permalink: /coronavirus/tutorial_ab_initio
title: "Ab initio Structure Prediction Tutorial"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

* Note to self: make sure we mention multiple models

* Because this type of structure prediction is incredibly difficult and computationally intensive, there are still many problems with current *ab initio* algorithms. Perhaps one of the largest setbacks is that current algorithms severely limit the length of the input sequence in order to preserve accuracy and reduce runtime.

The leading *ab initio* modeling algorithm *QUARK* limits the input to only 200 amino acids. As a result, we cannot use *ab initio* structure prediction on the RBD of SARS-CoV-2 (about 229 amino acids long according to PDB entry <a href="https://www.rcsb.org/structure/6M0J" target="_blank">6m0j</a>), let alone one of the chains of the S protein (about 1281 amino acids long).

* Nevertheless, we can use *QUARK* to try to model a smaller protein. Here we will use the human hemoglobin subunit alpha (from PDB entry <a href="https://www.rcsb.org/structure/1sI4" target="_blank">1si4</a>) , which is 141 amino acids long.

First, download the sequence of the protein:
<a href="/multiscale_biological_modeling/_pages/coronavirus/files/Human_Hemoglobin_subunit_alpha_Seq.txt" download>Hemoglobin Sequence</a>.

Next, go to *<a href="https://zhanglab.ccmb.med.umich.edu/QUARK2/" target="_blank">QUARK</a>* for the job submittion page. Here, we will copy and paste the sequence into the first box. Fill out the desired email adress to recieve the results. If you have never used *QUARK* before, you will need to register for a *QUARK* password. Follow the registration form and you will receive an email containing a six-digit password. Back in the job submittion page, enter your password and then click *Run QUARK*. The results should be finished within a few hours depending on the server load.

![image-center](../assets/images/QuarkTutorial.png){: .align-center}
{: style="font-size: medium;"}


Once your job is finished, you will receive an email notification and the ability to download the results. In the main text, we will show a figure of our models as well as the real tertiary structure of human hemoglobin subunit alpha from the PDB entry <a href="https://www.rcsb.org/structure/1sI4" target="_blank">1si4</a>. You can also download our models if you wish.

<a href="../_pages/coronavirus/files/QUARK_Hemoglobin.tar.bz2" download>QUARK Results</a>

Later in the module, we will learn how to assess the accuracy of our predicted structure models.

[Return to main text](ab_initio){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
