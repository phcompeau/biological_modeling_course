---
permalink: /coronavirus/tutorial_rmsd
title: "Calculating RMSD"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---
<a href="http://prody.csb.pitt.edu/" target="_blank">ProDy</a> is an open-source Python package that allows users to perform protein structural dynamics analysis. Its flexibility allows users to select specific parts or atoms of the structure for conducting normal mode analysis and structure comparison. Please be sure to have the following installed:

<a href="https://www.python.org/downloads/" target="_blank">Python</a> (2.7, 3.5, or later)

<a href="http://prody.csb.pitt.edu/downloads/" target="_blank">ProDy</a>

<a href="https://numpy.org/install/" target="_blank">NumPy</a>

<a href="https://biopython.org/" target="_blank">Biopython</a>

<a href="https://ipython.org/" target="_blank">IPython</a>

<a href="https://matplotlib.org/" target="_blank">Matplotlib</a>

### Getting Started
It is recommended that you create a workspace for storing created files when using ProDy or storing protein .pdb files. Make sure you are in your workspace before starting up IPython.
~~~ python
ipython --pylab
~~~~~

Import functions and turn interactive mode on (only need to do this once per session).
~~~ python
In[#]: from pylab import *
In[#]: from prody import *
In[#]: ion()
~~~~~


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
~~~~~

Next, we will parse the protein structures from PDB or from the current directory. *Note:* The protein structures need to be in *.pdb* format.
~~~ python
In[#]: struct1 = parsePDB(‘6crx’)
~~~~~~~
This will prompt the console to search for `6crx`, SARS Spike protein, from the Protein Data Bank, download the .pdb file into the current directory, and save it to the variable `struct1`.

~~~ python
In[#]: struct2 = parsePDB(‘6vxx.pdb’)
~~~~~
Including the .pdb tag will prompt the console to search for the file '6vxx.pdb' in the current directory and parse it. You can download .pdb files directly from PDB. If you do not have 6vxx.pdb, follow the format of the previous command.

With the protein structures parsed, we can now match chains. The default threshold for sequence identity and sequence overlap are 90%. This can be changed by specifying the desired thresholds. Here, a sequence identity threshold of 75% and an overlap threshold of 80% is specified.
~~~ python
In[#]: matches = matchChains(struct1, struct2, seqid = 75, overlap = 80)
~~~~~
Now, we will use our previously defined function to list out matched chains.
~~~ python
In[#]: for match in matches:
…: printMatch(match)
…:
~~~~~~
You should see the results listed like this:
![image-center](../assets/images/chris_RMSDResult1.png){: .align-center}

![image-center](../assets/images/chris_RMSDResult2.png){: .align-center}

The results are stored as a 2D array called `matches`, where `matches[i][j]` represents the *i*th match and *j*th chain (zero-based). Given the previous example, `matches[0][0]` corresponds to `Chain 1 : AtomMap Chain A from 6crx -> Chain A from 6vxx` and `matches[5][1]` corresponds to `Chain 2: AtomMap Chain C from 6vxx -> Chain B from 6crx`.

Let us say we want to calculate the RMSD score between the matched `Chain C` from `6crx` and `Chain A` from `6vxx`. We need to first transform and align the chains such that it minimizes the RMSD between the C⍺.
~~~ python
In[#]: first_ca = matches[6][0]
In[#]: second_ca = matches[6][1]
In[#]: calcTransformation(first_ca, second_ca).apply(first_ca);
~~~~~
Finally, we can calculate the RMSD score.
~~~ python
 In[#]: calcRMSD(first_ca, second_ca)
~~~~~~
The result should be an RMSD score of around 3.
It is also possible to merge matches to calculate the RMSD of the overall structure. In this example, both 6crx and 6vxx are each made up of three chains. Here we merge the three matches corresponding to A to A, B to B, and C to C.
~~~ python
In[#]: first_ca = matches[0][0] + matches[4][0] + matches[8][0]
In[#]: second_ca = matches [0][1] + matches[4][1] + matches[8][1]
In[#]: calcTransformation(first_ca, second_ca).apply(first_ca);
In[#]: calcRMSD(first_ca, second_ca)
~~~~~~
The result should be an RMSD score of around 11.

Now, let's head back to the main text to see how we use RMSD to assess the accuracy of the models we created of the SARS-COV-2 S protein.

[Return to main text](accuracy){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
