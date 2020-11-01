---
permalink: /chemotaxis/tutorial_walk
title: "Chemotactic random walk"
sidebar:
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

In this page, we will:
 - Simulate *E. coli* chemotaxis at a cellular level
 - Compare performance of chemotactic walk vs. standard random walk
 - Compare performances of different default tumbling frequencies.

## Simulation files and dependencies

Please download the simulation and visualization here: <a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/chemotaxis_walk.ipynb" download="chemotaxis_walk.ipynb">chemotaxis_walk.ipynb</a>. Detailed explanation of the model and each functions can be found in the file too.

Please make sure the following dependencies are installed:

| Installation Link | Version | Check install/version |
|:------|:-----:|------:|
| [Python3](https://www.python.org/downloads/)  |3.6+ |`python --version` |
| [Jupyter Notebook](https://jupyter.org/index.html) | 4.4.0+ | `jupyter --version` |
| [Numpy](https://numpy.org/install/) | 1.14.5+ | `pip list | grep numpy` |
| [Matplotlib](https://matplotlib.org/users/installing.html) | 3.0+ | `pip list | grep matplotlib` |
| [Colorspace](https://python-colorspace.readthedocs.io/en/latest/installation.html) or with [pip](https://pypi.org/project/colorspace/)| any | `pip list | grep colorspace`|

## Modeling chemotactic walk at a cellular level

Our model will be based on observations from BNG simulation and *E. coli* biology.

Ingredients and simplifying assumptions of the model:
 - Run. The background average duration of each run (`time_exp`) is a variable of interst. When the cell senses concentration change, the cell changes the expected run duration (`exp_run_time`). The duration of each run follows an exponential distribution with mean = `exp_run_time`.
 - Tumble. The duration of cell tumble follows an exponential distribution with mean 0.1s[^Saragosti2012]. When it tumbles, we assume it only changes the orientation for the next run but doesn't move in space. The degree of reorientation follows uniform distribution from 0° to 360°.
 - Response. As we've seen in the BNG model, the cell can respond to the gradient change within 0.5 seconds. In this model, we allow cells to re-measure the concentration after it runs for 0.5 seconds.
 - Gradient. We model an exponential gradient centered at [1500, 1500] with a concentration of 10<sup>8</sup>. All cells start at [0, 0], which has a concentration of 10<sup>8</sup>. The receptors saturate at a concentration of 10<sup>8</sup>.
 - Performance. The closer to the center of the gradient the better.

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
sec_per_step = 0.5 #Able to respond every 0.5 second
step_size, step_per_sec = speed * sec_per_step, 1.0 / sec_per_step

#Model constants
start = [0, 0]  #All cells start at [0, 0]
ligand_center = [1500, 1500] #Position of highest concentration
center_exponent, start_exponent = 8, 2
origin_to_center = 0 #will be actually calculated later
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

The run duration follows an exponential distribution with `exp_run_time`. When no gradient is present, `exp_run_time` = `time_exp`. When there is a change in ligand concentration, `exp_run_time` changes accordingly. The change is calculated as `(curr_conc - past_conc) / past_conc` to normalize for the exponential gradient. We model this response with `exp_run_time` = `time_exp` + 10 * `change`.

~~~ python
# Calculate the wait time for next tumbling event
def run_time(curr_conc, past_conc, position, time_exp):

    curr_conc = min(curr_conc, saturation_conc) #Can't detect higher concentration if receptors saturates
    past_conc = min(past_conc, saturation_conc)
    change = (curr_conc - past_conc) / past_conc #proportion change in concentration
    exp_run_time = time_exp * (1 + 10 * change)

    #print(exp_run_time, curr_conc, past_conc, position)

    if exp_run_time < 0.000001:
        exp_run_time = 0.000001 #positive wait times
    elif exp_run_time > 4 * time_exp:
        exp_run_time = 4 * time_exp     #the decrease to tumbling frequency is only to a certain extent
    curr_run_time = np.random.exponential(exp_run_time)

    return curr_run_time
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

Simulation through time for all `time_exp` of all cells.

For each cell, simulate through time as the following procedure:

while `t` < duration:
 - Assess the current concentration
 - Update current run duration `curr_run_time`
 - If `curr_run_time` < 0.5s:
        1. run for `curr_run_time` second along current direction
        2. Sample the duration of tumble `tumble_time` and the resulted direction
        3. increment t by `curr_run_time` and `tumble_time`
 - If `curr_run_time` > 0.5s:
        1. run for 0.5s along current direction
        2. increment `t` by 0.5s (and then the cell will re-assess the new concentration, and decide the duration of next run)

~~~ python
def simulate(num_cells, duration, time_exp):

    path = np.zeros((len(time_exp), num_cells, duration + 1, 2))
    terminals = [[] for i in range(len(time_exp))]

    for freq_i in range(len(time_exp)):
        for rep in range(num_cells):
            # Initialize simulation, direction initialized randomly
            t = 0
            curr_position = np.array(start)
            past_conc = calc_concentration(start) #Initialize concentration
            curr_direction, move_h, move_v, tumble_time = tumble_move(0)

            while t < duration:
                curr_conc = calc_concentration(curr_position)

                #get wait time
                curr_run_time = run_time(curr_conc, past_conc, curr_position, time_exp[freq_i])

                # if run time (r) is within the step (s), run for r second and then tumble
                if curr_run_time < sec_per_step:
                    curr_position = curr_position + np.array([move_h, move_v]) * speed * curr_run_time
                    curr_direction, move_h, move_v, tumble_time = tumble_move(curr_direction)
                    t += (curr_run_time + tumble_time)

                # if r > s, run for r; then it will be in the next iteration
                else:
                    curr_position = curr_position + np.array([move_h, move_v]) * speed * sec_per_step
                    t += sec_per_step

                #record position approximate for integer number of second
                curr_sec = int(t)
                if curr_sec <= duration:
                    path[freq_i, rep, curr_sec] = curr_position.copy()
                    past_conc = curr_conc

            terminals[freq_i].append((path[freq_i, rep, -1]))

    return terminals, path
~~~

## Compare performance of the two strategies

Please download the simulation and visualization here: <a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/chemotaxis_compare.ipynb" download="chemotaxis_compare.ipynb">chemotaxis_compre.ipynb</a>.

To compare the performance of the two strategies, we visualize the trajectories of simulation with 3 cells and quantitative compare the performance using simulation with 500 cells for each strategy.

Part 1 in the notebook defines the simulation using the two strategies (same as in the two tutorials).

**Qualitative comparison**. Run the two code blocks for Part2: Visualizing trajectories (1st block simulates, 2nd block is plotter).  The background color indicates concentration: white -> red = low -> high; black dot are starting points; red dots are the points they reached at the end of the simulation; colorful small dots represents trajectories (one color one cell): dark -> bright color = older -> newer time points; blue cross indicates the goal.

We will simulate 3 cells for 800 seconds for each of the strategies.

~~~ python
#Run simulation for 3 cells for each strategy, plot path
duration = 800   #seconds, duration of the simulation
num_cells = 3
origin_to_center = euclidean_distance(start, ligand_center) #Update the global constant
time_exp = 1.0

terminals_rand, path_rand = simulate_pure_random(num_cells, duration, time_exp)
terminals_che, path_che = simulate_che(num_cells, duration, time_exp)
terminals, path = np.array([terminals_rand, terminals_che]), np.array([path_rand, path_che])
~~~

The plotting is similar as before, except that this time, we will have two subplots, one for pure random walk and another for chemotactic random walk, initialized with `plt.subplots(1, 2)`. We will plot the simulation results for each strategy.
~~~ python
#Below are all for plotting purposes
methods = ["Pure random walk", "Chemotactic random walk"]
fig, ax = plt.subplots(1, 2, figsize = (16, 8))

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

for m in range(2):
    ax[m].imshow(conc_matrix.T, cmap=cmap_color, interpolation='nearest', extent = [-1000, 3000, -1000, 3000], origin = 'lower')

    #Plot simulation results
    time_frac = 1.0 / duration
    #Time progress: dark -> colorful
    for t in range(duration):
        ax[m].plot(path[m,0,t,0], path[m,0,t,1], 'o', markersize = 1, color = (0.2 * time_frac * t, 0.85 * time_frac * t, 0.8 * time_frac * t))
        ax[m].plot(path[m,1,t,0], path[m,1,t,1], 'o', markersize = 1, color = (0.85 * time_frac * t, 0.2 * time_frac * t, 0.9 * time_frac * t))
        ax[m].plot(path[m,2,t,0], path[m,2,t,1], 'o', markersize = 1, color = (0.4 * time_frac * t, 0.85 * time_frac * t, 0.1 * time_frac * t))
    ax[m].plot(start[0], start[1], 'ko', markersize = 8)
    for i in range(num_cells):
        ax[m].plot(terminals[m][i][0], terminals[m][i][1], 'ro', markersize = 8)
    ax[m].plot(1500, 1500, 'bX', markersize = 8)
    #ax.plot(path[:,0], path[:,1], '-', color = 'grey')

    #Indicate the saturation areas
    ax[m].set_title("{}\n Average tumble every 1 s".format(methods[m]), x = 0.5, y = 0.87)
    ax[m].set_xlim(-1000, 3000)
    ax[m].set_ylim(-1000, 3000)
    ax[m].set_xlabel("poisiton in μm")
    ax[m].set_ylabel("poisiton in μm")

fig.tight_layout()

plt.show()
~~~

Which strategy allows the cell travel towards the higher concentration?

Can we make a conclusion on which default tumbling frequencies are good yet? If not, why?

**Quantitative comparsion**. Because of the high variations due to randomness, trajectories for 3 cells is not convincing enough. To verify your hypothesis on which strategy is better, let's simulate 500 cells for 1500 seconds for each strategy. Run the two code blocks for Part3: Comparing performances (1st block simulates, 2nd block is plotter). Each colored line indicates a strategy, plotting average distances for the 500 cells; the shaded area is standard deviation; grey dashed line is where concentration reaches 1e8.

Like we did above, we run simulations for each strategies. 

~~~ python
#Run simulation for 3 cells with different background tumbling frequencies, Plot path

duration = 1500   #seconds, duration of the simulation
num_cells = 500
origin_to_center = euclidean_distance(start, ligand_center) #Update the global constant
time_exp = 1.0

terminals_rand, path_rand = simulate_pure_random(num_cells, duration, time_exp)
terminals_che, path_che = simulate_che(num_cells, duration, time_exp)
terminals, path = np.array([terminals_rand, terminals_che]), np.array([path_rand, path_che])

all_distance = np.zeros((2, num_cells, duration)) #Initialize to store results

for m in range(2):
    for c in range(num_cells):
        for t in range(duration):
            pos = path[m, c, t]
            dist = euclidean_distance(ligand_center, pos)
            all_distance[m, c, t] = dist

all_dist_avg = np.mean(all_distance, axis = 1)
all_dist_std = np.std(all_distance, axis = 1)
~~~

And then plotting the average distance to center vs. time like in the [previous tutorial](tutorial_purerandom).
~~~ python
#Below are all for plotting purposes
#Define the colors to use
colors1 = colorspace.qualitative_hcl(h=[0, 200.], c = 60, l = 70, pallete = "dynamic")(2)

xs = np.arange(0, duration)

fig, ax = plt.subplots(1, figsize = (10, 8))

for m in range(2):
    mu, sig = all_dist_avg[m], all_dist_std[m]
    ax.plot(xs, mu, lw=2, label="{}".format(methods[m]), color=colors1[m])
    ax.fill_between(xs, mu + sig, mu - sig, color = colors1[m], alpha=0.15)

ax.set_title("Average distance to highest concentration")
ax.set_xlabel('time (s)')
ax.set_ylabel('distance to center (µm)')
#ax.hlines(0, 0, duration, colors='gray', linestyles='dashed', label='concentration 10^8')
ax.legend(loc='upper right', ncol = 2, fontsize = 15)

ax.grid()
~~~

Which strategy is more efficient?

Go back to main text for now. We will return to this tutorial later.

[Back to Main Text](home_conclusion){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

## Qualitative comparison of different background tumbling frequencies

We will use <a href="https://purpleavatar.github.io/multiscale_biological_modeling/downloads/downloadable/chemotaxis_walk.ipynb" download="chemotaxis_walk.ipynb">chemotaxis_walk.ipynb</a> to compare trajectories for different tumbling frequencies.

Before assessing the performances of different default tumbling frequencies, let's run simulation for 3 cells for 800 seconds for the different tumbling frequencies to get a rough idea of what their trajectories look like. We will use a range of 10x shorter expected run time (0.1s) to 10x longer expected run time (10s).

~~~ python
duration = 800   #seconds, duration of the simulation
num_steps = duration * step_per_sec
num_cells = 3
origin_to_center = euclidean_distance(start, ligand_center) #Update the global constant
time_exp = [0.2, 1.0, 5.0]

terminals, path = simulate(num_cells, duration, time_exp)
~~~

Then we will plot the trajectories like we previously did. 

~~~ python
#Run simulation for 3 cells with different background tumbling frequencies, Plot path

duration = 800   #seconds, duration of the simulation
num_cells = 3
origin_to_center = euclidean_distance(start, ligand_center) #Update the global constant
time_exp = [0.2, 1.0, 5.0]


terminals, path = simulate(num_cells, duration, time_exp)

conc_matrix = np.zeros((3500, 3500))
for i in range(3500):
    for j in range(3500):
        conc_matrix[i][j] = math.log(calc_concentration([i - 500, j - 500]))

mycolor = [[256, 256, 256], [256, 255, 254], [256, 253, 250], [256, 250, 240], [255, 236, 209], [255, 218, 185], [251, 196, 171], [248, 173, 157], [244, 151, 142], [240, 128, 128]] #from coolors：）
for i in mycolor:
    for j in range(len(i)):
        i[j] *= (1/256)
cmap_color = colors.LinearSegmentedColormap.from_list('my_list', mycolor)


for freq_i in range(len(time_exp)):
    fig, ax = plt.subplots(1, figsize = (8, 8))
    ax.imshow(conc_matrix.T, cmap=cmap_color, interpolation='nearest', extent = [-500, 3000, -500, 3000], origin = 'lower')

    #Plot simulation results
    time_frac = 1.0 / duration
    #Time progress: dark -> colorful
    for t in range(duration):
        ax.plot(path[freq_i,0,t,0], path[freq_i,0,t,1], 'o', markersize = 1, color = (0.2 * time_frac * t, 0.85 * time_frac * t, 0.8 * time_frac * t))
        ax.plot(path[freq_i,1,t,0], path[freq_i,1,t,1], 'o', markersize = 1, color = (0.85 * time_frac * t, 0.2 * time_frac * t, 0.9 * time_frac * t))
        ax.plot(path[freq_i,2,t,0], path[freq_i,2,t,1], 'o', markersize = 1, color = (0.4 * time_frac * t, 0.85 * time_frac * t, 0.1 * time_frac * t))
    ax.plot(start[0], start[1], 'ko', markersize = 8)
    ax.plot(1500, 1500, 'bX', markersize = 8)
    for i in range(num_cells):
        ax.plot(terminals[freq_i][i][0], terminals[freq_i][i][1], 'ro', markersize = 8)
    #ax.plot(path[:,0], path[:,1], '-', color = 'grey')

    ax.set_title("Background tumbling freq:\n tumble every {} s".format(time_exp[freq_i]), x = 0.5, y = 0.9, fontsize = 12)
    ax.set_xlim(-500, 3000)
    ax.set_ylim(-500, 3000)
    ax.set_xlabel("poisiton in μm")
    ax.set_ylabel("poisiton in μm")
    
plt.show()
~~~

Run the two code blocks for Part2: Visualizing trajectories (1st block simulates, 2nd block is plotter). The background color indicates concentration: white -> red = low -> high; black dot are starting points; red dots are the points they reached at the end of the simulation; colorful small dots represents trajectories (one color one cell): dark -> bright color = older -> newer time points; if highest possible concentration > 1e8, dark dashed circle is where concentration reaches 1e8.

What do you observe? Pay attention to: 1) are the cells moving up the gradient?; 2) what's the differences between the shape of the trajectories?; 3) within the 1500 seconds, which expected run time allow the cells reach the black dashed circle (target)?; 4) after reaching the target, which expected run time allow the cells to stay in/near the circle?

## Quantitative comparison of different background tumbling frequencies

We will quantitatively measure the performances by the ability to reach the target at the end of the simulation. We will also calculate the average distance to the center at each time step for each of the `time_exp` values.

~~~ python
#Run simulation for 500 cells with different background tumbling frequencies, Plot average distance to highest concentration point
duration = 1500   #seconds, duration of the simulation
num_steps = duration * step_per_sec
num_cells = 500
time_exp = [0.1, 0.2, 0.5, 1.0, 2.0, 5.0, 10.0]
origin_to_center = euclidean_distance(start, ligand_center) #Update the global constant
radius_saturation = (1 - ((math.log10(saturation_conc) - start_exponent) / (center_exponent - start_exponent))) * origin_to_center

all_distance = np.zeros((len(time_exp), num_cells, duration)) #Initialize to store results

terminals, paths = simulate(num_cells, duration, time_exp) #run simulation

for freq_i in range(len(time_exp)):
    for c in range(num_cells):
        for t in range(duration):
            pos = paths[freq_i, c, t]
            dist = euclidean_distance(ligand_center, pos)
            all_distance[freq_i, c, t] = dist

all_dist_avg = np.mean(all_distance, axis = 1)
all_dist_std = np.std(all_distance, axis = 1)
~~~

And plot the average distance to center vs. time.

~~~ python
#Below are all for plotting purposes
#Define the colors to use
colors1 = colorspace.qualitative_hcl(h=[0, 300.], c = 60, l = 70, pallete = "dynamic")(len(time_exp))

xs = np.arange(0, duration)

fig, ax = plt.subplots(1, figsize = (10, 8))

for freq_i in range(len(time_exp)):
    mu, sig = all_dist_avg[freq_i], all_dist_std[freq_i]
    ax.plot(xs, mu, lw=2, label="tumble every {} second".format(time_exp[freq_i]), color=colors1[freq_i])
    ax.fill_between(xs, mu + sig, mu - sig, color = colors1[freq_i], alpha=0.1)

ax.set_title("Average distance to highest concentration")
ax.set_xlabel('time (s)')
ax.set_ylabel('distance to center (µm)')
ax.legend(loc='lower left', ncol = 1)

ax.grid()
~~~

**STOP:** Before visualizing the average distances at each time step, what do you expect the result to be (based on the trajectories)?
{: .notice--primary}

Run the two code blocks for Part3: Comparing performances (1st block simulates, 2nd block is plotter). Each colored line indicates a `time_exp`, plotting average distances for the 500 cells; the shaded area is standard deviation; grey dashed line is where concentration reaches 1e8.

What do you conclude about their performances?

Change the value for `duration` and run simulations for `time_exp = [0.25]` only. How long will the cells take to reach near the grey dashed line?

For all `time_exp`, after some time the average distance flattens. Why for different `time_exp` values, the lines flatten at different distances?


[^Saragosti2012]: Saragosti J., Siberzan P., Buguin A. 2012. Modeling *E. coli* tumbles by rotational diffusion. Implications for chemotaxis. PLoS One 7(4):e35412. [available online](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3329434/).

[^Berg1972]: Berg HC, Brown DA. 1972. Chemotaxis in Escherichia coli analysed by three-dimensional tracking. Nature. [Available online](https://www.nature.com/articles/239500a0)

[^Baker2005]: Baker MD, Wolanin PM, Stock JB. 2005. Signal transduction in bacterial chemotaxis. BioEssays 28:9-22. [Available online](https://pubmed.ncbi.nlm.nih.gov/16369945/)


[Back to Main Text](home_conclusion){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}


