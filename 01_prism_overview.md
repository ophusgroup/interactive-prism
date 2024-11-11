---
title: Introduction to PRISM
---
## Introduction to Guide
The purpose of the Interactive Guide to PRISM is to introduce the components that make this algorithm possible and allow those new to it to build intuitions about how it functions. This guide has small simulations that you are encouraged to play with and try to put the often intimidating Fourier math into practice. Hopefully, by the end of it you’ll have built an understanding of what is going on under the hood and maybe learned a thing or two about computational imaging algorithms and linear systems.

## What does PRISM do?
The PRISM algorithm is a method for improving the simulation speed of STEM simulations. It does this by downsampling the probe in reciprocal (or spatial frequency) space propagating each spatial frequency separately over the whole sample and constructing a set of new probe scattering images by recombining these spatial frequencies over the subregions of the sample a real probe would interact with. This procedure allows for efficiencies over previous algorithms in two ways the first is the most straightforward of downsampling and the latter is the ability to share common computation information across many probe positions. Please continue on to the next section to learn more about the fundamentals or skip ahead to multislice visualization to get right into the interactivity.

:::{figure} ./figures/colinPaperPRISMdemo.png
:name: PRISM outline from paper
:alt: Frequency space with coordinates replaced with plane waves of equivalent frequency
:align: center
PRISM outline from original ophus paper
:::