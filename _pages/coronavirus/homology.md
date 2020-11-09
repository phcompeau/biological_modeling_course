---
permalink: /coronavirus/homology
title: Homology Modeling for Protein Structure Prediction
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

* In the previous section, we stated that *ab initio* structure prediction may lead to more inaccurate results as the size of the protein increases with our current algorithms. So, we cannot confidently predict the large SARS-CoV-2 S protein using *ab initio* structure prediction. As mentioned in the *<a href="structure_intro">Introduction to Protein Structure Prediction</a>*, we have over 160,000 protein structure entries available in the PDB. With every new structure we identify, we learn a little more about the rules that nature uses to fold proteins into shapes. Within these entries are published structures of the SARS S protein, which we know is similar to the SARS-CoV-2 S protein based on the genomic sequence. So, why not leverage this database in predicting the SARS-CoV-2 S protein and other structures? This is exactly how homology modeling and other template-based methods approach structure prediction. By using protein structures in the PDB as templates to guide us in structure prediction, we are able to improve prediction accuracy and allow us to more confidently predict the structure of large proteins. 

* Homology modeling is based on the observation that proteins from the same evolutionary family with similar sequences typically adopt similar structures. Using sequence comparisons, we can scour the database for proteins with notable sequence identity to be used as a template. Now that we have a starting point, the chances of predicting the correct structure is greatly improved, as well as potentially speeding up the process.

* Unfortunately, there are occasions where no identified proteins have notable sequence similarities. The alternative is to use threading, or fold recognition. In this case, rather than comparing the target sequence to sequences in the database, this method compares the target sequence to structures themselves. The biological basis of this method is that in nature, protein structures tend to be highly conservative and unique structural folds are therefore limited. (If it ain’t broke… Of course, mutations and structure deviations occur over evolutionary timeline.) A simple explanation of the general threading algorithm is that structure predictions are created by placing or “threading” each amino acid in the target sequence to template structures from a non-redundant template database, and then assessing how well it fits with some scoring function[^score]. Then, the best-fit templates are used to build the predicted model. The scoring function varies per algorithm, but it typically takes secondary structure compatibilities, gap penalties during alignment, and other terms that depend on amino acids that are bought into contact by the alignment.

Here is a short YouTube video by *"Chee-Onn Leong"* that briefly goes over homology modeling and threading.

<iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/OEzVUrhtZ6s?t=82" frameborder="0" allowfullscreen></iframe>

Here is a flowchart of Yang Zhang Lab's threading method, I-TASSER [^tasser]. There is no need to understand each step, but recognize that even with templates, structure prediction is a complicated and computationally intensive process.

<img src="../_pages/coronavirus/files/ITASSER.png">

For those that want to know more about I-TASSER, the full description of the  can be found <a href="http://europepmc.org/backend/ptpmcrender.fcgi?accid=PMC2849174&blobtype=pdf" target="_blank">here</a>.

<hr>

In this tutorial, we will use model the SARS-CoV-2 S protein using publically available homology modeling servers.

[Visit tutorial](tutorial_homology){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

<hr>
In the tutorial, we used three different servers (SWISS-MODEL, Robetta, and GalaxyWEB) to predict the SARS-CoV-2 S protein in the tutorial. The reason we used multiple servers was because different structure prediction software use different algorithms, and comparing the results will give us more insight on protein prediction modeling. In the next lesson, we will learn how to assess the accuracy of predicted models and see how well our models performed.

If you like, you can dowload the results of our predicted models here:

|Structure Prediction Server|Results|
|:--------------------------|:------|
|SWISS-MODEL (S protein)|<a href="/multiscale_biological_modeling/_pages/coronavirus/files/SWISS_Model.zip" download>SWISS-MODEL Results</a>|
|Robetta (Single-Chain S protein)|<a href="/multiscale_biological_modeling/_pages/coronavirus/files/Robetta_Model.zip" download>Robetta Results</a>|
|GalaxyWEB|<a href="/multiscale_biological_modeling/_pages/coronavirus/files/GalaxyWEB_Models.zip" download> GalaxyWEB Results</a>|

[Next lesson](accuracy){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

[^score]: Movaghar, A. F., Launay, G., Schbath, S., Gibrat, J. F., & Rodolphe, F. 2012. Statistical significance of threading scores. Journal of computational biology : a journal of computational molecular cell biology, 19(1), 13–29. https://doi.org/10.1089/cmb.2011.0236

[^tasser]: Roy, A., Kucukural, A., Zhang, Y. 2010. I-TASSER: a unified platform for automated protein structure and function prediction. Nat Protoc, 5(4), 725-738. https://doi.org/10.1038/nprot.2010.5.
