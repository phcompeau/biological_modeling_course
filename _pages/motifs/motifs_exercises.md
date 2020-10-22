---
permalink: /motifs/exercises
title: "Exercises for Exploring Motifs"
sidebar:
 nav: "motifs"
toc: true
toc_sticky: true
---


## Exercises

1. Modify the Jupyter notebook provided in the [tutorial on loops](tutorial_loops) to count the number of feed-forward loops in the transcription factor network for *E. coli.*

2. How many feedforward loops that you found in the preceding exercise are incoherent type-1 feed-forward loops?

3. (Challenge exercise) How many feed-forward loops would you expect to see in a random network having the same number of nodes as the *E. coli* transcription factor network? How does this compare to your answers to the previous two questions?

4. There are eight types of feed-forward loops based on the eight different ways in which we can label the edges in the network with a "+" or a "-" based on upregulation or downregulation. Modify the Jupyter notebook to count the number of loops of each type present in the *E. coli* transcription factor network.

![image-center](../assets/images/ffl_types.png){: .align-center}
The eight types of feed-forward loops.[^ffl]
{: style="text-align: center;"}

5. More complex motifs may require more computational power to discover. Can you modify the Jupyter Notebook to identify circular loops of transcription factor regulation, such as the multi-component loop below?

![image-center](../assets/images/s_cerevisiae_tf_network.jpg){: .align-center}
Example of different motifs within the *S. Cerevisiae* network[^scNetwork]
{: style="text-align: center;"}

6. Using the *NAR_comparison_equal.blend* file from the NAR Tutorial, increase the reaction rate of X1 -> X1 + Y1 to 4e4, so the table should now look like:

| Reactants |Products|Forward Rate|
|:--------|:-------:|--------:|
| X1’  | X1’ + Y1’ | 4e5 |
| X2’  | X2’ + Y2’ | 4e2 |
| Y1’  | NULL | 4e2 |
| Y2’  | NULL | 4e2 |
|Y2’ + Y2’|Y2’|4e2|

If we plot this graph, we can see the steady states of Y1 and Y2 are different once again. Can you repair the system to find the appropriate reaction rate for X2 -> X2 + Y2 to make the steady states equal once more? Are you able to adjust the reaction Y2 + Y2 -> Y2 as well? Do the reaction rates scale at the same rate?

## Internal notes -- resolve before publication

* We also need a note at end of module about the fact that the cell is growing so that in more robust models the concentration is constantly decreasing as a result of the cell growing since it needs to double its volume every cell cycle.

* Exercise: develop a version of Gray-Scott from prologue that mimics the repressilator (?)

* What about a link to TF networks of other species? Human? Are these datasets readily available?

* I wonder what happens in the repressilator if all three particles start out at the same concentration ... will there be enough noise in the system to bounce it into an oscillation?

* Something is missing which is that we could increase the degradation rate as well instead of negative autoregulation. Why do you think that the cell doesn't simply have a higher degradation rate? Why might this be impractical?

* In Noah's second tutorial, Phillip needs to have a question at the end of the tutorial asking the user to draw the conclusion on their own.

* In the results section, should say something about there being practical limitations to how much we can increase the reaction producing *Y*, which provides a trade-off hence why the reaction rate isn't higher.

* NOAH: should we have an exercise asking students to use NFSim to replicate the other network motifs studied in this module?

* (Cite Alon book more than in the Alon quote. Perhaps in acknowledgements.)

* The following section is really good but I have no idea where it goes.

## Engineering a repressilator

In *A Synthetic Oscillatory Network of Transcriptional Regulators* by Elowitz and Leibler[^oscillator], the repressilator model we have simulated in CellBlender was successfully tested in a real E. coli cell (an *in vivo* experiment). Instead of the X, Y, Z molecules we used in our simulation, the authors inserted the genes *TetR*, *LacI*, and *cI*. These genes were set up in the same arrangement as our simulation, however there were key differences in the scale of the model. Our simulation was carried out in a single space with approximately 300 molecules per species. The reactions were carried out on the order of around 600 reactions per time step for 120,000 steps.

![image-center](../assets/images/repressilator_ecoli.png){: .align-center}
The repressilator model used in Elowitz and Leibler's *E. coli* system.
{: style="text-align: center;"}

In contrast, Elowitz and Leibler described a model with a variety of different reaction rates, such as a 0.0005 promoter strength, 20 proteins created per transcript, and a protein half-life of 10 minutes. Interestingly, this scale led to oscillations occurring with a periodicity that spanned different generations of E. coli! Nevertheless, the real E. coli repressilator systems showed clear patterns of oscillations with robustness to interruptions and disturbances. How would our simulations hold up to interruptions, and why is this kind of behavior needed in oscillators?  

![image-center](../assets/images/nf_sim_interrupted_break.PNG){: .align-center}

![image-center](../assets/images/nf_sim_interrupted_long.PNG){: .align-center}

![image-center](../assets/images/nf_sim_interrupted_spike.PNG){: .align-center}

[Next module](../chemotaxis/home){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

[^ffl]: Image adapted from Mangan, S., & Alon, U. (2003). Structure and function of the feed-forward loop network motif. Proceedings of the National Academy of Sciences of the United States of America, 100(21), 11980–11985. https://doi.org/10.1073/pnas.2133841100

[^oscillator]: Elowitz, M. B. & Leibler, S. A Synthetic Oscillatory Network of Transcriptional Regulators. Nature 403, 335-338 (2000).

[^scNetwork]: Lee, T. I., Rinaldi, N. J., Robert, F., Odom, D. T., Bar-Joseph, Z., Gerber, G. K., … Young, R. A. (2002). Transcriptional regulatory networks in Saccharomyces cerevisiae. Science, 298(5594), 799–804. https://doi.org/10.1126/science.1075090
