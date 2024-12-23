---
title: Power of the S Matrix
---







## Calculation of Scattering Matrix
Once we know which plane waves represent the spatial frequencies we will include we can construct the S-matrix by propagating each plane wave through the sample. The final matrix is the sample's width and height and the depth of the number of frequencies probed. Please see a representative depiction of each of the plane waves after passing through silicon.

:::{figure} ./figures/propagated_plane_waves.gif
:name: Probe after propagating through sample
:alt: Probe after propagating through sample
:align: center
Probe after propogating through sample
:::

## Abberations and Probe Location for Calculating Weightings
The beauty of the scattering matrix is that once it is constructed it can be used extremely efficiently to compute many probe positions by simply sampling and applying the weights accordingly. These weights are composed of two parts the first are the aberrations intentionally or unintentionally in the beam and the latter is the phase to be applied for offsets from the exact pixel spacing. Bellow we have a demonstration to allow you to get a feel for what these aberrations look like when applied to the probe and as well as a demonstration of how one can apply a phase offset in the frequency plane to achieve a lateral translation in the spatial domain.

:::{figure} #app:abberations
:name: Abberations Demonstration
:placeholder: ./figures/abberations.png
:alt: Demonstration of defocus and spherical
:align: center
Spherical and Defocus abberations simulator
:::


:::{figure} #app:linear_phase_shift
:name: Linear Phase Shift
:placeholder: ./figures/linear_phase_shift.png
:alt: Show linear phase shift movement
:align: center
Movement coming from linear phase shift
:::

## Scattering Matrix and Probe Vector Multiplication
One of the nice ideas that is presented in the PRISM paper is an idea that we can create an 3D array in which two of the axes create the real space plane waves and the third axis is effectively a Fourier domain because each depth corresponds to a unique spatial frequency. This means that in order to apply the necessary probe weighting in Fourier space we can simply multiply the phase component to along that axis associated with the pupil phase we anticipate.

(Diagram Coming soon)


## Final Output at STEM Probe
Final movie of STEM data being generated with the S matrix and abberation simulator
Once we have defined functions for creating a matrix representation of the material, the S matrix, and of the particular probe position, the weightings, we can reduce the creation of one STEM position measurement to the multiplication of a new weighting with the S matrix we have created.

:::{figure} ./figures/si_probe_ex.png
:name: Probe after propagating through sample
:alt: Probe after propagating through sample
:align: center
Probe after propagating through sample
:::