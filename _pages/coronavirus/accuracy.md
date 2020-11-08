---
permalink: /coronavirus/accuracy
title: "Assessing Accuracy of Models"
sidebar: 
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## RMSD

* We went through two methods of protein structure prediction, *ab initio* and template-based (homology and threading), and how there are many competing groups with different approaches. Regardless of how the predicted models are created, we need a way to assess the accuracy of the software and algorithm. We can do this by predicting the structure of a protein already in the Protein Data Bank (PDB) and then comparing the predicted model with the experimentally determined structure.

* One popular method of comparing a predicted model and the actual structure is by calculating the Root Mean Square Deviation (RMSD). RMSD is commonly used in mathematics to quantitatively measure the difference between two paired sets of values.

<a href="https://www.codecogs.com/eqnedit.php?latex=RMSD&space;=&space;\sqrt{\frac{1}{n}\sum_{i-1}^nd^2_i}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?RMSD&space;=&space;\sqrt{\frac{1}{n}\sum_{i-1}^nd^2_i}" title="RMSD = \sqrt{\frac{1}{n}\sum_{i-1}^nd^2_i}" /></a>

where *n* is the number of pairs, *d* is the difference in value or distance between the pair. A lower RMSD value indicates a higher similarity between the two sets and a RMSD value of 0 indicates a perfect fit. Below is a simple example of how RMSD is calculated.

<img src="../_pages/coronavirus/files/SampleRMSD.png">

## RMSD with Protein Structures
 
Because protein structures are stored with atomic coordinates, we can calculate the RMSD between two structures like the example above, given that they have the same number of atoms. In this case, RMSD will be in the units of angstroms ($$10^{-10}$$ meters). However, due to the molecules being dynamic structures and constantly fluctuating, experimentally determined protein structures are not exact. In fact, most structures determined by x-ray crystallography, the most accurate method of structure determination, has a resolution between one to five angstroms. If we were to compare every single atom between two structures, the RMSD score can be greatly inflated Therefore, we typically only use the minimum requirement and compare the positions of the alpha-Carbons of the protein backbone. Just from the positions of the alpha-Carbons, we can get a pretty good idea of the tertiary structure of the protein (refer back to <a href="structure_intro">Introduction to Protein Structure Prediction</a>).

However, there is an additional step for calculating RMSD when we are comparing two 3D structures. Because protein structures can be stored in a different orientations or starting points in the 3D-coordinate plane, we need to first superpose the structures on top of each other and make sure they are in the same orientation (minimizing RMSD). That way, we can make sure that the RMSD score actually represents structural deviation.

First, we translate the structures to the same coordinate point, such as the origin. This is easily done by subtracting the coordinates of the centroid, or average coordinates, from all corresponding point coordinates for both structures. Now, both structures will be superposed on top of each other. The next, most tricky part, is finding the right orientation for the structures. This can be done using the Kabsch Algorithm. The output of the algorithm is a rotation matrix that describes how to rotate one of the structures to match the orientation of the other. 

If you are interested in how the Kabsch algorithm works, click <a href="https://en.wikipedia.org/wiki/Kabsch_algorithm" target="_blank">here</a> (Wikipedia).

After this is done, we can then proceed to calculating the RMSD score between the two structures. The score would represent how much the positions of atoms deviate between the two structures, which is indicative of how different the overall structures are. By calculating RMSD between the protein model and the actual protein entry on PDB, we can assess how well the software and algorithm performed.

<img src="../_pages/coronavirus/files/RMSDExample.png">

STOP: Can you think of example where a small difference between protein structures can cause a large inflation in RMSD score? {: .notice--primary}

*	Unfortunately, there is no established threshold RMSD as scores vary based on protein size (larger proteins mean more fluctuating parts) and the resolution of the structure determination method. In addition, RMSD has its own flaws where a single misplaced loop or an off-angle bond can have profound effects on the score, as shown in the figure below. This is why other methods of structure comparisons are used in conjunction to RMSD for a more thorough comparison analysis. Nonetheless, a score under 2.0 angstroms is typically acceptable when comparing large molecules such as proteins.

