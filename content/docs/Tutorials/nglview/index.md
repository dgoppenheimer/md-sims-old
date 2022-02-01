---
categories: ["tutorials"]
tags: ["pdb files", "nglview"]
title: "NGLView Tutorial"
linkTitle: "NGLView Tutorial"
date: 2022-01-31
description: >
  In this tutorial, we will use `NGLView` to view and manipulate `.pdb` structure files and MD simulation trajectory files.
resources:
- src: "nglview1.png, nglviewer-gui.png"
  params:
    byline: ""
---

## Introduction

[NGLView](http://nglviewer.org/nglview/latest/) is an interactive widget to show and manipulate molecular structures and trajectories in Jupyter notebooks.

Here is an [example notebook](https://mybinder.org/repo/hainm/nglview-notebooks),

## Learning Objectives

By the end of this assignment, students will be able to:

- Install `NGLView` in a Colab notebook.
- Load a protein structure file into `NGLView`.
- Display multiple representations of a protein and ligand using the gui.
- Display multiple representations of a protein and ligand using commands.
- Align multiple protein structures.
- Save commands to easily reproduce multiple representations.
- Save protein images as `.png` files.
- Save trajectories as movie files.

## Installing the Software

### Preparation

Be sure that you are logged into your <mark>course (not personal or UF)</mark> Google account.

- Mount your Google Drive. 
- Save a copy of this notebook to your Google Drive and name it `<LastNameFirstInitial-NGLview-Tutorial.ipynb`, of course using your last name and first initial (without spaces and without the `< >` symbols.

```py
# Mount your Google Drive
from google.colab import drive
drive.mount('/content/drive')
```

### Install NGLView

```py
# Installing nglview using pip
!pip install -q nglview
from google.colab import output
output.enable_custom_widget_manager()
```

Let's test that NGLView works by downloading and viewing a `.pdb` file from the RCSB database.

```py
# Testing NGLView
import nglview
view = nglview.show_pdbid("3pqr", default=False)  # load "3pqr" from RCSB PDB and display viewer widget
view.add_cartoon(selection="protein", color='grey')
view.center()
view
```

The above code should produce something that looks like this:

{{< imgproc nglview1.png Resize "500x" >}}
Successful image rendered by NGLView
{{< /imgproc >}}

Let's play with NGLView a bit more.

```py
# more testing
# specify color
view.add_cartoon(selection="protein", color='yellow')
# specify residue
view.add_licorice('ALA, GLU')
# view non-protein items
view.add_ball_and_stick("not protein")
```

Commands for NGLView can be found in the [NGLView Documentation](http://nglviewer.org/nglview/latest/api.html#), but not many examples are given. 

### Enable the NGLView GUI

There is a development version of a graphical user interface (GUI) that runs well on colab. Let's play with it for a bit.

```py
import os
```

```py
import nglview as nv

view = nv.demo()
view.gui_style = 'ngl'
view
```

If the GUI was enabled successfully, you should see something like this:

{{< imgproc nglviewer-gui.png Resize "500x" >}}
Successfully enabled NGLViewer GUI
{{< /imgproc >}}

See the [MDsrv website](https://nglviewer.org/mdsrv/viewing.html) for an explanation of the controls for the NGLViewer GUI.

#### Questions

These questions were taken from the [OxCompBio NGLView tutorial](https://github.com/bigginlab/OxCompBio/blob/master/tutorials/MD/02_Protein_Visualization.ipynb).

1. When you load the structure, can you see the two subunits that form the dimer?
2. Can you locate the drug in the binding pocket?

*Hint:* Go to `View` and then `Full screen` to expand the viewing window.

3. Can you hide all the other representations and view only the drug?

*Hint:* Use your mouse to rotate, translate and zoom in and out.

*Hint:* You can hide/show a representation by clicking on the "eye" symbol on the right panel.

Explore the [NGLView documentation](http://nglviewer.org/nglview/latest/api.html), and play around with different representations, selections, colors etc. Take as much time as you want in this step.

## Assignment

Much of this assignment was adapted from the [Analyzing proteins using python](https://github.com/bigginlab/OxCompBio/blob/master/tutorials/Python/12_ProteinAnalysis/12_ProteinAnalysis.ipynb) Jupyter notebook, mostly the *Visualising a PDB using NGLView* subsection.

---

### Example Notebooks

The [NGLView website](https://github.com/nglviewer/nglview/blob/master/examples/README.md) has a link to [example notebooks](https://nbviewer.org/github/nglviewer/nglview/tree/master/examples/notebooks/) that show how to perform common tasks using NGLView in Jupyter notebooks.

### Saving Images

See the [export_image.ipynb](https://nbviewer.org/github/nglviewer/nglview/blob/master/examples/notebooks/export_image.ipynb) example notebook.



### Making Movies

See the [Interactive data analysis with NGLView: Make a movie](http://ambermd.org/tutorials/analysis/tutorial_notebooks/nglview_movie/) tutorial.

### Aligning Multiple Structures

```py
superpose(components=[1], ref=0, align=True, selection_0='', selection_1='')
# port superpose method from NGL. Good for single structures. 
```

