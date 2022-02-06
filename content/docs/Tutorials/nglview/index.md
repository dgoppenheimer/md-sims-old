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

[NGLView](http://nglviewer.org/nglview/latest/) is an interactive widget to show and manipulate molecular structures and trajectories in Jupyter notebooks [^1]. It is based on NGLViewer, a web application for molecular visualization [^2] [^3]. In this tutorial we will use a Jupyter notebook running on [Google Colaboratory](https://colab.research.google.com/) (Google Colab or Colab for short). You can start the notebook by going to the [course GitHub repository](), and clicking the `Open in Colab` button.

Here is an [example notebook](https://mybinder.org/repo/hainm/nglview-notebooks).

{{% alert title="Note" color="info" %}}
I have not had much luck getting the above notebook to launch. I'll likely remove it from this tutorial.
{{% /alert %}}

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
- Explain how the Q61L mutation in RAC1 leads to cancer by inactivating the GTPase.

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

Commands for NGLView can be found in the [NGLView Documentation](http://nglviewer.org/nglview/latest/api.html#), but not many examples are given. Also, visit the [NGL](https://www.rcsb.org/docs/3d-viewers/ngl) page on the RCSB website for help in using NGLViewer.

- [Interactive data analysis with NGLView, pytraj and Jupyter notebook](https://ambermd.org/tutorials/analysis/tutorial_notebooks/nglview_notebook/index.html)
- [Selection language from NGL documentation](https://nglviewer.org/ngl/api/manual/usage/selection-language.html)

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

See the [MDsrv website](https://nglviewer.org/mdsrv/viewing.html) for a brief explanation of the controls for the NGLViewer GUI. Here is a short summary:

#### Interactive Controls

- `scroll` (or `scroll wheel`) zoom scene
- `scroll-shift` move near clipping plane and far fog
- `drag-right click` pan/translate scene (this works with 3-button mouse, but not track pad on Mac)
- `drag-left click` rotate scene
- `clickPick-left click` auto view picked component element
- `left click` on the desired atom (or its representation) to center view on that atom

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

In a previous assignment we learned how to gather information from `.pdb` files using `grep` and `awk`. The Jupyter notebook mentioned above used the program, [MDAnalysis](https://userguide.mdanalysis.org/1.0.0/index.html), to accomplish many of the same tasks. Either way is fine.

Here we will focus our attention on using the GUI. All of the manipulations that you perform on a protein in the GUI can be done my typing commands in a code cell and then executing that cell. The pros of using the GUI include not having to memorize a long list of commands that have to be typed perfectly. The cons of using the GUI include the difficulty of repeating a long set of instructions using the mouse when the execution of a single code cell can accomplish the same task. Using both these tools (the GUI and code cells) can be very handy when examining your protein structure files.

### Use the GUI

Import human H-RAS Bound to a GTP Analog in NGLView, PDB ID `4g0n` (note that it is a zero and not an "oh"). Refer back to the [MDsrv website](https://nglviewer.org/mdsrv/viewing.html) for a brief explanation of the controls for the NGLViewer GUI.

Also visit the [NGLViewer Selection Language](https://nglviewer.org/ngl/api/manual/usage/selection-language.html) web page for instructions on how to select chains, ligands, and residues.

1. Go to *File*&#8594;*PDB*, and type `4g0n` into the box and then hit `return` on your keyboard.
2. Delete the representations of the other protein, and center `4g0n`.

You can see that `4g0n` consists of two proteins, Ras and the Ras-binding domain (RBD) of Raf kinase.

3. To hide the representation of the Raf RBD, type `:A` in the `cartoon` filter box, and hit `return`. This means that you want to filter the cartoon representations to only chain A.

4. Add a `hyperball` representation, and in the filter box, type `HOH and :A` (show waters and chain A) and hit `return`.

5. Alter the representation of the waters, by changing the `colorScheme` to `uniform` and clicking the `colorValue` box and entering `FF33F9` (and always hit `return`).

6. Add a`surface` representation to the Ras protein, and change the opacity so you can still see the cartoon. Filter the surface representation by entering `protein and not HOH and not ligand` in the filter box. Adjust the opacity so you can see the binding pocket. Also, play around with the different `surfaceTypes`.

The `surfaceType` abbreviations are:

- `vws`: van der Waals surface
- `sas`: solvent accessible surface
- `ms`: molecular surface
- `ses`: solvent excluded surface
- `av`: high quality molecular surface

7. Filter the `hyperball` representation by typing `HOH and 364` into the filter box. Adjust the `radiusScale` to `1.0`.

8. Add a `ball and stick` representation to the Ras protein and filter it by typing `61` (amino acid 61) into the filter box.

You can see here that water molecule 364 is in the binding pocket between the GTP analog and the glutamine at position 61. This water molecule plays a key role in hydrolyzing the gamma phosphate of GTP to turn Ras signaling off.

### Use Code

Restart runtime.
Install NGLView.

```py
import nglview as nv
view = nglview.show_pdbid("3pqr", default=False)  # load "3pqr" from RCSB PDB and display viewer widget
view.add_cartoon(selection="protein", color='grey')
view.center()
view.gui_style = 'ngl'
view
```


Let's test another protein. Download `4tuh` (Bcl-xL in complex with inhibitor) from the RCSB database. 

---

#Uncomment the command below to add a hyperball representation of the crystal water oxygens in grey
#protein_view.add_hyperball('HOH', color='grey', opacity=1.0)


&#8594;


## Colors

You can use [CSS named colors](https://www.w3schools.com/colors/colors_groups.asp) in NGLView. Colors should always be lower case.

Try it!






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

```py
def superpose(self, components=[1,], ref=0, align=True, selection_0='', selection_1=''):
  for index in components:
            self._remote_call('superpose', target='Widget',
                    args=[ref, index, align, selection_0, selection_1])
  
nglview.superpose(components=[1], ref=0, align=True, selection_0='4g0n', selection_1='4gzl')
```

## Other Resources

[Talktorial: T017 · Advanced NGLview usage](https://projects.volkamerlab.org/teachopencadd/talktorials/T017_advanced_nglview_usage.html). This is where I found how to render a static image.

```py
view.render_image(trim=True, factor=2)
```

>- `trim=True` removes the blank padding around the molecule
>- `factor=2` asks for a 2x resolution render for higher quality, but not too much (default is 4x)

```py
view._display_image()
```

Also a nice list of the `view.add_(…)` commands.




[^1]: Nguyen H, Case DA & Rose AS (2018) NGLview-interactive molecular graphics for Jupyter notebooks. *Bioinformatics* **34**: 1241-1242.[DOI: 10.1093/bioinformatics/btx789](https://doi.org/10.1093/bioinformatics/btx789)

[^2]: Rose AS & Hildebrand PW (2015) NGL Viewer: a web application for molecular visualization. *Nucleic Acids Res* **43**: W576-9. [DOI: 10.1093/nar/gkv402](https://doi.org/10.1093/nar/gkv402)

[^3]: Rose AS, Bradley AR, Valasatava Y, Duarte JM, Prlić A & Rose PW (2016) Web-based molecular graphics for large complexes. Proceedings of the 21st international conference on Web3D technology 185-186.[DOI: 10.1145/2945292.2945324](http://dx.doi.org/10.1145/2945292.2945324)
