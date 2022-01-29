---
title: "Open Babel"
linkTitle: "Open Babel"
date: 2022-01-28
description: >
  Open Babel is a set of tools to convert between the many filetypes used in molecular modeling, cheminformatics, and bioinformatics.
---

[Open Babel website](http://openbabel.org/wiki/Main_Page)  
[Open Babel documentation](https://openbabel.org/docs/dev/index.html)  
[Open Babel basic usage](https://openbabel.org/docs/dev/Command-line_tools/babel.html)  

### Installation in Colab

This installation code comes from the Colab notebook, [Docking with Smina](https://colab.research.google.com/drive/12pdvG99aij2put7o_QX-aw3MkRWdZa2g)

```py
!wget -c https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
!chmod +x Miniconda3-latest-Linux-x86_64.sh
!time bash ./Miniconda3-latest-Linux-x86_64.sh -b -f -p /usr/local
!time conda install -q -y -c openbabel openbabel
import sys
import os
sys.path.append('/usr/local/lib/python3.7/site-packages/')

# this failed with :
# UnsatisfiableError: The following specifications were found
# to be incompatible with the existing python installation in your environment:
# - openbabel -> python[version='2.7.*|3.4.*|3.5.*|3.6.*|>=2.7,<2.8.0a0|>=3.5,<3.6.0a0|>=3.6,<3.7.0a0|>=3.7,<3.8.0a0|>=3.4,<3.5.0a0']
# Your python: python=3.9

# I'll have to fix this.
```

### Using Open Babel to add hydrogens

You can use Open Babel to add hydrogens to a fixed protein structure file prior to using it for docking or molecular dynamics simulations. From [Stack Exchange](https://biology.stackexchange.com/questions/70841/why-add-hydrogens-in-molecular-dynamics-simulations):

>Most techniques in structural biology simply cannot resolve the positions of hydrogens in macromolecular structures; only NMR and neutron diffraction provide the positions of hydrogens "by default." Therefore, all-atom molecular dynamics force fields, the most notable of which are the OPLS, CHARMM, and AMBER series, require that the positions of hydrogens be deduced from the starting macromolecular structures and geometric criteria and patterns, that hydrogen bonds are known to generally follow, if the starting structure doesn't have hydrogens.
>
>In MD, most often one needs to add hydrogens everywhere, not just in the titratable groups, because the starting PDB structure doesn't have any hydrogens.

```zsh
obabel -ipdb 1maz-fix.pdb -opdb -O 1maz-fix-h.pdb -p7.4 

# obabel is the command that invokes the program
# -i indicates the input file type, and pdb denotes that it is pdb
# 1maz-fix.pdb is the name of the input file
# -opdb indicates that the output file type is pdb
# -O specifies the name of the output file, 1maz-fix-h.pdb 
# -p7.4 adds hydrogens appropriate for pH 7.4
```
