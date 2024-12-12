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
One of the features of the Fourier transform is that it is linear which means one can work with each of the components individually and then add up the result later. In the case of PRISM, we take advantage of this fact to propagate each of the plane waves separately so we can apply weightings to them after they have been propagated through the sample to make use of one multislice propagation many times. The construction of a probe using each of the spatial frequencies can be seen and demonstrated in the gif below. In the gif/interactive demo below do you notice anything that changes as more spatial frequencies are added? (See an explanation below of a few things that you can observe)

:::{figure} #app:time_evolution
:name: Probe Construction
:placeholder: ./figures/probe_construction.gif
:alt: Demonstration of the construction of a probe by slowly adding spatial frequencies (downsampled f = 8)
:align: center
Demonstration of the construction of a probe by slowly adding spatial frequencies (downsampled f = 8)
:::

:::{dropdown} Observations
:close:
### Size of Spot:
One key parameter that one can observe from the plot is the reduction of the primary spot as the rings are added to the pupil. This process is analogous to increasing the numerical aperture of a particular system which by the raleigh criterion one would expect to decrease the spot size.

### Increase in Detail:
The second key thing to observe is the most obvious which is that as we add spatial frequencies detail increases.

### Duplications of the Spot:
This occurs due to the way that the frequencies are evenly spaced out by a particular spacing and will be discussed further in the next section.

:::

## $f$ Sampling
So far the previous two sections have been focusing on trying to understand how the probe is formed and taking a look at how spatial frequencies work. In this section we're going to consider what downsampling looks like in Fourier Space as it is one of the main techniques that the PRISM algorithm uses. The PRISM takes advantage of downsampling the probe to reduce the computational burden of calculating as many frequencies as there is often a lot of redundant information present in the beam. What this is saying is that one can get a pretty good representation of the beam without some of the detail provided by having similar frequencies which you saw earlier in the probe construction demonstration. When done evenly this leads to an interesting effect that appears like a duplication for every factor $f$ of the object is downsampled. You can play with this downsampling and how it impacts this repetition of the probe below. Take a look at factors of 1,2,4,8,15 as these will help with showing without too many artifacts as they allow for a perfect splitting of the frequency space (840px by 840px) in the demo. 

:::{figure} #app:spatial_frequency_widget
:name: $f$ Sampling Example
:placeholder: ./figures/static_spatial_frequency_sampling.png
Downsampling by $f$ in frequency space example
:::

:::{note}
An f factor that is equal to one is the equivalent of the multislice algorithm and should reproduce the same output
:::

While downsampling in principle leads to a loss of information one can simulate nearly identical outputs with far fewer frequencies up until a point in which the information from scattering starts to cross from one of these repeated probe areas into another which can be seen in the [original paper by C Ophus](doi:10.1186/s40679-017-0046-1). Now that we have an intuition for frequency space and generally understand how downsampling works lets move on to the scattering matrix where we start to propogate each of these frequencies.