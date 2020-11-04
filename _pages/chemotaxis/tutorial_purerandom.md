---
permalink: /chemotaxis/tutorial_purerandom
title: "Software Tutorial: Modeling a Pure Random Walk Strategy"
sidebar:
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

In this tutorial, we will simulate a random walk and take a look at how well this allows a bacterium to reach a goal. We might not anticipate that the random walk will do a very good job of this -- which is correct -- but it will give us a baseline simple strategy to compare a more advanced random walk strategy against.

Specifically, we will build a Jupyter notebook to do so. You can create a blank file called `chemotaxis_std_random.ipynb` and type along, but the notebook will be quite lengthy, so feel free to download the final notebook here if you like: <a href="../downloads/downloadable/chemotaxis_std_random.ipynb" download="chemotaxis_std_random.ipynb">chemotaxis_std_random.ipynb</a>. A detailed explanation of the model and each function can be found in this completed file as well as the tutorial below.

Please make sure the following dependencies are installed:

| Installation Link | Version | Check install/version |
|:------|:-----:|------:|
| [Python3](https://www.python.org/downloads/)  |3.6+ |`python --version` |
| [Jupyter Notebook](https://jupyter.org/index.html) | 4.4.0+ | `jupyter --version` |
| [Numpy](https://numpy.org/install/) | 1.14.5+ | `pip list | grep numpy` |
| [Matplotlib](https://matplotlib.org/users/installing.html) | 3.0+ | `pip list | grep matplotlib` |
| [Colorspace](https://python-colorspace.readthedocs.io/en/latest/installation.html) or with [pip](https://pypi.org/project/colorspace/)| any | `pip list | grep colorspace`|

## Converting a run-and-tumble model to a random walk simulation

Our model will be based on observations from our BioNetGen simulation and known biology of *E. coli*. We summarize this simulation, discussed in the main text, as follows.

1. **Run.** The duration of run follows an exponential distribution with mean equal to the background run duration `time_exp`.
2. **Tumble.** The duration of cell tumble follows an exponential distribution with mean 0.1s. When it tumbles, we assume it only changes the orientation for the next run but doesn't move in space. The degree of reorientation is a random number sampled uniformly between 0° and 360°.
3. **Gradient.** We model an exponential gradient with a goal (1500, 1500) having a concentration of 10<sup>8</sup>. All cells start at the origin (0, 0), which has a concentration of 10<sup>2</sup>. The ligand concentration at a point (*x*, *y*) is given by *L*(*x*, *y*) = 100 · 10<sup>8 · (1-*d*/*D*)</sup>, where *d* is the distance from (*x*, *y*) to the goal, and *D* is the distance from the origin to the goal; in this case, *D* is approximately equal to 2121.32.

First, we will import all packages needed.

~~~ python
import numpy as np
import matplotlib.pyplot as plt
import math
from matplotlib import colors
from matplotlib import patches
import colorspace
~~~

Next, we specify model parameters:

* mean tumble time: 0.1s[^Saragosti2012];
* cell speed of 20µm/s[^Baker2005].

We also set a "seed" of our pseudorandom number generator. This ensures that the sequence of "random" numbers will be the same every time we run the simulation. We can change the seed to obtain a different outcome. For more on seeding, please consult the discussion of pseudorandom number generation at [Programming for Lovers](http://compeau.cbd.cmu.edu/programming-for-lovers/chapter-2-forecasting-a-presidential-election-with-monte-carlo-simulation/#pitfalls).

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

We now will have two functions that will establish the ligand concentration at a given point (*x*, *y*) as equal to *L*(*x*, *y*) = 100 · 10<sup>8 · (1-*d*/*D*)</sup>.

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

The duration of a cell tumble follows an exponential distribution with mean equal to 0.1s[^Saragosti2012]. When it tumbles, we assume that it only changes the orientation for the next run but does not move in space. The degree of reorientation is chosen uniformly between 0 and 2π radians (i.e., 0 and 360 degrees).

The following `tumble_move` function takes the current direction of movement, represented as an angle in radians between 0 and 2π, and uses this direction to determine a new direction of movement according to the rule above.

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

In a given run of the simulation, we keep track of the total time `t`, and we only continue our simulation if `t` < `duration`, where `duration` is a parameter indicating how long to run the simulation. If `t` < `duration`, then we apply the following steps to a given cell.

 - Sample the run duration `curr_run_time` from an exponential distribution with mean `time_exp`;
 - run for `curr_run_time` seconds in the current direction;
 - sample the duration of tumble `tumble_time`;
 - determine the new direction of the simulated bacterium by calling the `tumble_move` function discussed above;
 - increment t by `curr_run_time` and `tumble_time`.

These steps are achieved by the `simulate_std_random` function below, which takes the number of cells `num_cells` to simulate, the time to run each simulation for `duration`, and the mean time of a single run `time_exp`. This function stores the end points of the simulation for each cell in a variable named `terminals` and the trajectories of these cells in a variable named `path`.

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

Now that we have established parameters and written the functions that we will need, we will run our simulation with `num_cells` equal to 3 and `duration` equal to 500 in order to get a rough idea of what the trajectories of our simulated cells will look like.

~~~ python
duration = 800   #seconds, duration of the simulation
num_cells = 3
origin_to_center = euclidean_distance(start, ligand_center) #Update the global constant
time_exp = 1.0

terminals, path = simulat_std_random(num_cells, duration, time_exp)
print(terminals)
~~~

## Visualizing simulated cell trajectories

Now that we have generated the data of our randomly walking cells, our next step is to plot these trajectories using Matplotlib. We will color-code the background ligand concentration. The ligand concentrations at each position can be represented using a matrix, and we take the logarithm of each value of this matrix to better color our exponential gradient. That is, a value of 10<sup>8</sup> will be converted to 8, and a value of 10<sup>4</sup> will be converted to 4. A white background color will indicate a low ligand concentration, while red indicates high concentration.

~~~ python
fig, ax = plt.subplots(1, 1, figsize = (8, 8))

#First set color map
mycolor = [[256, 256, 256], [256, 255, 254], [256, 253, 250], [256, 250, 240], [255, 236, 209], [255, 218, 185], [251, 196, 171], [248, 173, 157], [244, 151, 142], [240, 128, 128]] #from coolors：）
for i in mycolor:
    for j in range(len(i)):
        i[j] *= (1/256)
cmap_color = colors.LinearSegmentedColormap.from_list('my_list', mycolor)


conc_matrix = np.zeros((4000, 4000))
for i in range(4000):
    for j in range(4000):
        conc_matrix[i][j] = math.log(calc_concentration([i - 1000, j - 1000]))

#Simulate the gradient distribution, plot as a heatmap
ax.imshow(conc_matrix.T, cmap=cmap_color, interpolation='nearest', extent = [-1000, 3000, -1000, 3000], origin = 'lower')
~~~

Next, we plot each cell's trajectory over each of its tumbling points. To visualize older vs. newer time points, we set the color as a function of `t` so that newer points have lighter colors.

~~~ python
#Plot simulation results
time_frac = 1.0 / duration
#Time progress: dark -> colorful
for t in range(duration):
    ax.plot(path[0,t,0], path[0,t,1], 'o', markersize = 1, color = (0.2 * time_frac * t, 0.85 * time_frac * t, 0.8 * time_frac * t))
    ax.plot(path[1,t,0], path[1,t,1], 'o', markersize = 1, color = (0.85 * time_frac * t, 0.2 * time_frac * t, 0.9 * time_frac * t))
    ax.plot(path[2,t,0], path[2,t,1], 'o', markersize = 1, color = (0.4 * time_frac * t, 0.85 * time_frac * t, 0.1 * time_frac * t))
ax.plot(start[0], start[1], 'ko', markersize = 8)
~~~

We mark the starting point of each cell's trajectory with a black dot and the ending point of the trajectory with a red dot. We place a blue cross over the goal. Finally, we set axis limits, assign axis labels, and generate the plot.

~~~ python
for i in range(num_cells):
    ax.plot(terminals[i][0], terminals[i][1], 'ro', markersize = 8)

ax.plot(1500, 1500, 'bX', markersize = 8)
ax.set_title("Pure random walk \n Background: avg tumble every {} s".format(time_exp), x = 0.5, y = 0.87)
ax.set_xlim(-1000, 3000)
ax.set_ylim(-1000, 3000)
ax.set_xlabel("poisiton in um")
ax.set_ylabel("poisiton in um")

plt.show()
~~~

**STOP:** Run the notebook. What do you observe? Are the cells moving up the gradient? Is this a good strategy for a bacterium to use to search for food?
{: .notice--primary}

## Quantifying the performance of our search algorithm

We already know from our work in previous modules that a random walk simulation can produce very different outcomes. In order to assess the performance of the random walk algorithm, we will simulate `num_cells` = 500 cells and `duration` = 1500 seconds.

Visualizing the trajectories for this many cells will be messy. Instead, we will measure the distance between each cell and the target at the end of the simulation, and then take the average and standard deviation of this value over all cells.

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

We will then plot the average and standard deviation of the distance to goal using the `plot` and `fill_between` functions.

~~~ python
#Below are all for plotting purposes
#Define the colors to use
colors1 = colorspace.qualitative_hcl(h=[0, 300.], c = 60, l = 70, pallete = "dynamic")(1)

xs = np.arange(0, duration)

fig, ax = plt.subplots(1, figsize = (10, 8))

mu, sig = all_dist_avg, all_dist_std
ax.plot(xs, mu, lw=2, label="pure random walk, back ground tumble every {} second".format(time_exp), color=colors1[0])
ax.fill_between(xs, mu + sig, mu - sig, color = colors1, alpha=0.15)

ax.set_title("Average distance to highest concentration")
ax.set_xlabel('time (s)')
ax.set_ylabel('distance to center (µm)')
ax.hlines(radius_saturation, 0, duration, colors='gray', linestyles='dashed', label='concentration 10^8')
ax.legend(loc='upper right')

ax.grid()
~~~

**STOP:** Before visualizing the average distances at each time step, what do you expect the average distance to the goal to be?
{: .notice--primary}

Now, run the notebook. The colored line indicates average distance of the 500 cells; the shaded area is standard deviation; and the grey dashed line corresponds to a maximum ligand concentration of 1e8.

If you run the notebook, you may not be surprised that this simple random walk strategy is not very effective at finding the goal. Not to worry: in the main text, we will discuss how to adapt this strategy into one that reflects how *E. coli* explores its environment based on what we have learned in this module about chemotaxis.

[^Saragosti2012]: Saragosti J., Siberzan P., Buguin A. 2012. Modeling *E. coli* tumbles by rotational diffusion. Implications for chemotaxis. PLoS One 7(4):e35412. [available online](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3329434/).

[^Berg1972]: Berg HC, Brown DA. 1972. Chemotaxis in Escherichia coli analysed by three-dimensional tracking. Nature. [Available online](https://www.nature.com/articles/239500a0)

[^Baker2005]: Baker MD, Wolanin PM, Stock JB. 2005. Signal transduction in bacterial chemotaxis. BioEssays 28:9-22. [Available online](https://pubmed.ncbi.nlm.nih.gov/16369945/)


[Return to main text](home_conclusion#strategy-2-chemotactic-random-walk){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
