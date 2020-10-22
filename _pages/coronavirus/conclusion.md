---
permalink: /coronavirus/conclusion
title: "Conclusion"
sidebar: 
 nav: "coronavirus"
---

## Overview
* Organisms come in all shapes and sizes, but at the end of the day, we are just a highly organized group of cells. In some cases, an organism can even be just a single cell. Cells are defined as the basic functional unit of life. But behind the scenes, it is proteins that are responsible for virtually all the functions and mechanisms of the cell. In a sense, we are living in a world governed by proteins.

* When the COVID-19 outbreak emerged, scientists across the world scrambled to study the virus in order to contain the disease and produce a vaccine. This first step is to sequence its genome to identify what type of virus is causing the disease, but if we want to find the cure, we need to understand the pathogenic mechanisms that allow the virus to infect human cells. Comparing the genomes, we discovered that the virus is related to the SARS virus, but somehow much more infectious. From previous research on SARS and experimental results on SARS-CoV-2, we learned that the virus utilizes the spike (S) protein on its surface to bind to the human cells as its method of infection. As such, much of the SARS-CoV-2 research went into studying the S protein as a potential vaccine target and to find out why SARS-CoV-2 is more infectious that SARS.

* In this module, we discussed the importance of the 3D (tertiary) structure of the protein and the many experimental methods of determining protein structure. Unfortunately, these methods requires time to collect physical samples, to run the complicated and expensive experiment, and to computationally store it into the protein data base. We then went into the complex problem of protein structure prediction, and the two main approaches of obtaining the 3D structure from the amino acid sequence of the protein, *ab initio* and homology modeling. Perhaps due to the severity of the outbreak and global contributions, studies after studies on SARS-CoV-2 were released along with experimentally determined 3D structures of the SARS-CoV-2 S protein. 

* With the 3D structures available, we used several protein analysis tools to compare the SARS-CoV-2 S protein with the SARS S protein and visualize the results. We learned about three subtle structural differences within the receptor binding domain (RBD) of the S proteins that appear to increase the binding affinity of the SARS-CoV-2 S protein and ACE2, which may be one of the reasons why SARS-CoV-2 is more infectious.

* Unfortunately, biology is extremely complex. There is so much more to the story than just the protein structure and binding affinity of the S protein. We need to consider things like what happens after the S protein binds to ACE2, how does the virus enter the cell, how does it replicate itself, how does it combat our immune systems. As our last lesson, we will explore how SARS-CoV-2 hides from our immune system.

## Glycans

The surface of viruses and host cells are not smooth, but rather “fuzzy”. This is because the surface is decorated by structures called glycans, which consists of numerous monosaccharides linked together by glycosidic bonds. Although this definition is also shared with polysaccharides, glycans typically refer to the carbohydrate portion of glycoproteins, glycolipids, or proteoglycans [^1]. Glycans have been found to have structural and modulatory properties and are crucial in recognition events, most commonly by glycan-binding proteins (GBPs) [^2]. In viral pathogenesis, glycans on host cells act as primary receptors, co-receptors, or attachment factors that are recognized by viral glycoproteins for viral attachment and entry. On the other hand, the immune system can recognize foreign glycans on viral surfaces and target the virus [^3]. Unfortunately, some viruses have evolved methods that allow them to effectively conceal themselves from the immune system. One such method is a **glycan shield**. By covering the viral surface and proteins with glycans, the virus can physically shield itself from antibody detection. Because the virus replicates by hijacking the host cells, the glycan shield can consist of host glycans and mimic the surface of a host cell. A notorious virus that utilizes glycan shielding is HIV. In the case of SARS-CoV-2, the immune system recognizes the virus through specific areas, or antigens, along the S protein. The S protein, however, is a glycoprotein, meaning that it is covered with glycans which can shield the S protein antigens from being recognized. 

In our last tutorial, we will use VMD to try to visualize the glycans of SARS-CoV-2 S protein.

