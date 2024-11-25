---
title: Visualizing Multislice
---

## Overview

In PRISM we use the multi-slice algorithm(cite) to propagate the probe through the sample. To do this we first must load in or generate a sample to measure and slice it into atom-thin chunks that we calculate the electric potentials of. Then we can propagate through the slices in alternating steps where we calculate the phase from the interaction of the electrons using the potential slices and then propagate the spacing between the z planes using a fresnel propagation. We can propagate using this method through the sample until we achieve the final diffraction pattern. 

## Building and Slicing Material:

To get started on slicing, one first must load or generate a sample which is often defined in terms of the coordinates of atoms as well as the atoms themselves. Once one has a sample one can slice the material into thin layers of sheets of atoms in the z-direction. It is important to slice at least as thin as the spacing between planes of atoms so that all of the atoms are incorporated properly. If one would like to the algorithm allows for varying slicing thicknesses as long as the thicknesses are then accounted for during the propagation step later.

:::{figure} ./figures/silicon_slice.png
:name: Generation of Silicon using ase
:alt: Slice of a Silicon Layer
:align: center
Generation of Silicon using ase
:::

## Calculating the Electronic Potential:
Once one has created sheets of atoms the electric potentials across each of the sheets can be computed using the methods found in the kirkland textbook that use

:::{figure} ./figures/pot_array.png
:name: Single Potential Slice of Silicon
:alt: Slice of a Silicon Layer
:align: center
Single potential Slice of Silicon calculated using kirkland algorithm in abtem package
:::

In this interactive demonstration, a combination of the ase and abtem packages will be used to generate material, slice it into layers, and ultimately calculate the electric potentials.
