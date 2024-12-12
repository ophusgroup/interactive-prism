---
title: Visualizing Multislice
---

## Overview

In PRISM we use the multi-slice algorithm to propagate the probe through the sample. To do this we first must load in or generate a sample to measure and slice it into atom-thin chunks that we calculate the electric potentials of. Then we can propagate through the slices in alternating steps where we calculate the phase from the interaction of the electrons using the potential slices and then propagate the spacing between the z planes using a fresnel propagation. We can propagate using this method through the sample until we achieve the final diffraction pattern. 

## Building and Slicing Material:

To get started on slicing, one first must load or generate a sample which is often defined in terms of the coordinates of atoms as well as the atoms themselves. Once one has a sample one can slice the material into thin layers of sheets of atoms in the z-direction. It is important to slice at least as thin as the spacing between planes of atoms so that all of the atoms are incorporated properly. If one would like to the algorithm allows for varying slicing thicknesses as long as the thicknesses are then accounted for during the propagation step later. Below is a representative plane of atoms that would be accounted for in a Si sample which is generated using the [ase](https://wiki.fysik.dtu.dk/ase/) python package.

:::{figure} ./figures/silicon_slice.png
:name: Generation of Silicon using ase
:alt: Slice of a Silicon Layer
:align: center
Generation of Silicon sample using the `ase` package.
:::

## Calculating the Electronic Potential:
Once one has created sheets of atoms the electric potentials across each of the sheets can be computed using the methods found in the kirkland textbook. By understanding what the electrical potentials are at each slice one can know how the electrons will be affected by each slice in the multislice algorithm. An example of what this potential looks like is shown below for the same silicon sample shown above and is calculated by using the [abtem](https://abtem.readthedocs.io/en/latest/intro.html) python package.

:::{figure} ./figures/pot_array.png
:name: Single Potential Slice of Silicon
:alt: Slice of a Silicon Layer
:align: center
Single potential Slice of Silicon calculated using kirkland algorithm in abtem package
:::

Once these electric potentials have been calculated for all of the slices one is ready to perform simulated measurements of the material which we will begin to discuss in the following section.
