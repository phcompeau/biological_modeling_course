---
permalink: /coronavirus/accuracy
title: "Comparing Protein Structures to Assess Model Accuracy"
sidebar:
 nav: "coronavirus"
toc: true
toc_sticky: true
---

## Protein structure comparison is related to shape comparison

In the previous lesson, we saw how to predict the structure of a protein from its sequence (as well as a library of known structures). We then used homology modeling to predict a structure for the SARS-CoV-2 spike protein using three different algorithms. In this lesson, we will discuss how to compare these predicted structures against each other as well as against the now verified structure of the SARS-CoV-2 spike protein.

Ultimately, the problem of comparing protein structures is intrinsically similar to the comparison of two shapes, a problem that we will discuss first. Consider the two shapes in the figure below. You may be able to see that these two shapes are the same, but training a computer to recognize that one shape has been flipped and rotated to yield the other is a nontrivial task. We are able to perform this task well because we have very highly evolved eyes that help us quickly cluster and classify the objects that we see in the world.

![image-center](../assets/images/two_shapes.png){: .align-center}
The red shape can be flipped and then rotated to yield the blue shape. Although you may be able to see this correspondence, it will be difficult to identify whether two shapes are the same if they become much more complicated.
{: style="font-size: medium;"}

## Comparing two shapes with the Kabsch algorithm

Our goal is to produce a distance function *d*(*S*, *T*) that takes two shapes *S* and *T* as input and that quantifies how different these shapes are. If the two shapes are the same, then the distance between them should be equal to zero; the more different the shapes become, the larger *d* should become.

To demonstrate that the two shapes in the preceding figure were the same, we would need to first move the red shape to superimpose it over the blue shape, then flip the red shape, and finally rotate it so that its boundary coincides with the blue shape.

![image-center](../assets/images/shape_transformations.gif){: .align-center}
We can transform the red shape into the blue shape by translating it, flipping it, and then rotating it.
{: style="font-size: medium;"}

More generally, *S* is the same shape as *T*, meaning that *d*(*S*, *T*) is equal to zero, if *S* can be translated, flipped, and/or rotated to produce *T*. The question is what to do if *S* and *T* are not the same to produce our desired distance function *d*(*S*, *T*).

Our idea is to translate, flip, and rotate *S* so that the resulting transformed shape resembles *T* as much as possible. We then define *d*(*S*, *T*) by how much this transformed shape differs from *T*.

To this end, we first translate *S* to have the same **center of mass** as *T*. This is not a particularly difficult problem because the center of mass of *S* is found at the point (*x*<sub><em>S</em></sub>, *y*<sub><em>S</em></sub>) such that *x*<sub><em>S</em></sub> is the average of *x*-coordinates on the boundary of *S* and *y*<sub><em>S</em></sub> is the average of *y*-coordinates on the boundary.

We can estimate the center of mass of *S* by sampling *n* points from the boundary of the shape and taking the point whose coordinates are the average of the *x* and *y* coordinates of points on the boundary. We then superimpose *S* and *T* by translating *S* so that its center of mass coincides with that of *T*.

Next, we want to find the rotation of *S*, possibly along with a flip as well, that makes the shape resemble *T* as much as possible. Imagine first that we have found this rotation; we can then define *d*(*S*, *T*) in the following way. We sample *n* equally spaced points along the boundary of each shape, meaning that *S* and *T* are converted into **vectors** *s* = (*s*<sub>1</sub>, ..., *s*<sub><em>n</em></sub>) and *t* = (*t*<sub>1</sub>, ..., *t*<sub><em>n</em></sub>), where *s*<sub><em>i</em></sub> is the *i*-th point on the boundary of *S*. We then compute the **root mean square deviation (RMSD)** between the two shapes, which is the square root of the average squared distance between corresponding points in the vectors.

$$\text{RMSD}(s, t) = \sqrt{\dfrac{1}{n} \cdot (d(s_1, t_1)^2 + d(s_2, t_2)^2 + \cdots + d(s_n, t_n)^2)} $$

In this formula, *d*(*s*<sub><em>i</em></sub>, *t*<sub><em>i</em></sub>) is the distance between the points *s*<sub><em>i</em></sub> and *t*<sub><em>i</em></sub> in 2-D or 3-D space as the case may be. (Note: root mean square deviation is a commonly used approach when measuring pairwise differences between two vectors.)

