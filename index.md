---
title: Interactive Guide to PRISM
short_title: Interactive Guide to PRISM
numbering:
  heading_2: false
---




+++ {"part": "abstract"} 

Simulating STEM images at atomic resolution for samples with realistic dimensions is computationally intensive using traditional algorithms. The PRISM algorithm bridges the gap between the Bloch wave and multislice methods, offering a highly efficient approach for STEM simulations. By introducing a Fourier interpolation factor $f$, with typical values of $f=2–20$ for atomic-resolution imaging, PRISM achieves speedups that scale as $f^2$ to $f^4$, compared to standard multislice methods, with minimal loss of accuracy. In this interactive demonstration paper, we help readers explore the principles behind the PRISM algorithm, understand how it achieves simulation speedups, and visualize the trade-offs between speed and accuracy.

This article is an interactive reimagining of the peer-reviewed manuscript [A fast image simulation algorithm for scanning transmission electron microscopy](https://doi.org/10.1186/s40679-017-0046-1).

+++





<!-- +++{"part":"epigraph"}
:::{warning} Pre-print
This article has not yet been peer-reviewed.  
_Updated 2024 August 27_
::: -->

+++

+++ {"part": "acknowledgements"} 


CO thanks Earl Kirkland, Christoph Koch, and Roar Kilaas for their helpful discussions on (S)TEM simulation methods, and thanks Hao Yang, Jim Ciston, Tyler Harvey, and Peter Ercius for their helpful suggestions on this manuscript.


+++

+++ {"part": "competing interests"} 
## Competing Interests

The authors declare no competing interests.

+++
