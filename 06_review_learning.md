---
title: Overview of PRISM Ideas
---

## General Process
1. Generate Slices
2. Sample Pupil
3. Propogate Plane Waves to Create S-Matrix
4. Calculate Probe Weighting
5. Simulate a STEM probe
6. Repeate steps 4 and 5 until completed simulation

## Efficiency Gains from using PRISM
Overall, the PRISM algorithm uses the fact that the frequencies propagated through have a linear response to create efficiencies in computing a large amount of STEM data. Below see a table of the step by step comparison of what is being done in a traditional vs multislice methodology. Once can see that the primary driver of the efficiency of using PRISM vs multislice is the number of probe positions that you will need to compute as this is where the efficiency gain is to be had. 

:::{figure} ./figures/algorithmCompVariableImage.png
:name: Variable Highlight for Algorithm Comparison of Multislice and PRISM
:alt: Visualizing the different variables in the algorithm comparison 
:align: center
Demonstration of various variables used for algorithm comparison N is the array width of the frequency space, 
B is the number of frequencies that fit within reciprocal space aperture when modeled,
f is the sampling factor previously described
:::

:::{figure} ./figures/algorithmComparisonTable.png
:name: Comparison of Multislice and PRISM algorithms computational complexity
:alt: Comparison of Multislice and PRISM algorithms computational complexity
:align: center
Comparison of Multislice and PRISM algorithms computational complexity, in addition to the variables noted above, H represents the number of slice layers of potential to propgate through.
:::


## Main Takeaways
The PRISM algorithm leverages a few important principles in order to get efficiency gains. The first is the fact that the simulation can be considered a linear system so each of the frequencies can be summed either before like in the multislice algorithm before propogation or after as is done in the PRISM implementation. Then with this knowledge the algorithm itself leverages a few linear algebra tricks and fourier intuitions in order to optimize the computational efficiency by using the S-matrix with a probe vector and also by acknowledging that in STEM data most of the information is contained with in a much smaller region than the full width of the image plane. 

## Using the PRISM algorithm
When deciding to use the PRISM algorithm there are a few parameters to keep in mind. The first is the f sampling of the object you have is limited by the spread of the final image not exceeding the width of the reduces square created by f sampling as this will cause wrap around effects. This is demonstrated in figure 3 of the [original paper by C Ophus](doi:10.1186/s40679-017-0046-1). The latter is to understand if there will be many probe positions that will be used as this will dictate your decision to use it or not. In most STEM imaging situations the amount of probes quickly leads to an improvement in the overall performance of STEM data generation. From taking the time to go through these interactive demonstrations I hope that you have gained some intuitions for how this algorithm works, why it is useful and potentially learned a thing or two that you can take with you.


<!-- (Make a powerpoint diagram to show how we can propagate through the system using STEM with a representative TEM diagram next to it) -->
