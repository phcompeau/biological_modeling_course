---
permalink: /chemotaxis/tutorial_adap
title: "Adaptation"
sidebar: 
 nav: "chemotaxis"
---

In this page, we will:
 - Add adaptation mechanisms
 - Learn compartmentalization rules for BNG and add to our model
 - Explore the behavior of CheY

## Include methylation in the model

Now let's add methylation states to model how *E. coli* can adapt to a higher attractant concentrations and bring back the tumbling frequency. The methylation states of the receptors store the *past* ligand concentrations. If we have a high level ligand-receptor binding, and that is consistent with the methylation states, then the cell doesn't need to decrease its tumbling frequency because no gradient is present. This is achieved by using higher methylation states to reflect higher past ligand concentration, leading to higher rates of autophosphorylation. This compensates for the low phosphorylation due to high levels of ligand binding. 

Our model will be based on the [model](https://www.pnas.org/content/94/14/7263) by Spiro et al.[^Spiro1997]

The complete code can be downloaded here: 
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/adaptation.bngl" download="adaptation.bngl">adaptation.bngl</a>

For simplicity, we will not code up the actual methylation states, but use low (A), medium (B), high (C) to indicate the methylation states instead. 

First add methylation states, low (A), medium (B), high (C), for the ternary complex. Also add a component `r` for later introduction of CheR. Update `T` to be `T(l,r,Meth~A~B~C,Phos~U~P)`.

Then we change the receptor autophosphorylation rules to reflect what we've just discussed. 

~~~ ruby
	#Receptor complex (specifically CheA) autophosphorylation
	#Rate dependent on methylation and binding states
	#Also on free vs. bound with ligand

	TaUP: T(l,Meth~A,Phos~U) -> T(l,Meth~A,Phos~P) k_TaUnbound_phos
	TbUP: T(l,Meth~B,Phos~U) -> T(l,Meth~B,Phos~P) k_TaUnbound_phos*1.1
	TcUP: T(l,Meth~C,Phos~U) -> T(l,Meth~C,Phos~P) k_TaUnbound_phos*2.8
	TaLP: L(t!1).T(l!1,Meth~A,Phos~U) -> L(t!1).T(l!1,Meth~A,Phos~P) 0
	TbLP: L(t!1).T(l!1,Meth~B,Phos~U) -> L(t!1).T(l!1,Meth~B,Phos~P) k_TaUnbound_phos*0.8
	TcLP: L(t!1).T(l!1,Meth~C,Phos~U) -> L(t!1).T(l!1,Meth~C,Phos~P) k_TaUnbound_phos*1.6
~~~

The methylation states of the receptor complexes are modified by CheR and CheB. CheR binds to receptor complexes and methylates them; the rate of methylation is higher for ligand-bound receptors. CheB is phosphorylated by CheA in the receptor complex, and CheB-P then demethylates receptor complexes. Therefore more ligand binding leads to higher methylation states.

Add `CheB(Phos~U~P)` and `CheR(t)` to the `molecule types` section. And add reactions we just discussed to `reaction rules`.

~~~ ruby
	#CheR binds to and methylates receptor complex
	#Rate dependent on methylation states and ligand binding
	TRBind: T(r) + CheR(t) <-> T(r!2).CheR(t!2) k_TR_bind, 1
	TaRUM: T(r!2,l,Meth~A).CheR(t!2) -> T(r,l,Meth~B) + CheR(t) k_TaR_meth
	TbRUM: T(r!2,l,Meth~B).CheR(t!2) -> T(r,l,Meth~C) + CheR(t) k_TaR_meth*0.1
	TaRLM: T(r!2,l!1,Meth~A).L(t!1).CheR(t!2) -> T(r,l!1,Meth~B).L(t!1) + CheR(t) k_TaR_meth*30
	TbRLM: T(r!2,l!1,Meth~B).L(t!1).CheR(t!2) -> T(r,l!1,Meth~C).L(t!1) + CheR(t) k_TaR_meth*3
	
	#CheB is phosphorylated by receptor complex, and autodephosphorylates
	CheBp: T(Phos~P) + CheB(Phos~U) -> T(Phos~U) + CheB(Phos~P) k_B_phos
	CheBdp: CheB(Phos~P) -> CheB(Phos~U) k_B_dephos
	
	#CheB demethylates receptor complex
	#Rate dependent on methyaltion states
	TbDm: T(Meth~B) + CheB(Phos~P) -> T(Meth~A) + CheB(Phos~P) k_Tb_demeth
	TcDm: T(Meth~C) + CheB(Phos~P) -> T(Meth~B) + CheB(Phos~P) k_Tc_demeth
~~~

Now we have a complete set of reaction rules. For convenience, give all reaction rates some meaningful names.

~~~ ruby
	begin reaction rules
		LigandReceptor: L(t) + T(l) <-> L(t!1).T(l!1) k_lr_bind, k_lr_dis
		
		#Receptor complex (specifically CheA) autophosphorylation
		#Rate dependent on methylation and binding states
		#Also on free vs. bound with ligand
		TaUP: T(l,Meth~A,Phos~U) -> T(l,Meth~A,Phos~P) k_TaUnbound_phos
		TbUP: T(l,Meth~B,Phos~U) -> T(l,Meth~B,Phos~P) k_TaUnbound_phos*1.1
		TcUP: T(l,Meth~C,Phos~U) -> T(l,Meth~C,Phos~P) k_TaUnbound_phos*2.8
		TaLP: L(t!1).T(l!1,Meth~A,Phos~U) -> L(t!1).T(l!1,Meth~A,Phos~P) 0
		TbLP: L(t!1).T(l!1,Meth~B,Phos~U) -> L(t!1).T(l!1,Meth~B,Phos~P) k_TaUnbound_phos*0.8
		TcLP: L(t!1).T(l!1,Meth~C,Phos~U) -> L(t!1).T(l!1,Meth~C,Phos~P) k_TaUnbound_phos*1.6
		
		#CheY phosphorylation by T and dephosphorylation by CheZ
		YPhos: T(Phos~P) + CheY(Phos~U) -> T(Phos~U) + CheY(Phos~P) k_Y_phos
		YDephos: CheZ() + CheY(Phos~P) -> CheZ() + CheY(Phos~U) k_Y_dephos
		
		#CheR binds to and methylates receptor complex
		#Rate dependent on methylation states and ligand binding
		TRBind: T(r) + CheR(t) <-> T(r!2).CheR(t!2) k_TR_bind, 1
		TaRUM: T(r!2,l,Meth~A).CheR(t!2) -> T(r,l,Meth~B) + CheR(t) k_TaR_meth
		TbRUM: T(r!2,l,Meth~B).CheR(t!2) -> T(r,l,Meth~C) + CheR(t) k_TaR_meth*0.1
		TaRLM: T(r!2,l!1,Meth~A).L(t!1).CheR(t!2) -> T(r,l!1,Meth~B).L(t!1) + CheR(t) k_TaR_meth*30
		TbRLM: T(r!2,l!1,Meth~B).L(t!1).CheR(t!2) -> T(r,l!1,Meth~C).L(t!1) + CheR(t) k_TaR_meth*3
		
		#CheB is phosphorylated by receptor complex, and autodephosphorylates
		CheBp: T(Phos~P) + CheB(Phos~U) -> T(Phos~U) + CheB(Phos~P) k_B_phos
		CheBdp: CheB(Phos~P) -> CheB(Phos~U) k_B_dephos
		
		#CheB demethylates receptor complex
		#Rate dependent on methyaltion states
		TbDm: T(Meth~B) + CheB(Phos~P) -> T(Meth~A) + CheB(Phos~P) k_Tb_demeth
		TcDm: T(Meth~C) + CheB(Phos~P) -> T(Meth~B) + CheB(Phos~P) k_Tc_demeth
		
	end reaction rules
~~~

## Adding Compartments

In biological systems, plasma membrane separates molecules inside of the cell from the environment. In our chemotaxis system, ligand are outside of the cell, receptors and flagellar proteins are on the membrane, and CheY, CheR, CheB, CheZ are inside the cell. We will also add this compartmentalization into our model.

We define extra-cellular spaces, plasma membrane, and cytoplasm. Here, each row indicates 1) name of the compartment, 2) dimension (2D or 3D), 3) surface area or volumn of the compartment, 4) the name of the parent compartment. More information on compartmentalization can be found page 54-55 [here](http://www.lehman.edu/academics/cmacs/documents/RuleBasedPrimer-2011.pdf).

~~~ ruby
	begin compartments
		EC  3  100       #um^3
		PM  2  1   EC    #um^2
		CP  3  1   PM    #um^3
	end compartments
~~~

## Specifying concentrations and reaction rates

We need to add the compartmentalization information in the `seed species`. Also update the initial concentrations of molecules at different states. The distribution of molecules are each state is very difficult to experimentally verify. The distribution provided here approximates equilibrium concentrations in our simulation, and they are within a biologically reasonable range.[^Bray1993]

~~~ ruby
	begin seed species
		@EC:L(t) L0
		@PM:T(l,r,Meth~A,Phos~U) T0*0.84*0.9
		@PM:T(l,r,Meth~B,Phos~U) T0*0.15*0.9
		@PM:T(l,r,Meth~C,Phos~U) T0*0.01*0.9
		@PM:T(l,r,Meth~A,Phos~P) T0*0.84*0.1
		@PM:T(l,r,Meth~B,Phos~P) T0*0.15*0.1
		@PM:T(l,r,Meth~C,Phos~P) T0*0.01*0.1
		@CP:CheY(Phos~U) CheY0*0.71
		@CP:CheY(Phos~P) CheY0*0.29
		@CP:CheZ() CheZ0
		@CP:CheB(Phos~U) CheB0*0.62
		@CP:CheB(Phos~P) CheB0*0.38
		@CP:CheR(t) CheR0
	end seed species

Specify all the molecules types you want to observe for in the `observable` section.

	begin observables
		Molecules bound_ligand L(t!1).T(l!1)
		Molecules phosphorylated_CheY CheY(Phos~P)
		Molecules low_methyl_receptor T(Meth~A)
		Molecules medium_methyl_receptor T(Meth~B)
		Molecules high_methyl_receptor T(Meth~C)
		Molecules phosphorylated_CheB CheB(Phos~P)
	end observables

And the last thing is to assign values to the parameters. Let's start with no ligand is added to the system. We assign the initial number for each molecule and reaction rates based on *in vivo* stoichiometry and parameter tuning [^1][^Li2004][^Stock1991]. Specifically, we will add number of CheR, CheB, the state-dependency of receptor complex autophosphorylation, reaction rates for receptor-CheR binding/dissociation, rates of receptor complex methylation and demethylation.

	begin parameters
		NaV 6.02e8   #Unit conversion to cellular concentration M/L -> #/um^3
		miu 1e-6
		
		L0 0             #number of molecules/cell
		T0 7000          #number of molecules/cell
		CheY0 20000      #number of molecules/cell
		CheZ0 6000       #number of molecules/cell
		CheR0 120        #number of molecules/cell
		CheB0 250        #number of molecules/cell
		
		k_lr_bind 8.8e6/NaV    #ligand-receptor binding
		k_lr_dis 35            #ligand-receptor dissociation
		
		k_TaUnbound_phos 7.5   #receptor complex autophosphorylation
		
		k_Y_phos 3.8e6/NaV     #receptor complex phosphorylates Y
		k_Y_dephos 8.6e5/NaV   #Z dephosphoryaltes Y
		
		k_TR_bind  2e7/NaV     #Receptor-CheR binding
		k_TR_dis   1           #Receptor-CheR dissociation
		k_TaR_meth 0.08        #CheR methylates receptor complex
		
		k_B_phos 1e5/NaV       #CheB phosphorylation by receptor complex
		k_B_dephos 0.17        #CheB autodephosphorylation
		
		k_Tb_demeth 5e4/NaV    #CheB demethylates receptor complex
		k_Tc_demeth 2e4/NaV    #CheB demethylates receptor complex
	end parameters
~~~

The complete code can be downloaded here: 
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/adaptation.bngl" download="adaptation.bngl">adaptation.bngl</a>.

## Adaptation: CheY returns to equilibrium

Fantastic, we have the model now.

Now run the simulation with `simulate({method=>"ssa", t_end=>800, n_steps=>800})`. You should be able to observe none of the observable change in concentration with no ligand present.

![image-center](../assets/images/chemotaxis_tutorial_oneadd0.png){: .align-center}

Run simulation with `L0 = 1e6`. What happens to CheY activity? What happens to methylation states?

You will observe CheY acitvity drops immediately, and returns to the original state gradually. You will also see when the system reaches steady state, the there are more highly methlyated receptors, and less weakly methylated receptors.

Try higher concentrations (L0 = 1e4, 1e5, 1e6, 1e7, 1e8), highlight the line showing CheY-P. What's the general trend? How does the change depend on ligand concentration?

Also try only simulate the first 10 seconds to zoom into what happens to the system there. When does CheY activities reach minimum?

That suggests ligand binding can lead to a very quick response (within 1s), and the cell slowly adapts to the concentration and returns to the background tumbling frequency in several minutes.


[^Spiro1997]: Spiro PA, Parkinson JS, and Othmer H. 1997. A model of excitation and adaptation in bacterial chemotaxis. Biochemistry 94:7263-7268. [Available online](https://www.pnas.org/content/94/14/7263).

[^Bray1993]: Bray D, Bourret RB, Simon MI. 1993. Computer simulation of phosphorylation cascade controlling bacterial chemotaxis. Molecular Biology of the Cell. [Available online](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC300951/)

[^Li2004]: Li M, Hazelbauer GL. 2004. Cellular stoichimetry of the components of the chemotaxis signaling complex. Journal of Bacteriology. [Available online](https://jb.asm.org/content/186/12/3687)

[^Stock1991]: Stock J, Lukat GS. 1991. Intracellular signal transduction networks. Annual Review of Biophysics and Biophysical Chemistry. [Available online](https://www.annualreviews.org/doi/abs/10.1146/annurev.bb.20.060191.000545)

[^Shimizu2005]: Shimizu TS, Delalez N, Pichler K, and Berg HC. 2005. Monitoring bacterial chemotaxis by using bioluminescence resonance energy transfer: absence of feedback from the flagellar motors. PNAS. [Available online](https://www.pnas.org/content/103/7/2093/)

[^Krembel2015]: Krembel A., Colin R., Sourijik V. 2015. Importance of multiple methylation sites in *Escherichia coli* chemotaxis. [Available online](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0145582)


[Back to Main Text](home_senseadap){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}




