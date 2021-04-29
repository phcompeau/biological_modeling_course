---
permalink: /contents/
title: "Contents"
classes: wide
---

This online course is divided into modules. Each module covers a single biological topic (e.g., "Analyzing the structure of the coronavirus spike protein") and introduces all of the modeling topics needed to address that topic from first principles.

Each module has a main narrative that can be explored by anyone, including beginners. When we need to build a model along the way, we pass our modeling work to "software tutorials" that show how to use high-powered modeling software produced by [MMBioS](http://mmbios.pitt.edu) researchers in order to build biological models. The software tutorials allow users wishing to get their hands dirty with modeling software to build all of the models that we need in this course. This allows for a course that can be explored by both casual and serious biological modeling learners alike.

After building a model in a software tutorial, we return to the main text and analyze the results of this model. In this way, the text forms a constant interplay between establishing a biological problem, describing how a model work, implementing that model in a software tutorial, and returning to the text to analyze the model and ask our next question, beginning the cycle anew.

The following contents is a work in progress and will be expanded in the coming weeks. For now, you can find links to the start of each published module below.

## [Prologue: An introduction to biological modeling via random walks and Turing patterns](../prologue/) ##

### Main text

* Introduction: Life is random

* Alan Turing and the zebra's stripes

* An introduction to random walks

* A reaction-diffusion model generating Turing patterns

* The Gray-Scott model: a cellular automaton generating Turing patterns

* Conclusion: Turing patterns are fine-tuned

### Software tutorials (featuring MCell and CellBlender)

* Simulating particle diffusion with CellBlender

* Generating Turing patterns with a reaction-diffusion simulation in CellBlender

* Building a diffusion cellular automaton with Jupyter notebook

* Implementing the Gray-Scott model for coarse-grained reaction-diffusion with Jupyter notebook


## [Module 1: Finding motifs in transcription factor networks](../motifs/home) ##

### Main text

* Introduction: a submicroscopic scale of interest

* Transcription and DNA-protein binding

* Transcription factor networks

* Using randomness to verify network motifs

* The negative autoregulation motif

* The feedforward loop motif

* Building a biological oscillator

* Conclusion: the importance of robustness in biological oscillations

### Software tutorials (featuring MCell and CellBlender)

* Looking for loops in transcription factor networks

* Comparing simple regulation to negative autoregulation

* Ensuring a mathematically controlled simulation for comparing simple regulation to negative autoregulation

* Implementing the feed-forward loop motif

* Implementing the repressilator: a biological oscillator

* Perturbing the repressilator

## [Module 2: Unpacking E. coli's genius exploration algorithm](../chemotaxis/home) ##

### Main text

* Introduction: Lost Immortals and chemotaxis

* Bacterial runs and tumbles

* Signaling and ligand-receptor dynamics

* Stochastic simulation of chemical reactions

* A biochemically accurate model of chemotaxis

* Methylation helps bacteria adapt to differing concentrations

* Modeling a bacterium's response to an attractant gradient

* Conclusion: the beauty of E. coli's robust randomized exploration algorithm

### Software tutorials (featuring BioNetGen)

* Getting started wtih BioNetGen and modeling ligand-receptor dynamics

* Adding phosphorylation to our BioNetGen model

* Modeling adaptation

* Traveling up an attractant gradient

* Traveling down an attractant gradient

* Modeling a pure random walk strategy

* Modeling E. coli's sophisticated random walk algorithm

* Comparing different chemotaxis default tumbling frequencies

## [Module 3: Analyzing the coronavirus spike protein](../coronavirus/home) ##

### Part 1: Protein structure prediction

* An introduction to protein structure prediction

* *Ab initio* protein structure prediction

* Homology modeling for protein structure prediction

* Comparing protein structures to assess model accuracy

* Part 1 conclusion: protein structure prediction is solved! (Kinda...)

### Part 2: Comparing SARS-CoV-2 and SARS-CoV

* Searching for local differences in the SARS-CoV and SARS-CoV-2 spike proteins

* Analyzing structural differences in the bonding of SARS-CoV and SARS-CoV-2 with the ACE2 enzyme

* Quantifying the interaction energy between the SARS-CoV-2 spike protein and ACE2

* Part 2 conclusion: from static protein analysis to molecular dynamics

### Software tutorials (featuring ProDy)

* Using *ab initio* modeling to predict the structure of hemoglobin subunit alpha

* Using homology modeling to predict the structure of the SARS-CoV-2 spike protein

* Using RMSD to compare the predicted SARS-CoV-2 spike protein against its experimentally validated structure

* Finding local differences in the SARS-CoV and SARS-CoV-2 spike protein structures

* Visualizing specific regions of interest within the spike protein structure

* Computing the energy contributed by a local region of the SARS-CoV-2 spike protein bound with the human ACE2 enzyme

* Molecular dynamics analysis of coronavirus spike proteins using GNM

* Adding directionality to GNM simulations using ANM

## Module 4: Training a computer to count white blood cells automatically ##

Coming soon!

Featured software: [CellOrganizer](http://www.cellorganizer.org)
