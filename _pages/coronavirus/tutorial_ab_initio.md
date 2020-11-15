---
permalink: /coronavirus/tutorial_ab_initio
title: "Using ab initio Modeling to Predict the Structure of Hemoglobin Subunit Alpha"
sidebar:
 nav: "coronavirus"
---

In this software tutorial, we will use the popular *ab initio* modeling software called QUARK. Because of the difficulty of quickly and accurately reconstructing the structure of a long protein, QUARK limits us to only polypeptides with at most 200 amino acids.

Because the SARS-CoV-2 spike protein is 1281 amino acids, we will demonstrate how to use *QUARK* to predict the structure of a smaller protein. In what follows, we will use human hemoglobin subunit alpha (PDB entry [1si4](https://www.rcsb.org/structure/1sI4)), which is 141 amino acids long.

Before beginning, if you have not used QUARK before, then you will need to *<a href="https://zhanglab.ccmb.med.umich.edu/QUARK2/registration/" target="_blank">register for a QUARK account</a>* to use this software. After registering, you will receive an email containing a temporary password.

Then, [download the sequence](../_pages/coronavirus/files/Human_Hemoglobin_subunit_alpha_Seq.txt) of the protein. Go to *<a href="https://zhanglab.ccmb.med.umich.edu/QUARK2/" target="_blank">QUARK</a>* to find the submission page for QUARK, where you should take the following steps as shown in the figure below.

1. Copy and paste the sequence into the first box.
2. Add your email address and password.
3. Click `Run QUARK`.

![image-center](../assets/images/QuarkTutorial.png){: .align-center}
{: style="font-size: medium;"}

Even though this is a short protein, it will take at least a few hours to run your submission, depending on server load. When your job has finished, you will receive an email notification and the ability to download the results.

In the meantime, you may like to join us back in the main text. QUARK will not return a single best answer but rather the top five best-scoring structures that it finds. We will show a figure of our models and compare them to the known structure of human hemoglobin subunit alpha from the PDB entry <a href="https://www.rcsb.org/structure/1sI4" target="_blank">1si4</a>. You can also <a href="../_pages/coronavirus/files/QUARK_Hemoglobin.tar.bz2" download>download</a> our completed models if you like.

[Return to main text](ab_initio#toward-a-faster-approach-for-protein-structure-prediction){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
