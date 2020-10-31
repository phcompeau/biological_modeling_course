---
permalink: /chemotaxis/tutorial_gradient
title: "Software Tutorial: Traveling Up an Attractant Gradient"
sidebar:
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

In the [previous tutorial](tutorial_adap), we modeled how bacteria react and adapt to a one-time addition of attractants. In real life, bacteria don't suddenly drop into an environment with more attractants; instead, they explore a variable environment. In this tutorial, we will adapt our model to simulate a bacterium as it travels up an exponentially increasing concentration gradient.

We will also explore defining and using **functions**, a feature of BioNetGen that will allow us to specify reaction rules in which the reaction rates are dependent on the current state of the system.

To get started, create a copy of your file from the adaptation tutorial and save it as `addition.bngl`. If you would rather not follow along below, you can download a completed BioNetGen file here: <a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/addition.bngl" download="addition.bngl">addition.bngl</a>

We also will build a Jupyter notebook in this tutorial. You should create a file called `plotter_up.ipynb`; if you would rather not follow along, we provide a completed notebook here:
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/plotter_up.ipynb" download="plotter_up.ipynb">plotter_up.ipynb</a>

Before running this notebook, make sure the following dependencies are installed.

| Installation Link | Version | Check install/version |
|:------|:-----:|------:|
| [Python3](https://www.python.org/downloads/)  |3.6+ |`python --version` |
| [Jupyter Notebook](https://jupyter.org/index.html) | 4.4.0+ | `jupyter --version` |
| [Numpy](https://numpy.org/install/) | 1.14.5+ | `pip list \| grep numpy` |
| [Matplotlib](https://matplotlib.org/users/installing.html) | 3.0+ | `pip list \| grep matplotlib` |
| [Colorspace](https://python-colorspace.readthedocs.io/en/latest/installation.html) or with [pip](https://pypi.org/project/colorspace/)| any | `pip list \| grep colorspace`|

## Modeling traveling up a gradient with BioNetGen

To mimic the ligand concentration change of bacteria moving up the gradient, we will gradually increase the ligand concentration in the environment, simulating the bacteria moving up the attractant gradient.

First create a copy of the adaptation model `adaptation.bngl`, name it `addition.bngl`.

We will simulate this increase in attractant concentration simply with a "fake reaction" that one ligand molecule becomes two through time. We will add the following reaction to the `reaction rules` section.

~~~ ruby
#Simulate exponentially increasing gradient
LAdd: L(t) -> L(t) + L(t) k_add
~~~

Like you've observed before, when ligand concentration is very high, the receptors are saturated, so the cell can no longer detect the change in ligand concentration. We can use this fact to cap our ligand concentration at `1e8` (in the [adaptation simulation](tutorial_adap), the cell can't differentiate between `1e8` and a higher concentration). We can do this by defining the rate of this reaction as a function `add_Rate()`. It requires another observable, `AllLigand`. By adding the line `Molecules AllLigand L` in the `observables` sections, `AllLigand` will record the total concentration of ligands in the system at each time step. When `AllLigand` is more than `1e8`, the rate of ligand concentration increase becomes 0. In the `if` statement, the syntax is `if(condition,valueTrue,valueFalse)`. Please add functions **before declaring reaction rules**.

~~~ ruby
begin functions
	addRate() = if(AllLigand>1e8,0,k_add)
end functions
~~~

Substitute `k_add` in `reaction rules` with `addRate()`

~~~ ruby
LAdd: L(t) -> L(t) + L(t) addRate()
~~~

In `parameters` section, we define the rate of ligand increase. We will try a reaction rate 0.1/s first with initial concentration 1e4. So the actual gradient the cell experiences is d[L]/dt = 0.1[L]. By integration then differentiation, we get [L] = 1000e<sup>0.1t</sup> molecules per second. Change initial ligand amount to 1e4 (too many/few ligands makes increase in ligand concentration too fast/slow), but you can try other values too.

~~~ ruby
k_add 0.1
L0 1e4
~~~

## Simulating response when moving up the gradient

Place the following after `end model` and you are ready to simulate. This time we simulate over 1000s.

~~~ ruby
generate_network({overwrite=>1})
simulate({method=>"ssa", t_end=>1000, n_steps=>500})
~~~

The following code contains our complete simulation, which you can also download here:
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/addition.bngl" download="addition.bngl">addition.bngl</a>

~~~ ruby
begin model

begin molecule types
	L(t)
	T(l,r,Meth~A~B~C,Phos~U~P)
	CheY(Phos~U~P)
	CheZ()
	CheB(Phos~U~P)
	CheR(t)
end molecule types

begin observables
	Molecules bound_ligand L(t!1).T(l!1)
	Molecules phosphorylated_CheY CheY(Phos~P)
	Molecules low_methyl_receptor T(Meth~A)
	Molecules medium_methyl_receptor T(Meth~B)
	Molecules high_methyl_receptor T(Meth~C)
	Molecules phosphorylated_CheB CheB(Phos~P)
	Molecules AllLigand L
end observables

begin parameters
	NaV2 6.02e8   #Unit conversion to cellular concentration M/L -> #/um^3
	miu 1e-6

	L0 1e4
	T0 7000
	CheY0 20000
	CheZ0 6000
	CheR0 120
	CheB0 250

	k_lr_bind 8.8e6/NaV2   #ligand-receptor binding
	k_lr_dis 35            #ligand-receptor dissociation

	k_TaUnbound_phos 7.5   #receptor complex autophosphorylation

	k_Y_phos 3.8e6/NaV2    #receptor complex phosphorylates Y
	k_Y_dephos 8.6e5/NaV2  #Z dephosphoryaltes Y

	k_TR_bind 2e7/NaV2          #Receptor-CheR binding
	k_TR_dis  1            #Receptor-CheR dissociaton
	k_TaR_meth 0.08        #CheR methylates receptor complex

	k_B_phos 1e5/NaV2      #CheB phosphorylation by receptor complex
	k_B_dephos 0.17        #CheB autodephosphorylation

	k_Tb_demeth 5e4/NaV2   #CheB demethylates receptor complex
	k_Tc_demeth 2e4/NaV2   #CheB demethylates receptor complex

	k_add 0.1              #Ligand increase

end parameters

begin functions
	addRate() = if(AllLigand>1e8,0,k_add)
end functions

begin reaction rules
	LigandReceptor: L(t) + T(l) <-> L(t!1).T(l!1) k_lr_bind, k_lr_dis

	#Receptor complex (specifically CheA) autophosphorylation
	#Rate dependent on methylation and binding states
	#Also on free vs. bound with ligand
	TaUnboundP: T(l,Meth~A,Phos~U) -> T(l,Meth~A,Phos~P) k_TaUnbound_phos
	TbUnboundP: T(l,Meth~B,Phos~U) -> T(l,Meth~B,Phos~P) k_TaUnbound_phos*1.1
	TcUnboundP: T(l,Meth~C,Phos~U) -> T(l,Meth~C,Phos~P) k_TaUnbound_phos*2.8
	TaLigandP: L(t!1).T(l!1,Meth~A,Phos~U) -> L(t!1).T(l!1,Meth~A,Phos~P) 0
	TbLigandP: L(t!1).T(l!1,Meth~B,Phos~U) -> L(t!1).T(l!1,Meth~B,Phos~P) k_TaUnbound_phos*0.8
	TcLigandP: L(t!1).T(l!1,Meth~C,Phos~U) -> L(t!1).T(l!1,Meth~C,Phos~P) k_TaUnbound_phos*1.6

	#CheY phosphorylation by T and dephosphorylation by CheZ
	YPhos: T(Phos~P) + CheY(Phos~U) -> T(Phos~U) + CheY(Phos~P) k_Y_phos
	YDephos: CheZ() + CheY(Phos~P) -> CheZ() + CheY(Phos~U) k_Y_dephos

	#CheR binds to and methylates receptor complex
	#Rate dependent on methylation states and ligand binding
	TRBind: T(r) + CheR(t) <-> T(r!2).CheR(t!2) k_TR_bind, k_TR_dis
	TaRUnboundMeth: T(r!2,l,Meth~A).CheR(t!2) -> T(r,l,Meth~B) + CheR(t) k_TaR_meth
	TbRUnboundMeth: T(r!2,l,Meth~B).CheR(t!2) -> T(r,l,Meth~C) + CheR(t) k_TaR_meth*0.1
	TaRLigandMeth: T(r!2,l!1,Meth~A).L(t!1).CheR(t!2) -> T(r,l!1,Meth~B).L(t!1) + CheR(t) k_TaR_meth*30
	TbRLigandMeth: T(r!2,l!1,Meth~B).L(t!1).CheR(t!2) -> T(r,l!1,Meth~C).L(t!1) + CheR(t) k_TaR_meth*3

	#CheB is phosphorylated by receptor complex, and autodephosphorylates
	CheBphos: T(Phos~P) + CheB(Phos~U) -> T(Phos~U) + CheB(Phos~P) k_B_phos
	CheBdephos: CheB(Phos~P) -> CheB(Phos~U) k_B_dephos

	#CheB demethylates receptor complex
	#Rate dependent on methyaltion states
	TbDemeth: T(Meth~B) + CheB(Phos~P) -> T(Meth~A) + CheB(Phos~P) k_Tb_demeth
	TcDemeth: T(Meth~C) + CheB(Phos~P) -> T(Meth~B) + CheB(Phos~P) k_Tc_demeth

	#Simulate exponentially increasing gradient
	LAdd: L(t) -> L(t) + L(t) addRate()

end reaction rules

begin compartments
  EC  3  100       #um^3
  PM  2  1   EC    #um^2
  CP  3  1   PM    #um^3
end compartments

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

end model

generate_network({overwrite=>1})
simulate({method=>"ssa", t_end=>1000, n_steps=>500})
~~~

Go to `simulation` and click `Run`. What happens to CheY phosphorylation? (Note: you can deselect `AllLigand` to make the plots clearer for phosphorylated CheY)

You will observe that CheY phosphorylation drops gradually first, instead of the instantaneous sharp drop as we add lots of ligand at once. That means, with the ligand concentration increases, the cell is able to continuously lower the tumbling frequency.

Try different values for `k_add`: 0.01, 0.03, 0.05, 0.1, 0.3, 0.5. What do different `k_add` values imply? How does the system respond to the different values - what are some common trends and some differences?

All simulation results are stored in the `RuleBender-workspace/PROJECT_NAME/results/MODEL_NAME/TIME/` directory in your computer. Rename the directory with the `k_add` values instead of the time of running for simplicity.

<!--
Please make sure have dependencies installed:
 - [Jupyter Notebook](https://jupyter.org/index.html)
 - [Python3](https://www.python.org/downloads/), version 3.6+
 - [Numpy](https://numpy.org/install/)
 - [Matplotlib](https://matplotlib.org/users/installing.html)
 - [Colorspace](https://python-colorspace.readthedocs.io/en/latest/installation.html) (simply [install with pip](https://pypi.org/project/colorspace/) works too)
-->

## Visualizing the results

We will use the jupyter notebook <a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/plotter_up.ipynb" download="plotter_up.ipynb">plotter_up.ipynb</a> to visualize results. First specify the directories, model name, species of interest, and rates. Put the `RuleBender-workspace/PROJECT_NAME/results/MODEL_NAME/` folder inside the same directory as the Jupyter notebook or change the `model_path`.

~~~ python
model_path = "addition"  #The folder containing the model
model_name = "addition"  #Name of the model
target = "phosphorylated CheY"    #Target molecule
vals = [0.01, 0.03, 0.05, 0.1, 0.3, 0.5]  #Gradients of interest
~~~

The second code block will load simulation result at each time point from the `.gdat` file, which stores concentration of all `observables` at all steps, and plot concentration of phosphorylated CheY through time.

Run the code blocks. How does `k_add` impact the CheY-P concentrations? Why? Are the tumbling frequencies restored to the background frequency?


[Return to main text](home_gradient#steady-state-tumbling-frequency-is-robust-when-traveling-up-an-attractant-gradient){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
