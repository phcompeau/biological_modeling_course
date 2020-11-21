---
permalink: /coronavirus/tutorial_rmsd
title: "Calculating RMSD"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

In this tutorial, we will demonstrate how to apply the Kabsch algorithm to compute the RMSD between two protein structures. In particular, we will show how to compare the experimentally validated structure of the SARS-CoV spike protein against the structures that we predicted in our previous homology modeling.

In this tutorial, we will first use ProDy, our featured software resource for this module. ProDy is an open-source Python package that allows users to perform protein structural dynamics analysis. Its flexibility allows users to select specific parts or atoms of the structure for conducting normal mode analysis and structure comparison.

To get started, make sure that you have the following software resources installed.

<a href="https://www.python.org/downloads/" target="_blank">Python</a> (2.7, 3.5, or later)

<a href="http://prody.csb.pitt.edu/downloads/" target="_blank">ProDy</a>

<a href="https://numpy.org/install/" target="_blank">NumPy</a>

<a href="https://biopython.org/" target="_blank">Biopython</a>

<a href="https://ipython.org/" target="_blank">IPython</a>

<a href="https://matplotlib.org/" target="_blank">Matplotlib</a>

## Getting Started

It is recommended that you create a workspace for storing created files when using ProDy or storing protein .pdb files. Make sure you are in your workspace before starting up IPython.

~~~ python
ipython --pylab
~~~

Import functions and turn interactive mode on (only need to do this once per session).

~~~
In[#]: from pylab import *
In[#]: from prody import *
In[#]: ion()
~~~


## How to Calculate RMSD

We will be matching chains between the two structures based on sequence identity and sequence overlap. The goal is to calculate the Root Mean Square Deviation scores between the alpha-Carbons of the matched chains. This will give us a basic quantitative measure of the structural differences between the two proteins. First, we will define a function that will list out matched chains for later use.

~~~ python
In[#]: def printMatch(match):
...: print('Chain 1 : {}'.format(match[0]))
...: print('Chain 2 : {}'.format(match[1]))
...: print('Length : {}'.format(len(match[0])))
...: print('Seq identity: {}'.format(match[2]))
...: print('Seq overlap : {}'.format(match[3]))
...: print('RMSD : {}\n'.format(calcRMSD(match[0], match[1])))
...:
~~~

Next, we will parse the protein structures (in `.pdb` format) that we want to compare. To use our own protein structure, make sure that the .pdb file is in the current directory. Let's parse in one of our models we obtained from homology modeling of the SARS-CoV-2 Spike protein, SWISS1. You can use your own SARS-CoV-2 Spike protein model that you generated, or download our [SWISS-MODEL Results](../_pages/coronavirus/files/SWISS_Model.zip). In this tutorial, our model will be called `swiss1.pdb`.

~~~ python
In[#]: struct1 = parsePDB(‘swiss1’)
~~~

Because we want to find out how well `swiss1.pdb` performed, we will compare it to the determined protein structure of SARS-CoV-2 Spike protein in the Protein Data Bank. Enter the code shown below. This will prompt the console to search for `6vxx`, the SARS-CoV-2 Spike protein, from the Protein Data Bank and download the .pdb file into the current directory. Then, it will save the structure to the variable `struct2`.

~~~ python
In[#]: struct2 = parsePDB(‘6vxx’)
~~~

Including the `.pdb` tag will prompt the console to search for the file '6vxx.pdb' in the current directory and parse it. You can download .pdb files directly from PDB. If you do not have 6vxx.pdb, follow the format of the previous command.

With the protein structures parsed, we can now match chains. The default threshold for sequence identity and sequence overlap are 90%. This can be changed by specifying the desired thresholds. Here, a sequence identity threshold of 75% and an overlap threshold of 80% is specified.

~~~ python
In[#]: matches = matchChains(struct1, struct2, seqid = 75, overlap = 80)
~~~

Now, we will use our previously defined function to list out matched chains.

~~~ python
In[#]: for match in matches:
…: printMatch(match)
…:
~~~

You should see the results listed like this:

![image-center](../assets/images/RMSDResult1.png){: .align-center}

![image-center](../assets/images/RMSDResult2.png){: .align-center}

The results are stored as a 2D array called `matches`, where `matches[i][j]` represents the *i*th match and *j*th chain (zero-based). Given the previous example, `matches[0][0]` corresponds to `Chain 1 : AtomMap Chain A from 6crx -> Chain A from 6vxx` and `matches[5][1]` corresponds to `Chain 2: AtomMap Chain C from 6vxx -> Chain B from 6crx`.

Let us say we want to calculate the RMSD score between the matched `Chain B` from `swiss1` and `Chain B` from `6vxx`. This will correspond to `matches[4][0]` and `matches[4][1]`. Afterwards, We need to first transform and align the chains such that it minimizes the RMSD between the C⍺.

~~~ python
In[#]: first_ca = matches[4][0]
In[#]: second_ca = matches[4][1]
In[#]: calcTransformation(first_ca, second_ca).apply(first_ca);
~~~

Finally, we can calculate the RMSD score.

~~~ python
In[#]: calcRMSD(first_ca, second_ca)
~~~

You should see something like this:

![image-center](../assets/images/RMSDResult3.png){: .align-center}

Finally, it is also possible to merge all the chains together to calculate the RMSD of the overall structure. Here we merge the three matches corresponding to A to A, B to B, and C to C.

~~~ python
In[#]: first_ca = matches[0][0] + matches[4][0] + matches[8][0]
In[#]: second_ca = matches [0][1] + matches[4][1] + matches[8][1]
In[#]: calcTransformation(first_ca, second_ca).apply(first_ca);
In[#]: calcRMSD(first_ca, second_ca)
~~~

Your results should look like this:
![image-center](../assets/images/RMSDResult4.png){: .align-center}

Now, we will head back to the main text to see the RMSD calculations for the rest of the models we created of the SARS-COV-2 S protein.

[Return to main text](accuracy#assessing-the-accuracy-of-our-structure-prediction-models){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
