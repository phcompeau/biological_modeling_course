---
permalink: /chemotaxis/tutorial_lr
title: "Getting Started with BioNetGen and Modeling Ligand-Receptor Dynamics"
sidebar:
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

This collection of tutorials will gradually build up from scratch a chemotaxis simulation using [BioNetGen](https://www.csb.pitt.edu/Faculty/Faeder/?page_id=409).

In this tutorial, we will:
 - set up BioNetGen;
 - explore several key aspects of BioNetGen modeling: rules, species, simulation method, and parameters
 - use BioNetGen to model ligand-receptor dynamics and compute a steady-state concentration of ligands and receptors.

## What is BioNetGen?

[BioNetGen](http://bionetgen.org/) is a software application for specification and simulation of rule-based modeling. In past modules, we have worked with chemical reactions that can be thought of as rules (e.g., "whenever an *X* particle and a *Y* particle collide, replace them with a single *X* particle"). The chemotaxis pathway also can be thought of as a set of biochemical rules specifying a set of mathematical equations dictating molecule concentrations. Our larger goal is to use BioNetGen to translate these rules into a reasonable chemotaxis simulation, then visualize and interpret the results.

In this tutorial, we will focus only on modeling ligand-receptor dynamics, which we will use as a starting point for more advanced modeling later.

## Installation and setup

[RuleBender](https://github.com/RuleWorld/rulebender/releases/tag/RuleBender-2.3.2) is the graphical interface for BioNetGen. Please [download](https://github.com/RuleWorld/rulebender/releases/tag/RuleBender-2.3.2) the version corresponding to your operating system. Here is a step-by-step [installation guide](https://github.com/RuleWorld/rulebender/blob/master/docs/RuleBender-installation-guide.pdf).

## Starting with Ligand-Receptor Dynamics

In this tutorial, we will build our model from scratch. If you like instead, you can download the completed simulation file here:
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/ligand_receptor.BioNetGenl" download="ligand_receptor.BioNetGenl">ligand_receptor.BioNetGenl</a>

In our system, there are only two types of molecules: the ligand (`L`), and the receptor (`T`). (The receptor is in fact a receptor complex because it is attached to additional molecules, which we will elaborate on later). The ligand can bind to the receptor, forming an intermediate, and the complex can also dissociate. We write this reaction as `L + T <-> L.T`, where the formation of the intermediate is called the **forward reaction**, and the dissociation is called the **reverse reaction**.

In our system, which starts with a quantity of free ligands and receptors, the numbers of these free molecules should drop quickly, because free ligands and free receptors are readily to meet each other. After a while, there will be more `L.T` in the system and therefore more dissociation; at the same time, because free `L` and `T` are less abundant, less binding happens. The system will gradually reach a steady-state where the rate of `L` and `T` binding equilibrates with `L.T` dissociation.

We will simulate reaching this steady state, which means that we will need to know the following two parameters:

1. The rate of the forward reaction: `k_lr_bind [L][T]`, where `k_lr_bind` is the rate constant.
2. The rate of the reverse reaction:  `k_lr_dis[L.T]`, where `k_lr_dis` is the rate constant.

Equilibrium is reached when `k_lr_bind [L][T]` = `k_lr_dis[L.T]`. Our goal in this tutorial is to use BioNetGen to determine this equilibrium in molecule concentrations as a proof of concept.

First, open RuleBender and select `File > New BioNetGen Project`.

![image-center](../assets/images/chemotaxis_tutorial1.png){: .align-center}

Save your file as `ligand_receptor.BioNetGenl`. Now you should be able to start coding on line 1.

![image-center](../assets/images/chemotaxis_tutorial2.png){: .align-center}

## Specifying molecule types

We will specify everything needed for this tutorial, but if you are interested, reference BioNetGen documentation can be found [here](http://comet.lehman.cuny.edu/griffeth/BioNetGenTutorialFromBioNetWiki.pdf).

To specify our model, add `begin model` and `end model`. Everything below regarding the specification of the model will go between these two lines.

We first add molecules to our model under a `molecule types` section. We will have molecules corresponding to `L` and `T` that we call `L(t)` and `T(l)`, respecively. The `(t)` specifies that molecule `L` contains a binding site with `T`, and the `(l)` specifies a component binding to `L`. We will use these components later when specifying reactions. You do not have to use `t` and `l` for this purpose, but it will make your model easier to understand.

~~~ ruby
	begin model

	begin molecule types
		L(t)
		T(l)
	end molecule types

	end model
~~~

## Specifying reaction rules

BioNetGen reaction rules are written similarly to chemical equations. The left side of the rule includes the reactants, which are followed by a unidirectional or bidirectional arrow, indicating the direction of the reaction, and the right side of the rule includes the products. After the reaction we indicate the rate constant of reaction; if the reaction is bi-directional, then we separate the forward and backward reaction rate constants with a comma.

For example, to code up the bi-directional reaction `A + B <-> C` with forward rate `k1` and reverse rate `k2`, we would write `A + B <-> C k1, k2`.

Our model consists of a single bidirectional reaction and will have only a single rule. The left side of this rule will be `L(t) + T(l)`; by specifying `L(t)` and `T(l)`, we indicate to BioNetGen that we are only interested in *unbound* ligand and receptor molecules. If we had wanted to select any ligand molecule, then we would have simply written `L + T`.

On the right side of the rule, we will have `L(t!1).T(l!1)`, which indicates the formation of the intermediate. In BioNetGen, `!` indicates formation of a bond; and a unique character specifies the possible location of this bond. In our case, we use the character `1`, so that the bond is represented by `!1`. The symbol `.` is used to indicate that the two molecules are joined into a complex.

Since the reaction is bidirectional, we will use `k_lr_bind` and `k_lr_dis` to denote the rates of the forward and reverse reactions, respectively. (We will specify values for these parameters later.)

~~~ ruby
	begin reaction rules
		LR: L(t) + T(l) <-> L(t!1).T(l!1) k_lr_bind, k_lr_dis
	end reaction rules
~~~

## Initializing the simulation

We need to specify how many molecules we want to put at the start of the simulation within `seed species` section. We are putting `L0` unbound L molecules, and `T0` unbound T molecules at the beginning.

~~~ ruby
	begin seed species
		L(t) L0
		T(l) T0
	end seed species
~~~

## Specifying parameters

Now let's declare all the parameters we mentioned __before__ any usage of them (here, before the `reaction rules` section). BioNetGen is unitless. For simplicity, we will use the number of molecule per cell for all cellular components. The reaction rates are conventionally in the unit of M/s.

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

**Time span**.`t_end`, the simulation duration. BioNetGen simulation time is unitless; for simplicity we define all time units to be second.

**Number of Steps**. `n_steps` tells the program to break the simulation into how many time points to report the concentration.

~~~ ruby
	generate_network({overwrite=>1})
	simulate({method=>"ssa", t_end=>1, n_steps=>100})
~~~

The whole simulation code for ligand-receptor dynamics (you can also download here:
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/ligand_receptor.BioNetGenl" download="ligand_receptor.BioNetGenl">ligand_receptor.BioNetGenl</a>)

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

If you are interested, a more detailed tutorial on BioNetGen modeling can be found [here](http://comet.lehman.cuny.edu/griffeth/BioNetGenTutorialFromBioNetWiki.pdf).

[^Li2004]: Li M, Hazelbauer GL. 2004. Cellular stoichimetry of the components of the chemotaxis signaling complex. Journal of Bacteriology. [Available online](https://jb.asm.org/content/186/12/3687)

[^Stock1991]: Stock J, Lukat GS. 1991. Intracellular signal transduction networks. Annual Review of Biophysics and Biophysical Chemistry. [Available online](https://www.annualreviews.org/doi/abs/10.1146/annurev.bb.20.060191.000545)

[^Spiro1997]: Spiro PA, Parkinson JS, and Othmer H. 1997. A model of excitation and adaptation in bacterial chemotaxis. Biochemistry 94:7263-7268. [Available online](https://www.pnas.org/content/94/14/7263).

[^Schwartz14]: Schwartz R. Biological Modeling and Simulaton: A Survey of Practical Models, Algorithms, and Numerical Methods. Chapter 14.1.

[^Schwartz17]: Schwartz R. Biological Modeling and Simulaton: A Survey of Practical Models, Algorithms, and Numerical Methods. Chapter 17.2.


[Back to Main Text](home_signalpart2){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