**NEED EXAMPLE OF RMSD -- CHRIS BELOW, WILL NEED TO CLEAN UP**

Below is a simple example of how RMSD is calculated.

![image-center](../assets/images/SampleRMSD.png){: .align-center}
Simple example of calculating RMSD between two paired sets of 3D-coordinates. The pairs are circles in the plot.
{: style="font-size: medium;"}

**RESUME**

However, all this has left open the fact that we assumed that we had rotated *S* to be as "similar" to *T* as possible. In practice, we will need to find the rotation of *S* that *minimizes* the RMSD between our vectorizations of *S* and *T*, and this resulting minimum will be what we consider *d*(*S*, *T*). It turns out that there is an approach to find this rotation called the **Kabsch algoithm**, which requires some advanced linear algebra and is beyond the scope of our work but can be read about <a href="https://en.wikipedia.org/wiki/Kabsch_algorithm" target="_blank">here</a>.

**STOP:** Do you see any potential pitfalls with our use of RMSD to determine how much two shapes differ?
{: .notice--primary}

NEED A QUESTION ABOUT N BEING TOO LOW. SQUARE vs. SQUARE and SQUARE Vs CIRCLE

## Applying the Kabsch algorithm to protein structure comparison

The Kabsch algorithm offers a compelling way to determine the similarity of two protein structures. We can convert a protein containing *n* amino acids into a vector of length *n* by selecting a single representative point from each amino acid. To do so, scientists typically choose the "alpha carbon", the amino acid's centrally located carbon atom that lies on the peptide's backbone; note that the position of this atom will already be present in the `.pdb` file for a given structure.

**STOP:** Can you think of example where a small difference between protein structures can cause a large inflation in RMSD score?
{: .notice--primary}

Applying the Kabsch algorithm to find the translation and rotation of a given protein structure that minimizes RMSD compared to another structure is a reasonable idea that may work well in practice. Yet there is no perfect metric for shape comparison, and the Kabsch algorithm is no different.

To take one example of how the Kabsch algorithm may be flawed, consider the figure below showing two toy protein structures. The orange structure (*S*) is identical to the blue structure (*T*) except for the change in a single bond angle between the third and fourth amino acids. And yet this tiny change in the protein's structure means that the computation of *d*(*s*<sub><em>i</em></sub>, *t*<sub><em>i</em></sub>)<sup>2</sup> for every *i* greater than 3 winds up being significant, increasing the RMSD significantly.

![image-center](../assets/images/RMSD_weakness_mutation.png){: .align-center}
(Top) Two hypothetical protein structures that differ in only a single bond angle between the third and fourth amino acids, shown in red. Each circle represents an alpha carbon. (Bottom left) Overlaying the first three amino acids shows how much the change in the bond angle throws off the computation of RMSD by increasing the distances between corresponding alpha carbons. (Bottom right) The Kabsch algorithm would align the centers of gravity of the two structures in order to minimize RMSD between corresponding alpha carbons. This makes it difficult for the untrained observer to notice that the two proteins only really differ in a single bond angle.
{: style="font-size: medium;"}

Another way in which the Kabsch algorithm could be fooled is in the case of a substructure that is appended to the side of a structure and that throws off the ordering of the amino acids. For example, consider the following toy example of a structure into which we incorporate a loop.

![image-center](../assets/images/RMSD_weakness_loop.png){: .align-center}
A simplification of two protein structures, one of which includes a loop of three amino acids. After the loop, each amino acid in the orange structure will be compared against an amino acid that occurs farther long in the blue structure, thus increasing *d*(*s*<sub><em>i</em></sub>, *t*<sub><em>i</em></sub>)<sup>2</sup> for each such amino acid.
{: style="font-size: medium;"}

These potential drawbacks of RMSD mean that we need to combine it with additional methods of structure comparison in many practical applications. Nonetheless, if we apply the Kabsch algorithm and get a *small* value of RMSD (e.g., just a few angstroms), then we can have some confidence that the proteins are indeed similar.

In the following tutorial, we will walk through how to apply the Kabsch algorithm to two protein structures in `.pdb` format.

