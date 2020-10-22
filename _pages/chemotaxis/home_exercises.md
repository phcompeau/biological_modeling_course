---
permalink: /chemotaxis/home_exercise
title: "Exercises"
sidebar:
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

## How do *E. coli* respond to repellents?

1. Based on what we've learned about chemotaxis towards higher attractant concentrations, can you summarize how would *E. coli* cells repond to an repellent?

2. Modify our BNG model to simulate what will happen if repellents, instead of attractants, are present in the system. In [phosphorylation tutorial](tutorial_phos), we defined the rate constant for free CheA autophosphorylation `k_T_phos`, and specified that when the receptor complex is bound to an attractant molecule, the autophosphorylation rate constant becomes `0.2 · k_T_phos`. Now, let's further specify that when the receptor complex is bound to a **repellent** molecule, the autophosphorylation rate constant becomes `5 · k_T_phos`. Please implement this rule in our BNG model. Please simulate the system with `L0 = 5000` and `L0 = 1e5` repellent molecules added at the beginning of the simulation, and observe the concentrations of chemical species for 3 seconds. How does the concentration of phosphorylated CheY change? What does that implicate?

## What if there are multiple attractant sources?

In the previous simulations, our *E. coli* cells only see a single gradient at all times. In real life, it is unlikely that a particular location gathers all nutrients in the world, and resource distribution is usually patchy, such that different nutrient sources accumulate unevenly in the landscape. Moreover, the distributions could vary through time. We will explore whether the chemotaxis mechanism allows the cells navigate through more realistic nutrient distributions.

