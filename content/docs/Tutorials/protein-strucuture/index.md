---
categories: ["tutorials"]
tags: ["pdb files", "rcsb", "MDAnalysis"]
title: "Analyzing Proteins"
linkTitle: "Analyzing Proteins"
date: 2022-02-05
description: >
  In this tutorial, we will use `MDAnalysis` to view and analyze `.pdb` structure files.
resources:
- src: ""
  params:
    byline: ""
---

## Introduction

There are many reasons to examine and manipulate the `.pdb` structure files downloaded from the [RCSB database](https://www.rcsb.org/). The structure files contain more than just positional information about the atoms that make up the protein. To better understand the data contained in PDB files, see [Introduction to PDB Data](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/introduction) including the following left menu items:

- [Biological Assemblies](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/biological-assemblies)
- [Dealing with Coordinates](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/dealing-with-coordinates)
- [Methods for Determining Atomic Structures](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/methods-for-determining-structure)
- [Missing Coordinates and Biological Assemblies](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/missing-coordinates-and-biological-assemblies)
- [Resolution](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/resolution)
- [R-value and R-free](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/r-value-and-r-free)
- [Primary Sequences and the PDB Format](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/primary-sequences-and-the-pdb-format)
- [Small Molecule Ligands](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/small-molecule-ligands)
- [Beginner’s Guide to PDB Structures and the PDBx/mmCIF Format](https://pdb101.rcsb.org/learn/guide-to-understanding-pdb-data/beginner%E2%80%99s-guide-to-pdb-structures-and-the-pdbx-mmcif-format)

In this tutorial we will use the free, open-source, software, [MDAnalysis](https://www.mdanalysis.org/), to explore and analyze protein structure (`.pdb`) files. For more information on MDAnalysis installation and usage, visit the [MDAnalysis User Guide](https://userguide.mdanalysis.org/stable/index.html).

This tutorial is largely based on the excellent [12. Analysing proteins using python](https://github.com/bigginlab/OxCompBio/blob/master/tutorials/Python/12_ProteinAnalysis/12_ProteinAnalysis.ipynb) tutorial by the [Biggin Laboratory](https://bigginlab.web.ox.ac.uk/) at the University of Oxford.

Let's get started!

## Learning Objectives

By the end of this tutorial, you will be able to:

- Install `MDAnalysis` and related libraries on Google Colab.

## Installation of software

### Mount Google Drive

Be sure that you are logged into your <mark>course (not personal or UF)</mark> Google account.

- Mount your Google Drive.
- Save a copy of this notebook to your GitHub repository and name it `<LastNameFirstInitial-Protein-Structure.ipynb`, of course using *your* last name and first initial (without spaces and without the `< >` symbols).

```py
# Mount your Google Drive
from google.colab import drive
drive.mount('/content/drive')
```

### Install Conda for Colab

See [How to install / use Conda on Google Colab](https://inside-machinelearning.com/en/how-to-install-use-conda-on-google-colab/), a guide to installing Conda when using Google Colab.

1. Check to see if conda is already installed:

```py
!conda --version
```

You should get the error, `/bin/bash: conda: command not found`.

2. Install conda:

```py
!pip install -q condacolab
import condacolab
condacolab.install()
```

Note that the kernel will reboot and you will see an error stating that *Your session crashed for an unknown reason.* It is okay to dismiss this notice.

3. Confirm that installation was successful:

```py
!conda --version
```

You should get `conda 4.9.2`. Success!

We will now install the following software (technically libraries):

[MDAnalysis](https://www.mdanalysis.org/)  
[NGLView](https://github.com/nglviewer/nglview)  
[Seaborn](https://seaborn.pydata.org/index.html)

### Install MDAnalysis

```py
!conda install -q -y --prefix /usr/local -c conda-forge mdanalysis
import sys
sys.path.append('/usr/local/lib/python3.7/site-packages/')
```

```py
import MDAnalysis as mda
```

### Install NGLView

See the [Install NGLView]({{< ref "nglview/index.md#install-nglview" >}} "Install NGLView") instructions in the NGLView Tutorial.

```py
# Installing nglview using pip
!pip install -q nglview
from google.colab import output
output.enable_custom_widget_manager()
```

From here we can enable the GUI or the widget. Let's enable the widget for this exercise.

```py
import nglview
```

### Install Seaborn

Here is a short exercise to familiarize yourself with Matplotlib: 

[Matplotlib Colab notebook](https://colab.research.google.com/github/jttoivon/data-analysis-with-python-spring-2019/blob/master/matplotlib.ipynb) and [04.00-Introduction-To-Matplotlib.ipynb](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/04.00-Introduction-To-Matplotlib.ipynb)

Also see [04.14-Visualization-With-Seaborn.ipynb](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/04.14-Visualization-With-Seaborn.ipynb) 


```py
!conda install seaborn
```

```py
import seaborn as sns
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
plt.show()
# just for fun
df = sns.load_dataset("penguins")
sns.pairplot(df, hue="species")
```

See [seaborn.boxplot](https://seaborn.pydata.org/generated/seaborn.boxplot.html) for syntax for my favorite type of plots.

And more information is found in [Visualization with Seaborn](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/04.14-Visualization-With-Seaborn.ipynb)

## Getting `.pdb` Files

An easy way to get files is to use `wget`.

```py
!wget http://files.rcsb.org/view/4tuh.pdb
```

Download the biological assembly of `4tuh`:

```py
!wget https://files.rcsb.org/download/4TUH.pdb1.gz
!gunzip 4TUH.pdb1.gz
```





Note: 

Test that you can download the biological assembly and rename it.

---

Good descriptions of what's in a `.pdb` file--better than the PDB101 section on the RCSB website. Assign the one from `wwpdb.org`.

[Protein Data Bank Contents Guide](https://www.wwpdb.org/documentation/file-format-content/format33/sect9.html)

[Coordinate File Description (PDB Format)](https://zhanggroup.org/SSIPe/pdb_atom_format.html)

[Introduction to Protein Data Bank Format](https://www.cgl.ucsf.edu/chimera/docs/UsersGuide/tutorials/pdbintro.html)
