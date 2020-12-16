---
permalink: /coronavirus/exercises
title: "Exercises"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

* Good exercise later: compute Q scores for the protein structure comparison that we performed at the end of part 1.

* Why are contact maps and cross correlation maps "symmetric" about the main diagonal?

* Something on identifying a dynamics difference from a contact map or better cross-correlation in similar proteins.

* If you have not already done so, try modeling the SARS-CoV-2 S protein or RBD using SWISS-MODEL, Robetta, or GalaxyWEB using the steps in<a href="tutorial_homology" target="blank">Homology Structure Prediction Tutorial</a>. Then, use ProDy to calculate the RMSD between your models and the PDB entries <a href="http://www.rcsb.org/structure/6VXX" target="blank">6vxx</a> for the S protein and <a href="http://www.rcsb.org/structure/6LZG" target="blank">6lzg</a> for the RBD. Did your models perform better than our models?

* Visualize your best performing model(s) and the corresponding PDB entryl in VMD. If the models are sufficiently similar, try performing a structural alignment using Multiseq and see where in the the structure your predicted models did well.

* Using VMD, model the SARS-CoV-2 S (<a href="http://www.rcsb.org/structure/6VXX" target="blank">6vxx</a>) protein and SARS S (<a href="https://www.rcsb.org/structure/5X58" target="blank">5x58</a>) protein. Create the graphical representation of glycans and compare the number of glycans between the two proteins. Are they any different? Could this possibly be another reason why SARS-CoV-2 is more infectious than SARS?

* In our GNM tutorial, we created the contact map using the threshold of 20Å. Try making the contact map of one of the chains of SARS-CoV-2 S protein [6vxx](http://www.rcsb.org/structure/6VXX) with different thresholds. Do the maps look different?

* In this module, we only used homology modeling for large molecules such as the SARS-CoV-2 S protein and the RBD. It would be interesting to directly compare the accuracy of homology modeling and *ab initio* modeling. Try using one of the three homology modeling softwares to predict the structure of the human hemoglobin subunit ([sequence](../_pages/coronavirus/files/Human_Hemoglobin_subunit_alpha_Seq.txt)). After you get your predicted models, try calculating the RMSD using the PDB entry [1si4](https://www.rcsb.org/structure/1sI4). How do they compare to the RMSD from our *ab inito* (QUARK) models?
