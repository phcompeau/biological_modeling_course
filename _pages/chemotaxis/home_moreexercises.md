---
permalink: /chemotaxis/home_moreexercise
title: "Exercises"
sidebar:
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

## How to calculate steady state concentration in a reversible bimolecular reaction?

Earlier in this chapter we learned how to calculate equilibrium concentrations of a [reversible bimolecular reaction](home_signal). It's time to get some exercise.

**Exercise:** How would the concentration of molecules change before and after the system reaches the steady state?

1. We have three types of molecules in the system, *A*, *B*, and *AB*. The molecules could form a complex via reaction *A* + *B* → *AB*, and the complexes could dissociate via reaction *AB* → *A* + *B*. Assume we know that *k*<sub>bind</sub> = 3, *k*<sub>dissociate</sub> = 3. Currently, the concentrations of each type of molecules are [*A*] = 95, [*B*] = 95, [*AB*] = 5. If we allow the system to continue to react, what are the concentrations of each type of molecules at the steady state?
2. Now assume we are at the equilibrium state and add additional 100 *A* molecules. What reactions would happen to the system and how would equilibrium concentrations change? What if *k*<sub>dissociate</sub> = 3 becomes 9? Verify your predictions with calculation.


## How to simulate a reaction step with the Gillespie algorithm?

We learned that [Poisson distribution, exponential distribution, and Gillespie algorithm](home_signalpart2) are behind the BioNetGen simulation. Let's try to simulate step by step in this way.

**Exercise:** We are interested in the "wait time" between individual reactions. Will wait time be longer or shorter if we have more molecules in the system?
{: .notice--info}

1. In the chapter we used counting customers entering the store as an example. Now we will think about a chemical system instead. Say that you are looking at a flask, and you have noticed that on average 100 reactions happen per second. What is the probability that exactly 100 reaction happen in the next second? Now you would like to see how long does it take for the next reaction to occur. How long would you expect to wait? What is the probability that the first reaction occur after 0.02 second?\\
 **Hint**: What is the λ in your system? What is the mean value of an exponential distribution?
2. Now let's consider a reaction in a very simplified bimolecular reaction system. There are two types of molecules, the ligand *L*, and the rececptor *T*. The reaction rate constant for binding is *k*<sub>bind</sub> = 1, and for dissociation is *k*<sub>dissociate</sub> = 2. Initially, the system contains 10 *L* molecules and 10 *T* molecules, and there is no *LT* present yet. How long would you expect to wait before the first reaction occurs? Is it possible that the first reaction occur after 0.1s? What is your first reaction and what molecules are present in the system after your first reaction? 
3. We continue to observe the reaction system after the first reaction occurs. What reactions are possible in the system now? How long would you expect to wait before the next reaction occurs? What are the probabilities of each possible reaction?


## What if the concentration gradient is linear?

In our BioNetGen model, we implemented the scenerio when a bacterium swims up against an [exponential attractant gradient](home_gradient). With the help of BioNetGen, we can actually simulate a variety of gradients. Now let's work with a scenerio such that the attractant concentration grows from 10<sup>4</sup> to 10<sup>5</sup> constantly. You could make a copy of the exponential gradient simulation and edit from there.

1. We could implement such a linear concentration by introducing a "fake" molecule type *L2*. These "fake" molecules convert to the ligand *L* at a constant rate. In `seed species`, please define `@EC:L2() 9e4`. Mirroring the exponential gradient, in the `reaction rules`, we could define `LAdd: L2() -> L(t) addRate()`, where `addRate()` is a function of `L2` concentration and the rate constant `k_add = 0.1`. Why do we need a function instead of just using `k_add` directly? The reason is that through time as `L2` concentration decreases the reaction of converting `L2` to `L` will get smaller, but we would like to get a constant rate of `L` concentration increasing. How would you implement the `addRate()` function? Run your simulation with `simulate({method=>"ssa", t_end=>500, n_steps=>300})`.
2. Predict what will happen if the gradient is steeper, or the attractant concentration is higher? Verify with BioNetGen simulation.



