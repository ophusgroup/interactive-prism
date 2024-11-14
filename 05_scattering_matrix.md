---
title: Power of the S Matrix
---







## Calculation of Scattering Matrix
Once we know which plane waves represent the spatial frequencies we will include we can construct the S-matrix by propagating each plane wave through the sample. The final matrix is the sample's width and height and the depth of the number of frequencies probed. Please see a representative depiction of each of the plane waves after passing through amorphous tantalum.

(Insert Image of Amporphus Tantalum projection image of the central spatial frequencies)

## Abberations and Probe Location for Calculating Weightings
The beauty of the scattering matrix is that once it is constructed it can be used extremely efficiently to compute many probe positions by simply sampling and applying the weights accordingly. These weights are composed of two parts the first are the aberrations intentionally or unintentionally in the beam and the latter is the phase to be applied for offsets from the exact pixel spacing. Bellow we have a demonstration to allow you to get a feel for what these aberrations look like when applied to the probe and as well as a demonstration of how one can apply a phase offset in the frequency plane to achieve a lateral translation in the spatial domain.

(Demonstration of the abberations)

(Demonstration of lateral translation using frequency response in fourier domain)

## Generating STEM data
Final movie of STEM data being generated with the S matrix and abberation simulator
Once we have defined functions for creating a matrix representation of the material, the S matrix, and of the particular probe position, the weightings, we can reduce the creation of one STEM position measurement to the multiplication of a new weighting with the S matrix we have created.
