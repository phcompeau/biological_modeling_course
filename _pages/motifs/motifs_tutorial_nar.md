---
permalink: /motifs/tutorial_nar
title: "Software Tutorial: Analyzing the Benefits of Negative Autoregulation"
sidebar:
 nav: "motifs"
toc: true
toc_sticky: true
---

### Comparing "Plain" Regulation with NAR (Negative Auto-Regulation)

Load up the *CellBlender_Tutorial_Template.blend* file from the [Random Walk Tutorial](https://purpleavatar.github.io/multiscale_biological_modeling/prologue/tutorial-random-walk). Save as *NAR_comparison_unqual.blend*

Go to *CellBlender > Molecules* and create the following molecules:

![image-center](../assets/images/motifs_norm1.png){: .align-center}

1. Click on the plus button
2. Select a color (such as yellow)
3. Name the molecule “Y1”
4. Select the molecule type as “Surface Molecule”
5. Add a diffusion constant of “1e-6”
6. Up the scale factor to 5 (click and type “5” or use the arrows)

Repeat the above steps to make sure the follow molecules are entered:

| Molecule Name | Molecule Type|Diffusion Constant| Scale Factor|
|:--------|:-------:|--------:|--------:|--------:|
| Y1  | Surface  | 1e-6  | 5|
| X1  | Surface  | 1e-6  | 1|

Now go to *CellBlender > Molecule Placement* to set the following sites:

![image-center](../assets/images/motifs_norm3.png){: .align-center}

1. Click on the plus button
2. Select or type in the molecule “X1”
3. Type in the name of the Object/Region “Plane”
4. Set the Quantity to Release as “300”

Repeat the above steps to make sure the following molecules are entered

| Molecule Name | Object/Region|Quantity to Release|
|:--------|:-------:|--------:|
| X1  | Plane | 300 |

Next go to *CellBlender > Reactions* to create the following reactions:

![image-center](../assets/images/motifs_norm4.png){: .align-center}

1. Click on the plus button
2. Under reactants, type “X1’” (NOTE the apostrophe)
3. Under products, type “X1’ + Y1’”
4. Set the forward rate as “2e2”

Repeat the above steps for the following reactions

| Reactants |Products|Forward Rate|
|:--------|:-------:|--------:|
| X1’  | X1’ + Y1’ | 4e2 |
| Y1’  | NULL | 4e2 |

Go to *CellBlender > Plot Output Settings* to set up a plot as follows:

![image-center](../assets/images/motifs_norm6.png){: .align-center}

1. Click on the plus button
2. Set the molecule name as Y1
3. Ensure “World” is selected
4. Make sure “Java Plotter” is selected
5. Ensure “One Page, Multiple Plots” is selected
6. Ensure “Molecule Colors” is selected

Repeat the above steps for the following molecules

| Molecule Name|Selected Region|
|:--------|:-------:|
| Y1| World|

Go to *CellBlender > Run Simulation* and select the following options:

![image-center](../assets/images/motifs_norm7.png){: .align-center}

1. Set the number of iterations to “20000”
2. Ensure the time step is set as “1e-6”
3. Click Export & Run

Click on *CellBlender > Reload Visualization Data*

![image-center](../assets/images/motifs_norm8.png){: .align-center}

You have the option of watching the animation within the Blender window by clicking the play button at the bottom of the screen.

Now go back to *CellBlender > Plot Output Settings* and scroll to the bottom to click “Plot”

![image-center](../assets/images/motifs_norm9.png){: .align-center}

You should be able to see Y reach a steady-state, where the number of particles essentially levels off.

Save this file

### Adding Negative Auto-Regulation

Now we will add the NAR reactions to compare how the two systems reach their respective steady-states.

Go to *CellBlender > Molecules* and create the following molecules:

![image-center](../assets/images/motifs_norm1.png){: .align-center}

1. Click on the plus button
2. Select a color (such as yellow)
3. Name the molecule “Y2”
4. Select the molecule type as “Surface Molecule”
5. Add a diffusion constant of “1e-6”
6. Up the scale factor to 5 (click and type “5” or use the arrows)

Repeat the above steps to make sure the follow molecules are entered:

| Molecule Name | Molecule Type|Diffusion Constant| Scale Factor|
|:--------|:-------:|--------:|--------:|--------:|
| Y1  | Surface  | 1e-6  | 5|
| X1  | Surface  | 1e-6  | 1|
| Y2  | Surface  | 1e-6  | 5|
| X2  | Surface  | 1e-6  | 1|

Now go to *CellBlender > Molecule Placement* to set the following sites:

![image-center](../assets/images/motifs_norm3.png){: .align-center}

1. Click on the plus button
2. Select or type in the molecule “X2”
3. Type in the name of the Object/Region “Plane”
4. Set the Quantity to Release as “300”

Repeat the above steps to make sure the following molecules are entered

| Molecule Name | Object/Region|Quantity to Release|
|:--------|:-------:|--------:|
| X1  | Plane | 300 |
| X2  | Plane | 300 |

Next go to *CellBlender > Reactions* to create the following reactions:

![image-center](../assets/images/motifs_norm4.png){: .align-center}

1. Click on the plus button
2. Under reactants, type “X2’” (NOTE the apostrophe)
3. Under products, type “X2’ + Y2’”
4. Set the forward rate as “2e2”

Repeat the above steps for the following reactions

| Reactants |Products|Forward Rate|
|:--------|:-------:|--------:|
| X1’  | X1’ + Y1’ | 4e2 |
| X2’  | X2’ + Y2’ | 4e2 |
| Y1’  | NULL | 4e2 |
| Y2’  | NULL | 4e2 |
|Y2’ + Y2’|Y2’|4e2|

Go to *CellBlender > Plot Output Settings* to set up a plot as follows:

![image-center](../assets/images/motifs_norm6.png){: .align-center}

1. Click on the plus button
2. Set the molecule name as Y1
3. Ensure “World” is selected
4. Make sure “Java Plotter” is selected
5. Ensure “One Page, Multiple Plots” is selected
6. Ensure “Molecule Colors” is selected

Repeat the above steps for the following molecules

| Molecule Name|Selected Region|
|:--------|:-------:|
| Y1| World|
| Y2| World|

Go to *CellBlender > Run Simulation* and select the following options:

![image-center](../assets/images/motifs_norm7.png){: .align-center}

1. Set the number of iterations to “20000”
2. Ensure the time step is set as “1e-6”
3. Click Export & Run

Click on *CellBlender > Reload Visualization Data*

![image-center](../assets/images/motifs_norm8.png){: .align-center}

You have the option of watching the animation within the Blender window by clicking the play button at the bottom of the screen.

Now go back to *CellBlender > Plot Output Settings* and scroll to the bottom to click “Plot”

![image-center](../assets/images/motifs_norm9.png){: .align-center}

The following plot should appear:

![image-center](../assets/images/nar_unequal_graph.PNG){: .align-center}

Save this file

[Return to main text](nar#Ensuring-the-same-steady-state-concentration){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}

### Matching Steady States

Open the file *NAR_comparison_unequal.blend* and a copy of the file as *NAR_comparison_equal.blend*

Go to *CellBlender > Reactions* to change the following reaction:

For X2’ -> X2’ + Y2’,  set the forward rate from 4e2 to 4e3

Go to *CellBlender > Run Simulation* and ensure the following options are selected:

![image-center](../assets/images/motifs_norm7.png){: .align-center}

1. Set the number of iterations to “20000”
2. Ensure the time step is set as “1e-6”
3. Click Export & Run

Click on *CellBlender > Reload Visualization Data*

![image-center](../assets/images/motifs_norm8.png){: .align-center}

You have the option of watching the animation within the Blender window by clicking the play button at the bottom of the screen.

Now go back to *CellBlender > Plot Output Settings* and scroll to the bottom to click “Plot”

![image-center](../assets/images/motifs_norm9.png){: .align-center}

The following plot should appear:

![image-center](../assets/images/nar_equal_graph.PNG){: .align-center}

[Return to main text](nar#Results){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
