---
permalink: /chemotaxis/tutorial_gradient
title: "Up gradient/Addition"
sidebar: 
 nav: "chemotaxis"
---

In this page, we will:
 - Simulate cellular response when traveling up the gradient.

## Files and dependencies 

The simulation can be downloaded here: <a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/addition.bngl" download="addition.bngl">addition.bngl</a>

The Jupyter notebook for visualizing results can be downloaded here: 
<a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/plotter_up.ipynb" download="plotter_up.ipynb">plotter_up.ipynb</a>

Please make sure the following dependencies are installed.

| Installation Link | Version | Check install/version |
|:------|:-----:|------:|
| [Python3](https://www.python.org/downloads/)  |3.6+ |`python --version` |
| [Jupyter Notebook](https://jupyter.org/index.html) | 4.4.0+ | `jupyter --version` |
| [Numpy](https://numpy.org/install/) | 1.14.5+ | `pip list | grep numpy` |
| [Matplotlib](https://matplotlib.org/users/installing.html) | 3.0+ | `pip list | grep matplotlib` |
| [Colorspace](https://python-colorspace.readthedocs.io/en/latest/installation.html) or with [pip](https://pypi.org/project/colorspace/)| any | `pip list | grep colorspace`|

## Modeling traveling up the gradient with BNG

We've built a model simulating the response of *E. coli* in response to a one-time addition of attractant. However, in real life, the bacterium doesn't suddenly drop into an environment with more attractants; instead, it searches the space to find the gradient. To mimic this phenomenon, we will gradually increase the ligand concentration in the environment, simulating the bacteria moving up the attractant gradient.

First create a copy of the adaptation model `adaptation.bngl`, name it `addition.bngl`.

We will simulate this increase in attractant concentration simply with a "fake reaction" that one ligand molecule becomes two through time. We will add the following reaction to the `reaction rules` section.

~~~ ruby
	#Simulate exponentially increasing gradient
	LAdd: L(t) -> L(t) + L(t) k_add
~~~

Like you've observed before, when ligand concentration is very high, the receptors are saturated, so the cell can no longer detect the change in ligand concentration. We can use this fact to cap our ligand concentration at `1e8` (in the [adaptation simulation](tutorial_adap), the cell can't differentiate between `1e8` and a higher concentration). We can do this by defining the rate of this reaction as a function `add_Rate()`. It requires another observable, `AllLigand`. By adding the line `Molecules AllLigand L` in the `observables` sections, `AllLigand` will record the total concentration of ligands in the system at each time step. When `AllLigand` is more than `1e8`, the rate of ligand concentration increase becomes 0. In the `if` statement, the syntax is `if(condition,valueTrue,valueFalse)`.

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

Set `simulate({method=>"ssa", t_end=>1000, n_steps=>500})`. Go to `simulation` and click `Run`. What happens to CheY phosphorylation? (Note: you can deselect `AllLigand` to make the plots clearer for phosphorylated CheY)

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


[Back to Main Text](home_gradient){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}




