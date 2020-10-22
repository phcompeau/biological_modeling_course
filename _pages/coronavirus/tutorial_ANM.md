---
permalink: /coronavirus/tutorial_ANM
title: "ANM Calculations & VMD Plugin: NMWiz"
sidebar: 
 nav: "coronavirus"
toc: true
toc_sticky: true
---

In this tutorial, we will use Normal Mode Wizard (NMWiz), a plugin in VMD that is designed to be a GUI for ProDy, to perform ANM analysis on the SARS-CoV-2 RBD. We will visualize the results in a cross-correlation map and square fluctuation plot and then produce ANM animations showing the predicted range of motion of the SARS-CoV-2 Spike RBD. Be sure to have installed VMD and know how to load molecules into the program. If you need a refresher, go to the <a href="tutorial_multiseq" target="_blank">VMD and Multiseq Tutorial</a>.

First, load <a href="https://www.rcsb.org/structure/6vw1" target="_blank">6vw1</a> into VMD by following the steps in the previous section *Loading Molecules*. Then, start up NMWiz by going to *Extensions>Analysis>Normal Mode Wizard*.

<img src="../_pages/coronavirus/files/ANMTutorial/Image1.png">

A small window will open. Select *ProDy Interface*

<img src="../_pages/coronavirus/files/ANMTutorial/Image2.png">

We want to focus only on the RBD of SARS-CoV-2, so we need to choose a new selection. In the *ProDy Interface*, change *Selection* to "protein and chain F" and click *Select*. Next, make sure that *ANM calculation* is selected for *ProDy job:*. Check the box for *write and load cross-correlations heatmap*. Finally click *Submit Job*.

<img src="../_pages/coronavirus/files/ANMTutorial/Image3.png">

*IMPORTANT*: Let the program run and do not click on any of the VMD windows as clicking on windows may cause the program to crash or become unresponsive. The job can take from a few seconds to a couple minutes. When the job is completed, you will see a new window *NMWiz - 6vw1_anm ...* and the cross=correlation heatmap appear.

<img src="../_pages/coronavirus/files/ANMTutorial/Image4.png">

<img src="../_pages/coronavirus/files/ANMTutorial/Image5.png">

Now that the ANM calculations are completed, you will see the visualization displayed in *VMD Main*. Disable the visualization of the original visualization of *6vw1* by double-clicking on the letter 'D'. The color red will represent that it is disabled.

<img src="../_pages/coronavirus/files/ANMTutorial/Image6.png">

In *OpenGL Display*, you will be able to see the protein with numerous arrows that represents the calculated fluctuations.

<img src="../_pages/coronavirus/files/ANMTutorial/Image7.png">

To actually see the protein move as described by the arrows, we have to create the animation. Go back to the *NMWiz - 6vw1_anm...* window and click *Make* next to *Animations*.

<img src="../_pages/coronavirus/files/ANMTutorial/Image8.png">

*VMD Main* should now display a new row for the animation.

<img src="../_pages/coronavirus/files/ANMTutorial/Image9.png">

The animation should also be visible in *OpenGL Display*. However, the previous visualizations are somewhat in the way. We can disable them in the same way as before by double-clicking on the letter 'D'.

<img src="../_pages/coronavirus/files/ANMTutorial/Image10.png">

Now, you should be able to clearly see the animation of the ANM fluctuations of 6vw1.

<video width="640" height="480" controls>
<source type="video/mp4" src="../_pages/coronavirus/files/ANMTutorial/6vw1_chainF.mp4">
</video>

Now let's go back to the main text to interpret the results.

[Return to main text](NMA){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

