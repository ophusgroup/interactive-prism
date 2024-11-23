---
title: Introduction to PRISM
---

## Overview

The purpose of the Interactive Guide to PRISM is to introduce the components that make this algorithm possible and allow those new to it to build intuitions about how it functions. This guide has small simulations that you are encouraged to play with and try to put the often intimidating Fourier math into practice. Hopefully, by the end of it you’ll have built an understanding of what is going on under the hood and maybe learned a thing or two about computational imaging algorithms and linear systems.


## What is PRISM? (CO)

PRISM is an acronym which stands for `Plane Wave Interpolated Scattering Matrix`. Don't worry if you don't understand what each of these terms means yet - this guide will cover all of them! PRISM is a numerical algorithm for simulating scanning transmission electron microscopy (STEM) experiments. The conventonal STEM simulation method we use is named the multislice method, where we use the @wiki:Schrodinger_equation to solve the scattering of an electron wave inside a sample. We use the PRISM algorithm to perform the same kinds of simulations.


## What does PRISM do?

The PRISM algorithm improves the simulation speed of STEM simulations. It does this by downsampling the probe in reciprocal (or spatial frequency) space propagating each spatial frequency separately over the whole sample and constructing a set of new probe scattering images by recombining these spatial frequencies over the subregions of the sample a real probe would interact with. This procedure allows for efficiencies over previous algorithms in two ways:

1. By reducing the sampling in Fourier space / reducing the bandwidth in real space (equivalent).  
2. By sharing common computation information across many probe positions. 

Please continue on to the next section to learn more about the fundamentals or skip ahead to multislice visualization to get right into the interactivity.

:::{figure} ./figures/colinPaperPRISMdemo.png
:name: PRISM outline from paper
:alt: Frequency space with coordinates replaced with plane waves of equivalent frequency
:align: center
PRISM outline from original ophus paper
:::