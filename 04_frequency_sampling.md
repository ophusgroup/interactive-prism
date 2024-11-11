---
title: Spatial Frequency Intuition
---

## Overview
The crux of the PRISM algorithm efficiency gains can be found in the downsampling of frequency space, propagating each spatial frequency through the sample, and reconstructing them together later. This is made possible because Fourier propagations are linear. This means that if we perform a function on all of the parts of an object we can add them together at the end to get the same thing as if we Fourier transformed the entire object. Below are some demonstrations designed to help you gain an intuitive understanding of how spatial frequencies work.

## What are spatial frequencies?
To start, we will try to build an understanding of what spatial frequencies are and an intuition for how Fourier transforms help us translate between real and frequency space. Spatial frequencies are essentially a bunch of sinusoidal patterns in real space that have certain orientations and frequencies. A single point in frequency space is represented by one of these sinusoids and a Fourier transform is a method to convert any representation of an object in our traditional spatial understanding to a new representation where that object is constructed of a bunch of weighted sinusoidal patterns. Figure 1 demonstrates how one might think of points in spatial frequency space where as you move away from the center of the Fourier transform the frequencies increase and the sinusoid is perpendicular to the direction of the point.


:::{figure} ./figures/planeWaves.png
:name: Plane wave representation of frequency coordinates
:alt: Frequency space with coordinates replaced with plane waves of equivalent frequency
:align: center
Frequency space with coordinates replaced with plane waves of equivalent frequency
:::

## Probe Construction
Since this process is linear one can work with each of the components individually and then add up the result later. In the case of PRISM, we take advantage of this fact to propagate each of the plane waves separately so we can apply weightings to them after they have been propagated through the plane to make use of one multislice propagation many times. The construction of a probe using each of the spatial frequencies can be seen and demonstrated in the gif below.

:::{figure} ./figures/probe_construction.gif
:name: Probe Construction
:alt: Demonstration of the construction of a probe by slowly adding spatial frequencies (downsampled f = 8)
:align: center
Demonstration of the construction of a probe by slowly adding spatial frequencies (downsampled f = 8)
:::

## F Sampling
The final thing that PRISM takes advantage of is that when one omits frequencies evenly one can downsample the probe which reduces the computational burden of calculating each of these frequencies individually. What this is saying is that one can get a pretty good representation of the beam without some of the detail provided by having similar frequencies. This can be seen very clearly above in the construction of the probe where the probe plane takes shape very quickly with limited frequencies and the details can slowly fill in. When done evenly this leads to an interesting effect that appears like a duplication for every factor f of 2 is downsampled. You can play with this downsampling and how it impacts this repetition of the probe below. Take a look at factors of 2,4,8,16 as these will be the most apparent as the sampling is perfect even in these cases.

:::{figure} #app:spatial_frequency_widget
:name: F Sampling Example
:placeholder: ./figures/static_spatial_frequency_sampling.png
Downsampling by f in frequency space example
:::

