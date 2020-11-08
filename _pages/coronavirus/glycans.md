---
permalink: /coronavirus/glycans
title: "Glycans"
sidebar: 
 nav: "coronavirus"
---

The surface of viruses and host cells are not smooth, but rather “fuzzy”. This is because the surface is decorated by structures called glycans, which consists of numerous monosaccharides linked together by glycosidic bonds. Although this definition is also shared with polysaccharides, glycans typically refer to the carbohydrate portion of glycoproteins, glycolipids, or proteoglycans [^1]. Glycans have been found to have structural and modulatory properties and are crucial in recognition events, most commonly by glycan-binding proteins (GBPs) [^2]. In viral pathogenesis, glycans on host cells act as primary receptors, co-receptors, or attachment factors that are recognized by viral glycoproteins for viral attachment and entry. On the other hand, glycans on viral surfaces are key for viral recognition by the host immune system [^3]. Unfortunately, some viruses have evolved methods that allow them to effectively conceal themselves from the immune system. One such method, which is utilized by SARS-CoV-2, is a “glycan shield”, where glycosylation of surface antigens allows the virus to hide from antibody detection. Another notorious virus that utilizes glycan shielding is HIV. The Spike protein of SARS-CoV-2 was also found to be heavily glycosylated, shielding around 40% of the Spike protein from antibody recognition [^4]. Such glycosylation does not hinder the Spike protein’s ability to interact with human ACE2 because the Spike protein is able to adopt an open conformation, allowing a large portion of the RBD being exposed.

Glycans are generally very flexible and have large internal motions that makes it difficult to get an accurate description of their 3D shapes. Fortunately, molecular dynamics (MD) simulations can be employed to predict the motions and shapes of the glycans. With a combination of MD and visualization tools (i.e. VMD), snapshots of the Spike protein with its glycosylation can be created.

<img src="../_pages/coronavirus/files/Glycan_Grant.png">


Nonetheless, basic visualizations of the Spike protein with its glycans can be made using just VMD. Here, we used SARS-CoV-2 Spike in its closed conformation (<a href="https://www.rcsb.org/structure/6vxx" target="_blank">6vxx)</a>) and SARS-CoV-2 Spike in its open conformation (<a href="https://www.rcsb.org/structure/6VYB" target="_blank">6vyb</a>) to create the following images. Notice how the RBD in the orange chain is much more exposed in the open conformation. The presumed glycans are shown in red.

<img src="../_pages/coronavirus/files/GlycanComparison.png">

To see how to visualize glycans in VMD, go to the following tutorial.

[Visit tutorial](tutorial_glycans){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

<hr>

[Next lesson](conclusion){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}


##### Sources
[^1]: Dwek, R.A. Glycobiology: Toward Understanding the Function of Sugars. Chem. Rev. 96(2),  683-720 (1996). https://pubs.acs.org/doi/10.1021/cr940283b

[^2]: Varki A, Lowe JB. Biological Roles of Glycans. In: Varki A, Cummings RD, Esko JD, et al., editors. Essentials of Glycobiology. 2nd edition. Cold Spring Harbor (NY): Cold Spring Harbor Laboratory Press; 2009. Chapter 6. https://www.ncbi.nlm.nih.gov/books/NBK1897/

[^3]: Raman, R., Tharakaraman, K., Sasisekharan, V., & Sasisekharan, R. Glycan-protein interactions in viral pathogenesis. Current opinion in structural biology, 40, 153–162 (2016). https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5526076/

[^4]: Grant, O. C., Montgomery, D., Ito, K., & Woods, R. J. Analysis of the SARS-CoV-2 spike protein glycan shield: implications for immune recognition. bioRxiv : the preprint server for biology, 2020.04.07.030445. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7217288/
