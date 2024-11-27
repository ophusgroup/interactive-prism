---
title: Overview of PRISM Ideas
---

## Main Takeaways
The ideas that are necessary to make that happen are a good understanding of fourier manipulations in the frequency domain and how to translate between each of those steps.


## Review of Intuitions and General Process
### Overall Process
1. Generate Slices
2. Sample Pupil
3. Propogate Plane Waves to Create S-Matrix
4. Calculate Probe Weighting
5. Simulate a STEM probe
6. Repeate steps 4 and 5 until completed simulation

## Efficiency Gains from using PRISM
Overall, the PRISM algorithm uses the fact that the frequencies propagated through have a linear response to create efficiencies in computing a large amount of STEM data. Below see a table of the step by step comparison of what is being done in a traditional vs multislice methodology.

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



<!-- (Make a powerpoint diagram to show how we can propagate through the system using STEM with a representative TEM diagram next to it) -->
