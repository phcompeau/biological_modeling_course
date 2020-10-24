---
permalink: /chemotaxis/tutorial_purerandom
title: "Standard random walk"
sidebar:
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

In this page, we will:
 - Simulate *E. coli* chemotaxis at a cellular level by standard random walk

## Simulation files and dependencies

Please download the simulation and visualization here: <a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/chemotaxis_std_random.ipynb" download="chemotaxis_std_random.ipynb">chemotaxis_std_random.ipynb</a>. Detailed explanation of the model and each functions can be found in the file too.

Please make sure the following dependencies are installed:

| Installation Link | Version | Check install/version |
|:------|:-----:|------:|
| [Python3](https://www.python.org/downloads/)  |3.6+ |`python --version` |
| [Jupyter Notebook](https://jupyter.org/index.html) | 4.4.0+ | `jupyter --version` |
| [Numpy](https://numpy.org/install/) | 1.14.5+ | `pip list | grep numpy` |
| [Matplotlib](https://matplotlib.org/users/installing.html) | 3.0+ | `pip list | grep matplotlib` |
| [Colorspace](https://python-colorspace.readthedocs.io/en/latest/installation.html) or with [pip](https://pypi.org/project/colorspace/)| any | `pip list | grep colorspace`|

## Modeling standard random walk at a cellular level

Our model will be based on observations from BNG simulation and *E. coli* biology.

Ingredients and simplifying assumptions of the model:
1. Run. The duration of run follows an exponential distrubtion with mean equals to the background run duration `time_exp`.
2. Tumble. The duration of cell tumble follows an exponential distribution with mean 0.1s. When it tumbles, we assume it only changes the orientation for the next run but doesn't move in space. The degree of reorientation follows uniform distribution from 0° to 360°.
3. Gradient. We model an exponential gradient centered at [1500, 1500] with a concentration of 10<sup>8</sup>. All cells start at [0, 0], which has a concentration of 10<sup>2</sup>. The receptors saturate at a concentration of 10<sup>8</sup>.
4. Performance. The closer to the center of the gradient the better.

First import all dependencies.

~~~ python
import numpy as np
import matplotlib.pyplot as plt
import math
from matplotlib import colors
from matplotlib import patches
import colorspace
~~~

Then we will specify all the model parameters, including mean tumble time 0.1s[^Saragosti2012], mean 68° and std 36° for reorientation[^Berg1972], cell speed of 20µm/s[^Baker2005], and parameters for our ligand concentration gradient. Set a seed for random numbers for reproducibility.

~~~ python
SEED = 128  #Any random seed
np.random.seed(SEED)

#Constants for E.coli tumbling
tumble_time_mu = 0.1

#E.coli movement constants
speed = 20         #um/s, speed of E.coli movement

#Model constants
start = [0, 0]  #All cells start at [0, 0]
ligand_center = [1500, 1500] #Position of highest concentration
center_exponent, start_exponent = 8, 2
origin_to_center = 0 #Distance from start to center, intialized here, will be actually calculated later
saturation_conc = 10 ** 8 #From BNG model
~~~

For each point on our 2D plane, the concentration can be calculated with an exponential distribution centered at `ligand_center = [1500, 1500]` with concentration = 1e8; origin `start = [0, 0]` has concentration 1e2. The concentration of ligands [*L*] grows exponentially from (0, 0) to (1500, 1500), such that [*L*] = 100 · 10<sup>(1-*d*/*D*) · 8</sup>, where *d* is the Euclidean distance from the current point to (1500, 1500), and *D* is the Euclidean distance from (0, 0) to (1500, 1500).

~~~ python
# Calculates Euclidean distance between point a and b
def euclidean_distance(a, b):
    return math.sqrt((a[0] - b[0]) ** 2 + (a[1] - b[1]) ** 2)
# Exponential gradient, the exponent follows a linear relationship with distance to center
def calc_concentration(pos):
    dist = euclidean_distance(pos, ligand_center)
    exponent = (1 - dist / origin_to_center) * (center_exponent - start_exponent) + start_exponent

    return 10 ** exponent
~~~

The duration of cell tumble follows an exponential distribution with mean 0.1s[^Saragosti2012]. When it tumbles, we assume it only changes the orientation for the next run but doesn't move in space. The degree of reorientation follows a uniform distribution with mean 68 and std 36[^Berg1972]. Also return the horizontal and vertical movement of the cell for the following run.

~~~ python
# Horizontal and Vertical movement of tumbling
def tumble_move(curr_dir):
    #Sample the new direction
    new_dir = np.random.uniform(low = 0.0, high = 2 * math.pi)
    new_dir += curr_dir

    if new_dir > 2 * math.pi:
        new_dir -= 2 * math.pi #Keep within [0, 2pi] for cleaner numbers

    move_h = math.cos(new_dir) #Horizontal displacement for next run
    move_v = math.sin(new_dir) #Vertical displacement for next run

    tumble_time = np.random.exponential(tumble_time_mu) #Length of the tumbling

    return new_dir, move_h, move_v, tumble_time
~~~

For each cell, simulate through time as the following procedure:

while `t` < duration:
 - Sample the current run duration `curr_run_time` from an exponential distribution with mean `time_exp`
 - Run for `curr_run_time` second along current direction
 - Sample the duration of tumble `tumble_time` and the resulted direction
 - increment t by `curr_run_time` and `tumble_time`

~~~ python
def simulate_std_random(num_cells, duration, time_exp):

    path = np.zeros((num_cells, duration + 1, 2))
    terminals = []

    for rep in range(num_cells):
        # Initialize simulation
        t = 0
        curr_position = np.array(start)
        curr_direction, move_h, move_v, tumble_time = tumble_move(0) #Initialize direction randomly
        past_sec = 0

        while t < duration:

            curr_run_time = np.random.exponential(time_exp) #get wait time before tumble

            curr_position = curr_position + np.array([move_h, move_v]) * speed * curr_run_time
            curr_direction, move_h, move_v, tumble_time = tumble_move(curr_direction)
            t += (curr_run_time + tumble_time)

            #record position approximate for integer number of second
            curr_sec = int(t)
            for sec in range(past_sec, min(curr_sec, duration) + 1):
                path[rep, sec] = curr_position.copy()
                past_sec= curr_sec

        terminals.append((path[rep, -1]))

    return terminals, path
~~~

## Visualizing trajectories

Run simulation for 3 cells for 500 seconds to get a rough idea of what their trajectories look like.

~~~ python
duration = 800   #seconds, duration of the simulation
num_cells = 3
origin_to_center = euclidean_distance(start, ligand_center) #Update the global constant
time_exp = 1.0

terminals, path = simulat_std_random(num_cells, duration, time_exp)
print(terminals)
~~~

Run the two code blocks for Part2: Visualizing trajectories (1st block simulates, 2nd block is plotter). The background color indicates concentration: white -> red = low -> high; black dot are starting points; red dots are the points they reached at the end of the simulation; colorful small dots represents trajectories (one color one cell): dark -> bright color = older -> newer time points; blue cross indicates the highest concentration [1500, 1500].

**STOP:** What do you observe? Are the cells moving up the gradient? Is this a good strategy to search for food?
{: .notice--primary}

## Measure performance quantitatively

Although the cells are under the same mechanisms for random walk, randomness introduced large variations among the trajectories. Therefore, simulation with 3 cells is not convincing. To assess the performances, let's simulate with 500 cells for 1500 seconds.

For 500 cells, visualizing the trajectories will be messy. We will quantitatively measure the performances by the ability to reach the target at the end of the simulation. We will calculate the average distance to the center at each time step.

~~~ python
#Run simulation for 500 cells, plot average distance to highest concentration point.
duration = 1500   #seconds, duration of the simulation
num_cells = 500
time_exp = 1.0
origin_to_center = euclidean_distance(start, ligand_center) #Update the global constant
radius_saturation = (1 - ((math.log10(saturation_conc) - start_exponent) / (center_exponent - start_exponent))) * origin_to_center

all_distance = np.zeros((num_cells, duration)) #Initialize to store results

terminals, paths = simulate_std_random(num_cells, duration, time_exp) #run simulation

for c in range(num_cells):
    for t in range(duration):
        pos = paths[c, t]
        dist = euclidean_distance(ligand_center, pos)
        all_distance[c, t] = dist

all_dist_avg = np.mean(all_distance, axis = 0)
all_dist_std = np.std(all_distance, axis = 0)
~~~

**STOP:** Before visualizing the average distances at each time step, what do you expect the result to be (based on the trajectories)?
{: .notice--primary}

Run the two code blocks for Part3: Measuring collective performances (1st block simulates, 2nd block is plotter). The colored line indicates average distance of the 500 cells; the shaded area is standard deviation; grey dashed line is where concentration reaches 1e8; blue cross indicates the goal [1500, 1500].

What do you conclude about their performances?


[^Saragosti2012]: Saragosti J., Siberzan P., Buguin A. 2012. Modeling *E. coli* tumbles by rotational diffusion. Implications for chemotaxis. PLoS One 7(4):e35412. [available online](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3329434/).

[^Berg1972]: Berg HC, Brown DA. 1972. Chemotaxis in Escherichia coli analysed by three-dimensional tracking. Nature. [Available online](https://www.nature.com/articles/239500a0)

[^Baker2005]: Baker MD, Wolanin PM, Stock JB. 2005. Signal transduction in bacterial chemotaxis. BioEssays 28:9-22. [Available online](https://pubmed.ncbi.nlm.nih.gov/16369945/)


[Return to main text](home_conclusion#strategy-2-chemotactic-random-walk){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