<img src="../_pages/coronavirus/files/RMSDCartoon.png">

(This is another cartoon to show the sensitivity of RMSD. We could use either figure)

<img src="../_pages/coronavirus/files/RMSDCartoon2.png">

In this tutorial, we will walk through how to calculate RMSD using two experimentally determined protein structures from the PDB: the SARS-CoV-2 S protein, <a href="http://www.rcsb.org/structure/6VXX" target="_blank">6vxx</a>, and SARS S protein, <a href="https://www.rcsb.org/structure/6CRX" target="_blank">6vrx</a>.

[Visit tutorial](rmsd2){: .btn .btn--primary .btn--x-large}
{: style="font-size: 100%; text-align: center;"}

<hr>

## Accuracy of Our Structure Prediction Models

In the previous tutorials, we used various publically available protein structure servers to predict the structure of the human hemoglobin subunit alpha (*ab initio*) and SARS-CoV-2 S protein (homology). Using the same method of calculating RMSD from the tutorial, let's see how well our models performed.

### *Ab initio* (QUARK) models of Human Hemoglobin Subunit Alpha

In the *<a href="tutorial_ab_initio">ab initio tutorial</a>*, we used *<a href="https://zhanglab.ccmb.med.umich.edu/QUARK/" target="_blank">QUARK</a>* to do *ab initio* structure prediction of the human hemoglobin subunit alpha, producing five models. We can compare these models to the actual structure from the PDB, <a href="https://www.rcsb.org/structure/1SI4" target="_blank">1si4</a> and calculate RMSD scores to see how accurate the models are. Here is a table of the scores:

| Quark Model | RMSD  |
|:------------|:------|
| QUARK1      | 1.58  |
| QUARK2      | 2.0988|
| QUARK3      | 3.11  |
| QUARK4      | 1.9343|
| QUARK5      | 2.6495|

From these scores, we can see that model QUARK1 was the most accurate due to having the smallest RMSD score. However, because human hemoglobin subunit alpha is a small protein (141 amino acids), the score is considered high. Before we make any conclusions, let's see how our homology-based models of larger proteins performed. 

### Homology models of SARS-CoV-2 S protein

In the *<a href="tutorial_homology">homology tutorial</a>*, we used two different homology structure prediction web-servers to predict the structure of SARS-CoV-2 S protein and one web-server to predict the structure of SARS-CoV-2 S protein RBD. In addition to our predicted models, we will also assess five predicted models of the full S protein released by <a href="https://boinc.bakerlab.org/" target="_blank">Rosetta@Home</a> to the *Seattle Structural Genomics Center for Infectious Disease* (SSGCID). Because these SSGCID models were produced using much greater computational power and ran for a long period of time, comparing the RMSD scores with our models will provide more insight on the effect of computational power on accuracy. The SSGCID models can be found <a href="https://www.ssgcid.org/cttdb/molecularmodel_list/?target__icontains=BewuA" target="_blank">here</a>.

#### SWISS-MODEL

<a href="https://swissmodel.expasy.org/" target="_blank">SWISS-MODEL</a> produced models of the entire S protein. We compared each model to the actual structure from the PDB, <a href="https://www.rcsb.org/structure/6vxx">6vxx</a>.

| SWISS MODEL | RMSD |
|:------------|:-----|
|SWISS1| 5.8518|
|SWISS2| 11.3432|
|SWISS3| 11.3432|

From the scores, we see that model SWISS1 performed the best, but is greatly exceeds the generally accepted threshold score of 2.0. However, we have to consider that the full S protein is very large. Recall that the S protein is made up of three identical chains, each around 1281 amino acids long. Because RMSD is very senstive, larger proteins are more prone to higher RMSD values. Nonetheless, these models can be considered as inaccurate.

#### Robetta
 
<a href="https://robetta.bakerlab.org/" target="_blank">Robetta</a> produced five models of a single chain of the S protein. Just like the models from the SWISS-MODEL, we compared the them to the actual structure from the PDB, <a href="https://www.rcsb.org/structure/6vxx">6vxx</a>.

