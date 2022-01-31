---
categories: ["tutorials"]
tags: ["allostery", "gromacs"]
title: "GROMACS Tutorial 1"
linkTitle: "GROMACS Tutorial 1"
date: 2022-01-28
description: >
  In this GROMACS tutorial, we will simulate the top inhibitor pose on Bcl-6 from an earlier MTiOpenScreen small molecule inhibitor screen.
resources:
- src: "switch-accounts-*.png"
  params:
    byline: ""
---

I previously conducted an MTiOpenScreen inhibitor screen of Bcl-6 (PDB ID [6TOF](https://www.rcsb.org/structure/6TOF)). We will use the top binding pose and perform a GROMACS simulation following the tutorial: [GROMACS: MD Simulation of a Protein-Ligand Complex](https://angeloraymondrossi.github.io/workshop/charmm-gromacs-small-organic-molecules-new.html). This tutorial is derived from the tutorial, [Lysozyme in Water](http://www.mdtutorials.com/gmx/lysozyme/index.html) by Justin A. Lemkul, PhD.

I'm going to (try) set up and carry out this tutorial using [Google Colab](https://colab.research.google.com/).

## Introduction

GROMACS (a proper name--not an an acronym for anything) is a collection of tools to perform molecular dynamics simulations. See the [GROMACS](https://www.gromacs.org/About_Gromacs) website for details on its features and performance.

For more information on how to use GROMACS and useful tutorials, visit the [GROMACS documentation](https://manual.gromacs.org/documentation/current/index.html).

## Learning Objectives

By the end of this tutorial, you will be able to

- understand the fundamentals of molecular dynamics simulations (solving Newton's equations of motion by deriving the potential and kinetic energy of each atom over time).
- prepare a protein structure file for a molecular dynamics simulation
- prepare a ligand for a molecular dynamics simulation
- analyze the resulting trajectory

## References

Instructions for setting up GROMACS on Google Colab was taken from the Jupyter notebook, [Lab.07 Molecular Dynamics on GROMACS](https://colab.research.google.com/github/pb3lab/ibm3202/blob/master/tutorials/lab07_MDsims.ipynb), which is part of the IBM3202 course, [Cloud-based Tutorials on Structural Bioinformatics](https://github.com/pb3lab/ibm3202).

## Start the Notebook

- Control-Click on the `Open in Colab` button, below, and choose `Open Link in New Tab`.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dgoppenheimer/PCB3109-Cancer-Biology/blob/main/gromacs_tutorial_1.ipynb)

- Switch to your Google Cancer Biology account.
- Connect to a runtime. If you get the error message that you have too many active connections, go to manage sessions and terminate the active session(s). Then connect to the runtime.
- Connect to 
- Copy the notebook to your Google Drive. Rename your notebook from `Copy of gromacs-tutorial-1.ipynb` to `<last name first initial>-gromacs-tutorial-1.ipynb` with no spaces and omitting the `< >`.
- Mount Google Drive.

```py
from google.colab import drive
drive.mount('/content/drive')
```

You should be able to see the copy of your notebook at `drive/MyDrive/Colab Notebooks/` in the left file browser.

## Part 1. Installing the Software

1. We start by downloading a pre-compiled and installed GROMACS on Google Colab.

```py
# Download and unzip the compressed folder of GROMACS 2020.6 version
!wget https://raw.githubusercontent.com/pb3lab/ibm3202/master/software/gromacs.tar.gz
!tar xzf gromacs.tar.gz
```

2. Upgrade `cmake`.

```py
# It is recommended (and required for GROMACS 2021) to upgrade cmake
!pip install cmake --upgrade
```

3. Check that GROMACS works.

```py
# Checking that our GROMACS works
%%bash
source /content/gromacs/bin/GMXRC
gmx -h
```

4. Install NGLView to view our molecules.

```py
# Installing nglview using pip
!pip install -q nglview
from google.colab import output
output.enable_custom_widget_manager()
```

```py
# Testing NGLView
import nglview
view = nglview.show_pdbid("3pqr", default=False)  # load "3pqr" from RCSB PDB and display viewer widget
view.add_cartoon(selection="protein", color='grey')
view.center()
view
```

```py
# more testing
# specify color
view.add_cartoon(selection="protein", color='yellow')
# specify residue
view.add_licorice('ALA, GLU')
# view non-protein items
view.add_ball_and_stick("not protein")
```

```py
# another test
# clear representations
view.representations = []
# add a new representation
view.add_representation('cartoon', color='maroon')
view.add_surface(selection="protein", opacity=0.1)
```

```py
# anther test
view.remove_cartoon(component=0)
```

5. Install **BioPython**.

```py
# Installing biopython using pip
!pip install biopython
```

Success! We are now ready do molecular dynamics simulations.

## Part 2. Setting Up the MD Simulation System

There are several parts to setting up the protein and ligand for an MD simulation.



---
- Click on your profile icon.

{{< imgproc switch-accounts-1.png Resize "600x" >}}
{{< /imgproc >}}

- Choose your Cancer Biology account.

{{< imgproc switch-accounts-2.png Resize "300x" >}}
{{< /imgproc >}}