[Visit tutorial](tutorial_glycans){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

From the visualization we created in the tutorial, we can see that glycans are present all around the S protein. In fact, the glycans cover around 40% of the Spike protein[^4]! This raises an important question: If the glycans on the S protein can hide from antibodies, won't it get in the way of binding with ACE2? Such glycosylation does not hinder the Spike protein’s ability to interact with human ACE2 because the Spike protein is able to adopt an open conformation, allowing a large portion of the RBD being exposed. In the figure below, we compared the SARS-CoV-2 Spike in its closed conformation (PDB entry: <a href="https://www.rcsb.org/structure/6vxx" target="_blank">6vxx)</a>) and SARS-CoV-2 Spike in its open conformation (PDB entry: <a href="https://www.rcsb.org/structure/6VYB" target="_blank">6vyb</a>). The presumed glycans are shown in red. Notice how the RBD in the orange chain is much more exposed in the open conformation. 

<img src="../_pages/coronavirus/files/GlycanComparison.png">

Glycans are generally very flexible and have large internal motions that makes it difficult to get an accurate description of their 3D shapes. Fortunately, molecular dynamics (MD) simulations can be employed to predict the motions and shapes of the glycans. With a combination of MD and visualization tools (i.e. VMD), very nice looking snapshots of the glycans on the S protein can be created.

<img src="../_pages/coronavirus/files/Glycan_Grant.png">

## SARS-CoV-2 Vaccine

Much of vaccine development for SARS-CoV-2 has been focused on the S protein given how it facillitates the viral entry into host cells. In vaccine development, it is critical to understand every strategy that the virus employs to evade immune response. As we have discussed, SARS-CoV-2 hides its S protein from antibody recognition through glycosylation, creating a glycan shield around the protein. In fact, the "stalk" of the S protein has been found to be completely shielded from antibodies and other large molecules. In contrast, the "head" of the S protein is vulnerable because of the RBD is less glycosylated and becomes fully exposed in the open conformation. Thus, there is an opportunity to design small molecules that target the head of the protein [^Casalino]. Glycan profiling of SARS-CoV-2 is extremely important in guiding vaccine development as well as improving COVID-19 antigen testing [^Watanabe].

<hr>

This concludes the final lesson of the third module. If you would like to learn more about COVID-19 and SARS-CoV-2, check out this free online course: *<a href="https://sites.google.com/view/sarswars/home" target="_blank">SARS Wars: A New Hope</a>* by <a href="https://www.cs.cmu.edu/~cjl/" target="_blank">Christopher James Langmead</a>.


[Exercises](exercises){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

[^1]: Dwek, R.A. Glycobiology: Toward Understanding the Function of Sugars. Chem. Rev. 96(2),  683-720 (1996). https://doi.org/10.1021/cr940283b

[^2]: Varki A, Lowe JB. Biological Roles of Glycans. In: Varki A, Cummings RD, Esko JD, et al., editors. Essentials of Glycobiology. 2nd edition. Cold Spring Harbor (NY): Cold Spring Harbor Laboratory Press; 2009. Chapter 6. https://www.ncbi.nlm.nih.gov/books/NBK1897/

[^3]: Raman, R., Tharakaraman, K., Sasisekharan, V., & Sasisekharan, R. 2016. Glycan-protein interactions in viral pathogenesis. Current opinion in structural biology, 40, 153–162. https://doi.org/10.1016/j.sbi.2016.10.003

[^4]: Grant, O. C., Montgomery, D., Ito, K., & Woods, R. J. 2020. Analysis of the SARS-CoV-2 spike protein glycan shield: implications for immune recognition. bioRxiv. https://doi.org/10.1101/2020.04.07.030445

[^Casalino]: Casalino, L., Gaieb, Z., Dommer, A. C., Harbison, A. M., Fogarty, C. A., Barros, E. P., Taylor, B. C., Fadda, E., & Amaro, R. E. 2020. Shielding and Beyond: The Roles of Glycans in SARS-CoV-2 Spike Protein. bioRxiv : the preprint server for biology, 2020.06.11.146522. https://doi.org/10.1101/2020.06.11.146522

[^Watanabe]: Watanabe, Y., Allen, J., Wrapp, D., McLellan, J., Crispin, M. Site-specific glycan analysis of the SARS-CoV-2 spike. Science 369, 330-333. https://doi.org/10.1126/science.abb9983