| Robetta | RMSD |
|:--------|:-----|
|Robetta1| 3.1189|
|Robetta2| 3.7568|
|Robetta3| 2.9972|
|Robetta4| 2.5852|
|Robetta5| 12.0975|

We see that the model Robetta4 performed the best, yet still exceeds the 2.0 threshold.

#### SSGCID

As explained above, these SSGCID models of the S protein released by <a href="https://boinc.bakerlab.org/" target="_blank">Rosetta@Home</a> used large amounts of computational power. Therefore, we expect to see RMSD scores lower than those of our models. Like before, we will compare the models to the actual structure from the PDB, <a href="https://www.rcsb.org/structure/6vxx">6vxx</a>. This time, we will assess both the accuracy of the entire S protein and a single chain, and compare them with the scores from SWISS-MODEL and Robetta.

| SSGCID | RMSD (Full Protein) | RMSD (Single Chain)|
|:-------|:--------------------|:-------------------|
|SSGCID1|3.505|2.7843|
|SSGCID2|2.3274|2.107|
|SSGCID3|2.12|1.866|
|SSGCID4|2.0854|2.047|
|SSGCID5|4.9636|4.6443|

We can see that the SSGCID models outclass our SWISS-MODEL models for the full S protein and our Robetta models for a single chain. Interestingly, SSGCID3 modeled a more accurate chain, but SSGCID4 modeled a more accurate full protein. Given that the S protein is very large, SSGCID4 may be considered as fairly accurate. In addition, SSGCID3 is within the accepted RMSD score threshold.

#### GalaxyWEB

Finally, we used <a href="http://galaxy.seoklab.org/" target="_blank">GalaxyWEB</a> to model just the RBD of the S protein. We compared the models to the actual structure from the PDB, <a href="https://www.rcsb.org/structure/6LZG" target="_blank">6lzg</a>.

| GalaxyWEB | RMSD |
|:--------|:-----|
|Galaxy1| 0.1775|
|Galaxy2| 0.1459|
|Galaxy3| 0.1526|
|Galaxy4| 0.1434|
|Galaxy5| 0.1202|

We see that all models from GalaxyWEB are well within the RMSD score threshold and can be considered as being very accurate. These models may have performed extremely well because the RBD has a fairly small sequence length, about 229 amino acids, when compared to the S protein chain of about 1281 amino acids.

## Main Takeaway

RMSD is a useful method of calculating a quantitatively assessing the accuracy of predicted models. From our results, we saw that SSGCID models performed much better than the models produced by SWISS-MODEL and Robetta. In addition, GalaxyWEB produced highly accurate models with a much smaller sequence length. Although each web-server have different algorithms for homology modeling, the difference in performance support a positive correlation between computational power and accuracy, as well as a negative correlation between sequence length and accuracy.

We can also compare the RMSD scores between *QUARK* and GalaxyWEB. Both servers were given a small amino acid sequence to create their models: 141 amino acids for *QUARK*, and 229 amino acids for *GalaxyWEB*. However, all the *QUARK* models exceeded the RMSD threshold, while GalaxyWEB models were highly accurate. From this we can see the difference in accuracy between *ab initio* modeling and homology modeling.

Although we have yet to find nature's magic algorithm for protein folding, it is amazing how far protein structure prediction has improved over the years from the establishment of the Soviet research institution in the 1960s. The models we assessed here may not be 100% accurate, but they all do a good job on generating an approximate answer to the protein structure. As these algorithms continue to improve, the day when we solve the protein folding problem slowly approaches.

This concludes the part one of this module on how scientists can analyze new proteins before their structures are experimentally determined. In part two of the module, we will explore the differences between SARS and-SARS CoV2 S protein by using the x-ray crystallography structures of the proteins that have been publshed on the PDB during the first wave of outbreak. With this comparison, our goal is to find molecular explanations of why SARS-CoV-2 is much more infectious. 

[Next lesson](multiseq){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
