---
permalink: /chemotaxis/tutorial_tumbling_frequencies
title: "Comparing the outcomes of chemotaxis with different background tumbling frequencies"
sidebar:
 nav: "chemotaxis"
toc: true
toc_sticky: true
---

In this tutorial, we will run a comparison of the chemotactic random walk over a variety of different background tumbling frequencies. Are some frequencies better than others at helping the bacterium reach the goal?

## Qualitative comparison of different background tumbling frequencies

We will use <a href="../downloads/downloadable/chemotaxis_walk.ipynb" download="chemotaxis_walk.ipynb">chemotaxis_walk.ipynb</a> to compare trajectories for different tumbling frequencies.

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

Our measure of how well a bacterium has done at reaching the goal will be the distance from the simulated cell to the goal at the end of the simulation. We will also calculate the average distance to the center at each time step for each of the `time_exp` values.

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

Run the entire notebook for the chemotactic walk, including both simulation and plotting. Each colored line indicates a `time_exp`, plotting average distances for the 500 cells; the shaded area is standard deviation; grey dashed line is where concentration reaches 1e8.

What do you conclude about their performances?

Change the value for `duration` and run simulations for `time_exp = [0.25]` only. How long will the cells take to reach near the grey dashed line?

For all `time_exp`, after some time the average distance flattens. Why for different `time_exp` values, the lines flatten at different distances?

[Return to main text](home_conclusion#why-is-bacterial-background-tumbling-frequency-constant-across-species){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
