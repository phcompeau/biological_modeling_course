---
permalink: /motifs/tutorial_oscillators
title: "Oscillators"
sidebar:
 nav: "motifs"
toc: true
toc_sticky: true
---

### The Repressilator

Load up the *CellBlender_Tutorial_Template.blend* file from the [Random Walk Tutorial](https://purpleavatar.github.io/multiscale_biological_modeling/prologue/tutorial-random-walk). Save as *repressilator.blend*

Go to *CellBlender > Molecules* and create the following molecules:

![image-center](../assets/images/motifs_norm1.png){: .align-center}

1. Click on the plus button
2. Select a color (such as yellow)
3. Name the molecule “Y1”
4. Select the molecule type as “Surface Molecule”
5. Add a diffusion constant of “1e-6”
6. Up the scale factor to 5 (click and type “5” or use the arrows)

Repeat the above steps to make sure the following molecules are entered.

| Molecule Name | Molecule Type|Diffusion Constant| Scale Factor|
|:--------|:-------:|--------:|--------:|--------:|
| X  | Surface  | 4e-5  | 5|
| Y  | Surface  | 4e-5  | 5|
| Z  | Surface  | 4e-5  | 5|
| HiddenX  | Surface  | 3e-6  | 3|
| HiddenY  | Surface  | 3e-6  | 3|
| HiddenZ  | Surface  | 3e-6  | 3|
| HiddenX_off  | Surface  | 1e-6  | 3|
| HiddenY_off  | Surface  | 1e-6  | 3|
| HiddenZ_off  | Surface  | 1e-6  | 3|


Now go to *CellBlender > Molecule Placement* to set the following sites:

![image-center](../assets/images/motifs_norm3.png){: .align-center}

1. Click on the plus button
2. Select or type in the molecule “X”
3. Type in the name of the Object/Region “Plane”
4. Set the Quantity to Release as “150”

Repeat the above steps to make sure the following molecules are entered.

| Molecule Name | Object/Region|Quantity to Release|
|:--------|:-------:|--------:|
| X  | Plane | 150 |
| HiddenX  | Plane | 100 |
| HiddenY  | Plane | 100 |
| HiddenZ  | Plane | 100 |

Next go to *CellBlender > Reactions* to create the following reactions:

![image-center](../assets/images/motifs_norm4.png){: .align-center}

1. Click on the plus button
2. Under reactants, type “HiddenX’” (NOTE the apostrophe)
3. Under products, type “HiddenX’ + X’”
4. Set the forward rate as “2e3”

Repeat the above steps for the following reactions.

| Reactants |Products|Forward Rate|
|:--------|:-------:|--------:|
| HiddenX’  | HiddenX’ + X’ | 2e3 |
| HiddenY’  | HiddenY’ +Y’ | 2e3 |
| HiddenZ’  | HiddenZ’ + Z’ | 2e3 |
| X’ + HiddenY’ | HiddenY_off’ + X, | 6e2 |
| Y’ + HiddenZ’ | HiddenZ_off’ + Y, | 6e2 |
| Z’ + HiddenX’ | HiddenX_off’ + Z, | 6e2 |
| HiddenX_off’ | HiddenX’ | 6e2 |
| HiddenY_off’ | HiddenY’ | 6e2 |
| HiddenZ_off’ | HiddenZ’ | 6e2 |
| X’ | NULL | 6e2 |
| Y’ | NULL | 6e2 |
| Z’ | NULL | 6e2 |
| X, | X’ | 2e2 |
| Y, | Y’ | 2e2 |
| Z, | Z’ | 2e2 |
{: text-align: center;"}

NOTE: Some molecules require an apostrophe or a comma. This represents the orientation of the molecule in space and is very important to the reactions!

Go to *CellBlender > Plot Output Settings* to set up a plot as follows:

![image-center](../assets/images/motifs_norm6.png){: .align-center}

1. Click on the plus button
2. Set the molecule name as X
3. Ensure “World” is selected
4. Make sure “Java Plotter” is selected
5. Ensure “One Page, Multiple Plots” is selected
6. Ensure “Molecule Colors” is selected

Repeat the above steps for the following molecules.

| Molecule Name|Selected Region|
|:--------|:-------:|
| X | World|
| Y | World|
| Z | World|

Go to *CellBlender > Run Simulation* and select the following options:

![image-center](../assets/images/motifs_norm7.png){: .align-center}

1. Set the number of iterations to “120000”
2. Ensure the time step is set as “1e-6”
3. Click Export & Run

Click on *CellBlender > Reload Visualization Data*

![image-center](../assets/images/motifs_norm8.png){: .align-center}

You have the option of watching the animation within the Blender window by clicking the play button at the bottom of the screen.

Now go back to *CellBlender > Plot Output Settings* and scroll to the bottom to click “Plot”

![image-center](../assets/images/motifs_norm9.png){: .align-center}

* NOAH: please continue to flesh this out and provide a final probing question asking students to interpret the plot that they produced.

[Return to main text](oscillators#the-oscillations-of-the-repressilator){: .btn .btn--primary .btn--large}
{: style="font-size: 100%; text-align: center;"}
