---
permalink: /chemotaxis/tutorial_phos
title: "Adding Phosphorylation to our BioNetGen Model"
sidebar:
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

In this tutorial, we will extend the BioNetGen model covered in the [tutorial_lr](ligand-receptor tutorial) to add the phosphorylation chemotaxis mechanisms described in the main text, shown in the figure reproduced below.

![image-center](../assets/images/chemotaxisphosnew.png){: .align-center}

To get started, create a copy of your file from the ligand-receptor tutorial and save it as `phosphorylation.bngl`. If you would rather not follow along below, you can download a completed BioNetGen file here:
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/phosphorylation.bngl" download="phosphorylation.bngl">phosphorylation.bngl</a>

## Defining molecules

First, we introduce `state` in our particles to mark whether it is phosphorylated or not. Change `T(l)` to `T(l,Phos~U~P)`. The `Phos~U~P` indicates we introduce phosphorylation states to `T`: `U` indicates unphosphorylated, and `P` indicates phosphorylated. You can also use other letters. We also add molecule `CheY(Phos~U~P)` and `CheZ()`. (*Note: be careful with the use of spaces; don't put spaces after the comma.*)

~~~ ruby
begin molecule types
	L(t)             #ligand molecule
	T(l,Phos~U~P)    #receptor complex
	CheY(Phos~U~P)
	CheZ()
end molecule types
~~~

We are also interested in the number of T-P and CheY-P during the simulation.

~~~ ruby
begin observables
	Molecules phosphorylated_CheY CheY(Phos~P)
	Molecules phosphorylated_CheA T(Phos~P)
	Molecules bound_ligand L(t!1).T(l!1)
end observables
~~~

## Defining reactions

And we update the reaction rules with the phosphorylation and dephosphorylation reactions above.

~~~ ruby
begin reaction rules
	LigandReceptor: L(t) + T(l) <-> L(t!1).T(l!1) k_lr_bind, k_lr_dis

	#Free vs. ligand-bound complexes autophosphorylates
	FreeTP: T(l,Phos~U) -> T(l,Phos~P) k_T_phos
	BoundTP: L(t!1).T(l!1,Phos~U) -> L(t!1).T(l!1,Phos~P) k_T_phos*0.2

	YP: T(Phos~P) + CheY(Phos~U) -> T(Phos~U) + CheY(Phos~P) k_Y_phos
	YDep: CheZ() + CheY(Phos~P) -> CheZ() + CheY(Phos~U) k_Y_dephos
end reaction rules
~~~

## Initializing molecules and parameters

We need to indicate the number of molecules at **each state** present at the beginning of the simulation. Since we are adding ligands at the beginnin of the simulation, the initial amount of molecules at each same state should be equal to the equilibrium concentrations when no ligand is present.

~~~ ruby
begin seed species
	L(t) L0
	T(l,Phos~U) T0*0.8
	T(l,Phos~P) T0*0.2
	CheY(Phos~U) CheY0*0.5
	CheY(Phos~P) CheY0*0.5
	CheZ() CheZ0
end seed species
~~~

Update all the parameters based on *in vivo* quantities [^Li2004][^Spiro1997][^Stock1991]. Specifically, we include the number of CheY, CheZ, and the rate constants for receptor complex autophosphorylation, CheY phosphorylation, and CheZ dephosphorylation.

~~~ ruby
begin parameters
	NaV 6.02e8   #Unit conversion to cellular concentration M/L -> #/um^3
	L0 0        #number of ligand molecules
	T0 7000       #number of receptor complexes
	CheY0 20000
	CheZ0 6000

	k_lr_bind 8.8e6/NaV   #ligand-receptor binding
	k_lr_dis 35           #ligand-receptor dissociation
	k_T_phos 15           #receptor complex autophosphorylation
	k_Y_phos 3.8e6/NaV    #receptor complex phosphorylates CheY
	k_Y_dephos 8.6e5/NaV  #Z dephosphoryaltes CheY
end parameters
~~~

## Running the simulation

Observe the simulation for a bit longer. Change `t_end` at the bottom to 3.

~~~ ruby
	generate_network({overwrite=>1})
	simulate({method=>"ssa", t_end=>3, n_steps=>100})
~~~

## Simulating responses to attractants

Before running the simulation, let's think about what will happen. If we don't add any ligand molecule into the system, then T phosphorylation happens at rate [*T*]*k_T_phos*, and T will phosphorylate CheY, which will be dephosphorylated by CheZ. The concentrations of phosphorylated T and CheY will stay at a steady state. That's the initial concentrations of molecules at each state we defined earlier.

Run simulation with no ligand molecule present by setting `L0` in the `parameters` section to 0, and click `Run` under `Simulate`. What do you observe?

When we add ligand molecules into the system, as we did in the tutorial for [ligand-receptor dynamics](tutorial_lr), concentration of bound T increases. What will happen to the concentration of phosphorylated CheA, and phosphorylated CheY? What will happen to steady state concentrations?

Run simulation with `L0 = 5000` and `L0 = 1e5` to confirm your hypothesis. What do you observe?

For different `L0`'s, how do the steady state for bound ligand, active receptor, and active CheY differ and why?

Exercise: Try several different `L0` values (ex. 1e3, 1e7, 1e9). Are you seeing what you expected? If at some point the result won't change anymore, why? What does it imply about limitation in chemotaxis (but it's already a wide range of concentrations isn't it)?

## blah

Our model will start with the ligand binding and dissociation reaction `L + T <-> LT` with rate `k_lr_bind`, `k_lr_dis`. It will then need to expand to include the following additional reactions.



**Receptor complex autophosphorylation**. The receptor complex is composed of MCPs, CheW, and CheA. CheA undergoes autophosphorylation, and the rate of autophosphorylation depends on conformation of the receptor complex. Faster autophosphorylation for free MCPs. Note that the phosphoryl group is from an ATP->ADP reaction, but we will just code as phosphorylation states in modeling for simplicity.
 - T -> T-P    rate constant `k_T_phos`
 - LT -> LT-P  rate constant `0.2 · k_T_phos`

**CheY phosphorylation and dephosphorylation**. CheA-P phosphorylates CheY. Phosphorylated CheY will be responsible for the cellular response (CW rotataion), so we will use the level of CheY-P to indicate the level of cellular response.
 - T-P + CheY -> T + CheY-P  `k_Y_phos`
 - Z + Y-P -> Z + Y + P `k_Y_dephos`


[^Bertoli2013]: Bertoli C, Skotheim JM, de Bruin RAM. 2013. Control of cell cycle transcription during G1 and S phase. Nature Reviews Molecular Cell Biology 14:518-528. [Available online](https://www.nature.com/articles/nrm3629).

[^Li2004]: Li M, Hazelbauer GL. 2004. Cellular stoichimetry of the components of the chemotaxis signaling complex. Journal of Bacteriology. [Available online](https://jb.asm.org/content/186/12/3687)

[^Stock1991]: Stock J, Lukat GS. 1991. Intracellular signal transduction networks. Annual Review of Biophysics and Biophysical Chemistry. [Available online](https://www.annualreviews.org/doi/abs/10.1146/annurev.bb.20.060191.000545)

[^Spiro1997]: Spiro PA, Parkinson JS, and Othmer H. 1997. A model of excitation and adaptation in bacterial chemotaxis. Biochemistry 94:7263-7268. [Available online](https://www.pnas.org/content/94/14/7263).


[Return to main text](home_biochem#tumbling-frequency-and-changing-ligand-concentrations){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