1. When modeling *E. coli*'s CheY phosphorylation level when [moving up the gradient](home_gradient), we observed that *E. coli* can continuously respond to an increasing concentration. Here we will explore what if there are different types of attractant molecules. Before that, let's model a scenerio after the *E. coli* adapts to `L0 = 1e6` ligand molecules, another `1e6` same ligand molecules are suddenly add to the system. Please modify your model from [adaptation tutorial](tutorial_senseadap). How does the cell respond to the additional `1e6` ligand molecules? (*Hint*: set an additional component for ligand `L(t,Lig~A~B)`, and specify the rate constant for binding of `L(t,Lig~B)` and the receptor with a `function`: if the cell hasn't adapt to the first batch of ligand, the rate constant is 0; otherwise the rate is `k_lr_bind` - recall defining `function`s in [up-gradient tutorial](tutorial_gradient). You can judge whether the cell is adapted to `1e6` ligand *A* molecules by methylation levels.)

2. So far our model only included one type of attractant and one type of receptor. However, *E. coli* has different receptors specific for different types of attractants. Assuming all receptor types influence the downstream reactions equally, can we include multiple ligand/receptor types into our model easily? Please modify your model from [adaptation tutorial](tutorial_senseadap) to reflect 2 types of receptor specific for 2 types of ligand (call them *A* and *B*). Assume we have 3500 receptor molecules of each type. (*Hint*: you won't need to have additional chemical species for `L` and `T`; just specify additional states for them, for example `L(t,Lig~A)` only binds with `T(l,Lig~A)`. Don't forget to update `seed species`.)

3. What will happen if after the cell adapts to [*A*], attractant B molecules are suddenly added to the system? Let's model a scenerio such that after the cell adapts to `1e6` ligand molecules *A*, suddenly add `1e6` ligand molecules *B*. Please observe the concentrations of phosphorylated CheY. Is the cell able to respond to ligand *B* after adapting to the concentration of ligand *A*? Why is CheY phosphorylation different from the scenerio of two releases of the same ligand? (*Hint*: the hints for part 1 also apply here.)

4. What if there are two nutrient rich locations? In [chemotactic walk tutorial](tutorial_walk), we have a concentration gradient growing exponential from center to the goal (1500, 1500), so that *L(x,y)* = 100 · 10<sup>8 · (1-*d*/*D*)</sup>. Please modify your model from the tutorial to include another goal at location (-1500, 1500), and a similar concentration gradient growing from the center to the goal. The new concentration of ligands, [*L*] will be *L(x,y)* = 100 · 10<sup>8 · (1-*d*<sub>1</sub>/*D*<sub>1</sub>)</sup> + 100 · 10<sup>8 · (1-*d*<sub>2</sub>/*D*<sub>2</sub>)</sup>, where *d*<sub>1</sub> is the distance from *(x,y)* to goal1 (1500, 1500), *d*<sub>2</sub> is the distance from *(x,y)* to goal2 (-1500, 1500), and *D*<sub>1</sub> = *D*<sub>2</sub> are the distances from the origin to goal1 and goal2. Please simulate with tumble every 1 second as the background tumbling frequency, and visualize trajectories for several cells. Are the cells able to find the goals?

## The actual tumbling reorientation is smarter than our model?

Earlier we said that when *E. coli* tumbles, the degree of reorientation is actually not uniformly random from 0° to 360°. With background ligand concentration, the degree of reorientation approximately follows a normal distribution with mean of 68° and standard deviation of 36°. Recent research suggests that when the cell is moving up the gradient, the degree of reorientation is smaller [^Saragosti2011]. Although currently we don't have definitive measurements for the smaller angle of reorientation when moving up the gradient, let's specify it is 0.1 π smaller. Please modify your model from [chemotactic walk tutorial](tutorial_walk) to change the random uniform sampling to this "smarter" sampling. 

Please quantitatively compare the performance for the chemotactic walk strategy, and this smarter strategy by calculating the mean and standard deviation of each cell's distance to the goal for 500 cells with `time_exp = [0.2, 0.5, 1.0, 2.0, 5.0]`. How much faster can the cells find the goal? Why faster?

## Want another BNG model?

Like what we've seen in this module, BNGL is very good at simulating systems that involve a large number of species and particles yet can be summarized with a small set of rules. Polymerization reactions is another good example of such systems. **Polymerization** is the process of monomer molecules react to form polymer chains, for example, polyvinyl chloride (PVC) is formed from many vinyl monomers. We can build a BNGL model to simulate a version of polymerization of monomer *A* to form polymer *AAAAAA*... The reaction can be written as *A*<sub>m</sub> + *A*<sub>n</sub> -> *A*<sub>m+n</sub>. There are two sites on `A` that are involved in the reaction: one "head" to join a free "tail", and one "tail" to allow a "head" to bind. We will model a polymerization reaction with BNG (this model is inspired by the [BLBR model in official BNG tutorials](https://github.com/RuleWorld/BNGTutorial/blob/master/CBNGL/BLBR.bngl)).

Please open a new `.bngl` file. We will have only one moleclue type: `A(h,t)`; the `s` and `t` indicates the "head" and "tail". Please implement the four reaction rules: 
- initializing the series of reactions: two unbound `A` forms the intial dimer; 
- adding an unbound `A` to the "tail" of an existing `A` n-mer to form an `A` (n+1)-mer; 
- adding an existing `A` n-mer to the "tail" of an unbound `A` to form an (n+1)-mer;
- adding an existing `A` m-mer to the "tail" of an existing `A` n-mer to form an (n+m)-mer.

To select any species that is bound at a component, please use `!+`. For example, `A(h!+,t)` will select any `A` bound at "head". The use of `+` is similar as in regular expression. Set all forward and reverse reaction rates to be 0.01.

We will simulate with 1000 `A` monomers at the beginning of the simulation, and observe for the formation of polymers composed of different number of `A`s. To do so, we select the pattern of containing *x* `A`'s with `A == x`. `Species` instead of `Molecules` is required for selecting polymer patterns.

	begin seed species
		A(h,t) 1000
	end seed species

	begin observables
		Species A1 A==1
		Species A2 A==2
		Species A3 A==3
		Species A5 A==5
		Species A10 A==10
		Species A20 A==20
		Species ALong A>=30
	end observables

For this model, let's try another simulation method - **Network-free** simulation. It is similar to the SSA simulation [we used before](home_signalpart2), but instead of simulating transitions between states of the whole *system*, it tracks individual *particles*. In this polymerization model, the possible number of reactions is much higher than we had in chemotaxis models - we can have any m-mer reacting with any n-mer at any step. Considering all these possible reactions makes SSA slow. Luckily, we don't have that many particles, so trucking each particle will be much faster (in our chemotaxis model, tracking over 10<sup>8</sup> molecules individually is difficult).

Please simulate with the command `simulate({method=>"nf", t_end=>100, n_steps=>1000})`. Note that we do not need the `generate_network()` command. What happens to the concentration of short A polymers? What about the long A polymers?

We care about polymerization reactions because they are involved in many essential biological processes. For example, although more enzymes and reaction intermediates are involved, starches are polymers of glucose molecules. Another example is the elongation step in RNA translation, in which tRNAs transfer amino acids to a growing amino acid chain. (Can you modify our model to only allow `A` monomers to be added to the "tail" of growing `A` n-mers?) The products of RNA translation are proteins, and we will explore protein folding in the next module.



[^Saragosti2011]: Saragosti J, Calvez V, Bournaveas, N, Perthame B, Buguin A, Silberzan P. 2011. Directional persistence of chemotactic bacteria in a traveling concentration wave. PNAS. [Available online](https://www.pnas.org/content/pnas/108/39/16235.full.pdf)

[^Saragosti2012]: Saragosti J., Siberzan P., Buguin A. 2012. Modeling *E. coli* tumbles by rotational diffusion. Implications for chemotaxis. PLoS One 7(4):e35412. [available online](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3329434/).

[^Berg1972]: Berg HC, Brown DA. 1972. Chemotaxis in Escherichia coli analysed by three-dimensional tracking. Nature. [Available online](https://www.nature.com/articles/239500a0)

[^Baker2005]: Baker MD, Wolanin PM, Stock JB. 2005. Signal transduction in bacterial chemotaxis. BioEssays 28:9-22. [Available online](https://pubmed.ncbi.nlm.nih.gov/16369945/)


[Next module](../coronavirus/home){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