[Visit tutorial](tutorial_rmsd){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Assessing the accuracy of our structure prediction models

In the tutorials occurring earlier in this module, we used publicly available protein structure prediction servers to predict the structure of human hemoglobin subunit alpha (using *ab initio* modeling) and the SARS-CoV-2 spike protein (using homology modeling).

Let's see how well the models performed by showing the values of RMSD produced by the Kabsch algorithm when comparing each of our models against these validated structures.

### *Ab initio* (QUARK) models of Human Hemoglobin Subunit Alpha

In the [ab initio tutorial](tutorial_ab_initio), we used *<a href="https://zhanglab.ccmb.med.umich.edu/QUARK/" target="_blank">QUARK</a>* to perform *ab initio* structure prediction of human hemoglobin subunit alpha from its amino acid sequence, producing five models. In the following table, we show the RMSD produced by the Kabsch algorithm for each of these models against the validated structure of this subunit (PDB: <a href="https://www.rcsb.org/structure/1SI4" target="_blank">1si4</a>).

| Quark Model | RMSD  |
|:------------|:------|
| QUARK1      | 1.58  |
| QUARK2      | 2.0988|
| QUARK3      | 3.11  |
| QUARK4      | 1.9343|
| QUARK5      | 2.6495|

It is tempting to conclude that our *ab initio* prediction was a success. However, because human hemoglobin subunit alpha is such a short protein (141 amino acids), this score would be considered high. We know that homology modeling will be faster than *ab initio* modeling. But will it be more accurate as well?

### Homology models of SARS-CoV-2 S protein

In the [homology tutorial](tutorial_homology), we used SWISS-MODEL and Robetta to predict the structure of the SARS-CoV-2 spike protein, and we used GalaxyWeb to predict the structure of this protein's receptor binding domain (RBD). In addition to our predicted models, we will also assess five predicted models of the full SARS-CoV-2 spike protein released early in the COVID-19 pandemic by <a href="https://boinc.bakerlab.org/" target="_blank">Rosetta@Home</a> and published to the Seattle Structural Genomics Center for Infectious Disease (SSGCID). Because the work needed to generate these models was distributed over many users' machines, comparing the RMSD scores obtained by the Rosetta@Home models against our own may provide insights on the effect of computational power on the accuracy of predictions. The SSGCID models can be found <a href="https://www.ssgcid.org/cttdb/molecularmodel_list/?target__icontains=BewuA" target="_blank">here</a>.

#### GalaxyWEB

First, we consider the <a href="http://galaxy.seoklab.org/" target="_blank">GalaxyWEB</a> models we produced of the spike protein RBD. We compared these models to the validated SARS-CoV-2 RBD (PDB entry: <a href="https://www.rcsb.org/structure/6LZG" target="_blank">6lzg</a>).

| GalaxyWEB | RMSD |
|:--------|:-----|
|Galaxy1| 0.1775|
|Galaxy2| 0.1459|
|Galaxy3| 0.1526|
|Galaxy4| 0.1434|
|Galaxy5| 0.1202|

All of these models have an excellent RMSD score and can be considered as very accurate. Note that their RMSD is more than an order of magnitude lower than the RMSD computed for our *ab initio* model of hemoglobin subunit alpha, despite the fact that the RBD is longer than this protein at 229 amino acids.

#### SWISS-MODEL

We now shift to homology models of the entire spike protein and start with <a href="https://swissmodel.expasy.org/" target="_blank">SWISS-MODEL</a>. We compared each model produced by SWISS-MODEL against to the validated structure of the SARS-CoV-2 spike protein (PDB entry: <a href="https://www.rcsb.org/structure/6vxx">6vxx</a>).

| SWISS MODEL | RMSD |
|:------------|:-----|
|SWISS1| 5.8518|
|SWISS2| 11.3432|
|SWISS3| 11.3432|

From the scores, we can see that model SWISS1 performed the best. Even though the RMSD score of 5.818 is significantly higher than what we saw for the GalaxyWEB prediction for the RBD, keep in mind that the spike protein is 1281 amino acids long, and so the sensitivity of RMSD to slight changes should give us confidence that our models are on the right track.

#### Robetta

<a href="https://robetta.bakerlab.org/" target="_blank">Robetta</a> produced five models of a single chain of the SARS-CoV-2 spike protein. As with the models produced by SWISS-MODEL, we compared each of them against the validated structure of the SARS-CoV-2 spike protein (PDB: <a href="https://www.rcsb.org/structure/6vxx">6vxx</a>).

| Robetta | RMSD |
|:--------|:-----|
|Robetta1| 3.1189|
|Robetta2| 3.7568|
|Robetta3| 2.9972|
|Robetta4| 2.5852|
|Robetta5| 12.0975|

Most of the Robetta models for a single chain beat the SWISS-Model predictions for the entire protein. This makes it difficult to say at the moment which resource has performed better.

#### SSGCID

As explained above, the SSGCID models of the S protein released by <a href="https://boinc.bakerlab.org/" target="_blank">Rosetta@Home</a> used large amounts of computational power. Therefore, we might to see RMSD scores lower than those of our models. Like before, we will compare the models to the validated structure of (PDB: <a href="https://www.rcsb.org/structure/6vxx">6vxx</a>). This time, we will assess both the accuracy of the Rosetta@Home predictions of the entire spike protein as well as a single chain.

| SSGCID | RMSD (Full Protein) | RMSD (Single Chain)|
|:-------|:--------------------|:-------------------|
|SSGCID1|3.505|2.7843|
|SSGCID2|2.3274|2.107|
|SSGCID3|2.12|1.866|
|SSGCID4|2.0854|2.047|
|SSGCID5|4.9636|4.6443|

**STOP:** Consider the following three questions.<br><br>
First, note that SSGCID3 modeled a single chain more accurately, but SSGCID4 modeled a more accurate full protein. What do you think might have caused this?<br><br>
Second, why do you think that the full protein RMSD values are so close to the single chain values?<br><br>
Finally, which do you think performed more accurately on our predictions: SWISS-MODEL or Robetta?
{: .notice--primary}

As we might expect due to their access to the resources of thousands of users' computers, the SSGCID models outperform our SWISS-MODEL models. But it is also worth noting that even with such a significant amount of computation, their RMSD values are not as close to zero as we might expect. Protein structure prediction remains a difficult problem.

## Part 1 conclusion: SARS-CoV-2 protein structure prediction and open science

The models we assessed here may not be 100% accurate, but they all do a good job of approximating protein structure when this structure is not yet known experimentally. We have yet to decipher nature's magic algorithm for protein folding, and yet we can retain hope that continued improvements to our algorithms and ever-increasing computational resources will allow us to one day say, "good enough!".

We also hope that this discussion has provided perspective on how a problem in biological modeling can be easy to state and very difficult to solve. This is in sharp contrast to our work in previous modules, where we saw biological models that more or less quickly solved the problems presented to them. After all, the Soviets founded an entire [research insitute](https://www.protres.ru) dedicated to protein research in 1967. Most of the scientists who were there for its grand opening are dead now, and yet the institute carries on.

Finally, we would point out that although scientific research five decades ago was, like the Soviet protein institute, siloed away from the public, the COVID-19 pandemic offers an excellent example of how citizens around the world can follow and even get involved in real research.

For example, the [GISAID](https://www.gisaid.org) organization published their first publicly available SARS-CoV-2 genome on December 24, 2019. Within six months, this database had grown to contain over 50,000 entries. At any point in early 2020, anyone could have grabbed their favorite SARS-CoV-2 genome, excised the sequence of the spike protein, and used one of a variety of different software resources to predict its structure. Alternatively, a more communally minded person could have enlisted their home machine as part of a global race to provide vaccine developers with accurate estimations of the protein's structure. Despite 2020 being a time of international crisis, the progress we have made in opening scientific research to the public is cause for optimism.

Thus ends part 1 of this module. But there is still much for us to discuss. We hope that you will join us for part 2, in which we will delve into the differences between the spike proteins of SARS-CoV-1 and SARS-CoV-2 using the validated protein structures published to PDB early in the pandemic. Can we use modeling and computation to determine why SARS-CoV-2 has been so much more infectious? We hope that you will join us to find out.

[Next lesson](multiseq){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Extra

* Badly need to mention the published version of the spike protein
