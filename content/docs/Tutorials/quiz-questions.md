---
title: "Quiz Questions"
linkTitle: "Quiz Questions"
date: 2022-01-28
description: >
  Questions for use on quizzes and Exams
draft: false
---

**Question:** Why do we need to add hydrogens to protein structures from the RCSB?

>Because most methods used for determining protein structure (crystallography, electron microscopy) cannot determine the positions of hydrogen atoms. We can infer their positions from the positions of the heavy atoms.

**Question:** Why is it important to carefully inspect your protein structure (and protein structure file) before using programs like Open Babel to add hydrogens?

>You need to make sure that titratable groups like histidine are not part of the active sites before blindly adding hydrogens. The local environment of histidine and other groups can affect their pKa in unexpected ways. Also, you need to check the structure and structure file to determine if there are rotamers, missing heavy atoms, or missing loops. Missing atoms have to be fixed before MD simulations.
