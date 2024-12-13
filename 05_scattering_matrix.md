---
title: Power of the S Matrix
---


## Calculation of Scattering Matrix
Once we know which plane waves represent the spatial frequencies we can construct the S-matrix by propagating each plane wave through the sample. The final matrix is the sample's width and height and the depth of the number of frequencies probed. Below is a gif that is scanning through a set of these plane waves after propogating through the silicon sample shown in previous sections.

:::{figure} ./figures/propagated_plane_waves.gif
:name: Probe after propagating through sample
:alt: Probe after propagating through sample
:align: center
Probe after propogating through sample
:::

## Abberations and Probe Location for Calculating Weightings
The beauty of the scattering matrix is that once it is constructed it can be used extremely efficiently to compute many probe positions by simply sampling and applying the weights accordingly. These weights are composed of two parts the first are the aberrations intentionally or unintentionally in the beam and the latter is the phase to be applied for offsets from the exact pixel spacing. Some of the most common abberations that are often seen in an electron microscope are defocus, spherical and astigmatism and below is a representation of what these abberations look like in frequency space. Feel free to adjust the knobs yourself and see how different combinations of abberations can lead to different patterns.

:::{figure} #app:abberations
:name: Abberations Demonstration
:placeholder: ./figures/abberations.png
:alt: Demonstration of defocus and spherical
:align: center
Spherical and Defocus abberations simulator
:::

The second part noted earlier is the phase shift needed to be applied to have precise control over the position you'd like to sample the beam at. The reason that we need to apply a phase shift is that the position of the beam in real space is encoded into the Fourier space as a linear phase term. Below you can see how by applying a linear phase to the frequency space there is a translation in the real space probe location.

:::{figure} #app:linear_phase_shift
:name: Linear Phase Shift
:placeholder: ./figures/linear_phase_shift.png
:alt: Show linear phase shift movement
:align: center
Movement coming from linear phase shift
:::

## Scattering Matrix and Probe Vector Multiplication
Once these two components have been computed we're ready to compute an individual probe location. One might think that you need to convert the real space plane waves into frequency space to apply these values however the beauty of the scattering matrix is that the third dimension is essentialy already in frequency space as each plane wave corresponds to a point in frequency space. So to compute the final matrix it is as simple as multiplying a probe vector along that axis.

(Diagram Coming soon)

That results in a single probe position an example of which can be seen below. To create another probe simulation just alter the weighting of the vector and quickly create a large set of data.

:::{figure} ./figures/si_probe_ex.png
:name: Probe after propagating through sample
:alt: Probe after propagating through sample
:align: center
Probe after propagating through sample
:::