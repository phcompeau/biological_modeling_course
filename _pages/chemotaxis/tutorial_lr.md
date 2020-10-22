---
permalink: /chemotaxis/tutorial_lr
title: "Getting Start and Modeling Steady State"
sidebar: 
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

This set of tutorials will gradually build a chemotaxis simulation with [BioNetGen](https://www.csb.pitt.edu/Faculty/Faeder/?page_id=409) at the molecular level from scratch.

In this page, we will:
 - Set up BioNetGen
 - Start to model ligand-receptor dynamics in BNG
 - Explore several key aspects of BNG modeling: rules, species, simulation method, and parameters
 - Model the steady state of ligand-receptor dynamics

## What is BioNetGen?

[BioNetGen](http://bionetgen.org/) (BNG) is a software for specification and simulation of rule-based modeling. The chemotaxis pathyway is essentially a set of rules that can specify a set of mathematical equations of concentration of molecules, and we would like to translate it into a simulation. We can specify our rules in BNG, run the simulation, and visualize the results easily. 

## Set-up

[RuleBender](https://github.com/RuleWorld/rulebender/releases/tag/RuleBender-2.3.2) is the graphical interface for BioNetGen. Please [download](https://github.com/RuleWorld/rulebender/releases/tag/RuleBender-2.3.2) the version corresponding to your operating system. Here is a step-by-step [installation guide](https://github.com/RuleWorld/rulebender/blob/master/docs/RuleBender-installation-guide.pdf).

## Starting with Ligand-Receptor Dynamics

You can download the simulation file here: 
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/ligand_receptor.bngl" download="ligand_receptor.bngl">ligand_receptor.bngl</a>

In our system, there are only two types of molecules: the ligand (L), and the receptor (actually a receptor complex in our chemotaxis system which we will elaborate on later, and often abbreviated as T). The ligand can bind to the receptor, forming an intermediate, and they could also dissociate. We write this reaction as `L + T <-> L.T`, where the formation of intermediate is **forward reaction**, and the dissociation is **reverse reaction**.

In our system, the numbers of free ligands and free receptors drop quickly at the beginning of the simulation, because free ligands and free receptors are readily to meet each other. After a while, there will be more `L.T` in the system and therefore more dissociation; at the same time, because free `L` and `T` are less abundant, less binding happens. The system will gradually reach a steady-state where the rate of `L` and `T` binding equilibrates with `L.T` dissociation.
- Rate of forward reaction: `k_lr_bind`[*L*][*T*], where `k_lr_bind` is the rate constant.
- Rate of reverse reaction:  `k_lr_dis`[*L.T*], where `k_lr_dis` is the rate constant.
- Equilibrium is reached when `k_lr_bind`[*L*][*T*] = `k_lr_dis`[*L.T*].

We will simulate reaching this steady state.

First, open RuleBender. Select "New BioNetGen Project" under File. Since we are going to build from scratch, we can start with blank files. Feel free to start with any existing template too. Name it as you like.
![image-center](../assets/images/chemotaxis_tutorial1.png){: .align-center}

Rename your `.bngl` file as `ligand_receptor.bngl`. Now you should be able to start coding at line 1.

![image-center](../assets/images/chemotaxis_tutorial2.png){: .align-center}

## Specifying molecule types

We will walk through all codes, but for your reference, BNG documentation can be found [here](http://comet.lehman.cuny.edu/griffeth/BioNetGenTutorialFromBioNetWiki.pdf).

We need to tell BNG the rules for our model. To specify our model, specify the `begin model` and `end model`. We will add all model specification information between the two lines. Add molecules to the model under the `molecule types` section. The `(t)` specifies that molecule `L` contains one component: the binding site with `T`. Same for `T`: the `(l)` specifies the component binding to `L`. We will use this component for L-R binding later. Here, the letter of components indicates its binding partner (for example, `t` indicates binding with `T`), but feel free to substitute with other notations.

~~~ ruby
	begin model

	begin molecule types
		L(t)
		T(l)
	end molecule types

	end model
~~~

## Specifying reaction rules

BNG reaction rules are written similar as chemical equations. Left-hand-side includes the reactants, which is followed by a unidirectional or bidirectional arrow, indicating uni/bi-directional reaction, and right-hand-side includes the products. After that, indicate the rate constant of reaction; and if bi-directional, separate forward and backward reaction rate constants with a comma. For example, to code up A + B <-> C with forward rate k1, reverse rate k2, we can write `A + B <-> C k1, k2`.

Also add reaction rules within the model. At the left-hand-side, by specifying `L(t)`, we select only unbound `L` molecules; by `T(l)`, we select only unbound receptors; if we wanted to select any ligand molecule, simply write `L`. At the right-hand-side of the reaction, `L(t!1).T(l!1)` indicates the formation of the intermediate. In BNG, `!` indicates formation of a bond; and a unique character specifies each bond type. We will denote this bond as `!1`. Since the reaction is bidirectional, we will use `k_lr_bind` to denote the rate of forward reaction, and `k_lr_dis` to denote the rate of reverse reaction. *Note: if you compile now, an error will occur because we haven't define parameters yet.*

~~~ ruby
	begin reaction rules
		LR: L(t) + T(l) <-> L(t!1).T(l!1) k_lr_bind, k_lr_dis
	end reaction rules
~~~

We need to specify how many molecules we want to put at the start of the simulation within `seed species` section. We are putting `L0` unbound L molecules, and `T0` unbound T molecules at the beginning.

~~~ ruby
	begin seed species
		L(t) L0
		T(l) T0
	end seed species
~~~

## Specifying parameters

Now let's declare all the parameters we mentioned __before__ any usage of them (here, before the `reaction rules` section). BNG is unitless. For simplicity, we will use the number of molecule per cell for all cellular components. The reaction rates are conventionally in the unit of M/s. 

Because of the molecules/cell and the M/s units, we need to do some unit conversion here (when calculating the ligand-receptor binding by hand in the main text, we already did unit conversion for you). The volume of *E. coli* is approximately 1µm<sup>3</sup>, so our molecule counts are of unit num_molecule/µm^<sup>3</sup>. One mole of molecule is approximately 6.02 · 10<sup>32</sup> molecules (Avogadro's number), so the unit M is approximately 6.02 · 10<sup>23</sup> molecules/L, or 6.02 · 10<sup>8</sup> molecules/µm<sup>3</sup>. We record this as `NaV`. For bimolecular reactions, the rate constant should have unit M<sup>-1</sup>s<sup>-1</sup>, and we devide with NaV to convert to (molecules/µm<sup>3</sup>)<sup>-1</sup>)s<sup>-1</sup>. For monomolecular reactions, the rate constant have unit s<sup>-1</sup>, so no unit conversion is required.

Although the specific numbers of cellular components vary among each bacterium, the components in chemotaxis pathway follows a relatively constant ratio. For all the simulations in this module, we assign the initial number for each molecule and reaction rates by first deciding a reasonable range based on *in vivo* quantities [^Li2004][^Spiro1997][^Stock1991] and then tuning to fit the model.

~~~ ruby
	begin parameters
		NaV 6.02e8    #Unit conversion M -> #/µm^3
		L0 1e4        #number of ligand molecules
		T0 7000       #number of receptor complexes
		k_lr_bind 8.8e6/NaV   #ligand-receptor binding
		k_lr_dis 35           #ligand-receptor dissociation
	end parameters
~~~

Before simulating our model, we would also like to define the observables under `observables` section within the model specification. 

~~~ ruby
	begin observables
		Molecules free_ligand L(t)
		Molecules bound_ligand L(t!l).T(l!l)
		Molecules free_receptor T(l)
	end observables
~~~

If you save the file now, you should be able to see a Contact-Map indicating the potential bonding of L and T at the upper right corner of your graphical user interface. A contact map helps to visualize the interaction of species in the system.

![image-center](../assets/images/chemotaxis_tutorial3.png){: .align-center}

## Specifying simulation commands

And now we are ready to simulate. Add the `generate_network` and `simulate` command outside of your model specification. We will specify three arguments:

**Method**. We will use `method=>"ssa"` (Gillespie algorithm or SSA) in all the tutorials, but there are also `method=>"nf"` (network-free) and `method=>"ode"`(ordinary differential equations) that you can try. 

**Time span**.`t_end`, the simulation duration. BNG simulation time is unitless; for simplicity we define all time units to be second.

**Number of Steps**. `n_steps` tells the program to break the simulation into how many time points to report the concentration.

~~~ ruby
	generate_network({overwrite=>1})
	simulate({method=>"ssa", t_end=>1, n_steps=>100})
~~~

The whole simulation code for ligand-receptor dynamics (you can also download here: 
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/ligand_receptor.bngl" download="ligand_receptor.bngl">ligand_receptor.bngl</a>)

~~~ ruby
	begin model

	begin molecule types
		L(t)
		T(l)
	end molecule types

	begin parameters
		NaV2 6.02e8   #Unit conversion to cellular concentration M/L -> #/um^3
		L0 1e4        #number of ligand molecules
		T0 7000       #number of receptor complexes
		k_lr_bind 8.8e6/NaV2   #ligand-receptor binding
		k_lr_dis 35            #ligand-receptor dissociation
	end parameters

	begin reaction rules
		LR: L(t) + T(l) <-> L(t!1).T(l!1) k_lr_bind, k_lr_dis
	end reaction rules

	begin seed species
		L(t) L0
		T(l) T0
	end seed species

	begin observables
		Molecules free_ligand L(t)
		Molecules bound_ligand L(t!l).T(l!l)
		Molecules free_receptor T(l)
	end observables

	end model

	generate_network({overwrite=>1})
	simulate({method=>"ssa", t_end=>1, n_steps=>100})
~~~

**STOP:** Based on your results from calculating by hand, predict how would the concentrations change.
{: .notice--primary}

Go to `Simulation` at the right side of the Contact Map button and click `Run`. You can visualize your `.gdat` data.

What do you predict to be the concentration of `L(t!l).T(l!l)` after 1 hour?

If you are interested, a more detailed tutorial on BNG modeling can be found [here](http://comet.lehman.cuny.edu/griffeth/BioNetGenTutorialFromBioNetWiki.pdf).

[^Li2004]: Li M, Hazelbauer GL. 2004. Cellular stoichimetry of the components of the chemotaxis signaling complex. Journal of Bacteriology. [Available online](https://jb.asm.org/content/186/12/3687)

[^Stock1991]: Stock J, Lukat GS. 1991. Intracellular signal transduction networks. Annual Review of Biophysics and Biophysical Chemistry. [Available online](https://www.annualreviews.org/doi/abs/10.1146/annurev.bb.20.060191.000545)

[^Spiro1997]: Spiro PA, Parkinson JS, and Othmer H. 1997. A model of excitation and adaptation in bacterial chemotaxis. Biochemistry 94:7263-7268. [Available online](https://www.pnas.org/content/94/14/7263).

[^Schwartz14]: Schwartz R. Biological Modeling and Simulaton: A Survey of Practical Models, Algorithms, and Numerical Methods. Chapter 14.1. 

[^Schwartz17]: Schwartz R. Biological Modeling and Simulaton: A Survey of Practical Models, Algorithms, and Numerical Methods. Chapter 17.2.


[Back to Main Text](home_signalpart2){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}



