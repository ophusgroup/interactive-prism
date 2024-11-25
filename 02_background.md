---
title: PRISMs Alternatives
math:
  \ii: '{i\mkern1mu}'
  \invFFT: \mathcal{F}^{-1}_{\mathbf{k}\to\mathbf{r}}
  \FFT: \mathcal{F}_{\mathbf{r}\to\mathbf{k}}
  \angstroms: \text{\normalfont\AA}
---

# Overview

Historically, there have been two primary methods used to simulate elastic scattering of electrons interacting with a sample in a TEM:
1. The Bloch wave method.
2. The mutlislice methods.

For the reader interested in understanding more about these please see the [Kirkland Textbook](doi:10.1007/978-1-4419-6533-2), as we will primarily do an overview of the principles and primary limitations of each algorithm. In both algorithms, the goal is to compute how the electron wavefunction evolves as it progresses through a three-dimensional sample. 

## Mathematical Representations

Following [Kirkland](doi:10.1007/978-1-4419-6533-2), we first write the Schrödinger equation which describes the evolution along the $z$-axis for an electron wavefunction $\psi(\bm{r})$ as

```{math}
:label: eq:Shrodinger_electron
\frac{\partial }{\partial z} \psi(\bm{r})
    =
    \frac{\ii \lambda}{4 \pi} {\nabla_{xy}}^2 \psi(\bm{r})
    + 
    \ii \sigma V(\bm{r}) \psi(\bm{r}),
```
where $\ii$ is the imaginary constant, $\lambda$ is the relativistically-corrected electron wavelength, ${\nabla_{xy}}^2 $ is the @wiki:Laplace_operator, $\sigma$ is the electron-matter interaction constant for a given wavelength, and $V(\bm{r})$ is the electrostatic potential of a sample. Each algorithm we consider is a solution to this equation.


## Bloch Waves

### Formalism and ansatz

Bloch waves are solutions to the Schrödinger equation in a periodic potential, such as a crystal lattice. They take the form of a set of plane waves modulated by a periodic function, reflecting the symmetry of the lattice, and are expressed as:
$$
\psi(\bm{r}) = u(\bm{r}) e^{\ii 2 \pi \bm{k} \cdot \bm{r}},
$$
where $u(\bm{r})$ has the same periodicity as the lattice. We assume our solution a has the form (i.e. we assume a Bloch wave ansatz)
$$
\psi(\bm{r}) = \sum_{\bm{g}} c_{\bm{g}}(z) e^{\ii 2 \pi (\bm{g} + \bm{k}) \cdot \bm{r}},
$$
where $\bm{g}$ are reciprocal lattice vectors, $\bm{k}$ is the wavevector, and $c_{\bm{g}}(z)$ are the coefficients describing the evolution of the wave along $z$. 
We assume rhe potential $V(\bm{r})$ is periodic and can be expressed as:
$$
V(\bm{r}) = \sum_{\bm{g}} V_{\bm{g}} e^{\ii 2 \pi \bm{g} \cdot \bm{r}},
$$

with $V_{\bm{g}}$ being the Fourier components of $V(\bm{r})$. Substituting $\psi(\bm{r})$ and $V(\bm{r})$ into the two parts of the Schrödinger equation:

### Laplacian term

The transverse Laplacian acts on the plane wave component $e^{\ii 2 \pi (\bm{g} + \bm{k}) \cdot \bm{r}}$:

$$
\nabla_{xy}^2 e^{\ii 2 \pi (\bm{g} + \bm{k}) \cdot \bm{r}} = -(2 \pi)^2 |\bm{g}_{xy} + \bm{k}_{xy}|^2 e^{\ii 2 \pi (\bm{g} + \bm{k}) \cdot \bm{r}},
$$

so:

$$
\frac{\ii \lambda}{4 \pi} \nabla_{xy}^2 \psi(\bm{r}) = -\ii \lambda \pi \sum_{\bm{g}} |\bm{g}_{xy} + \bm{k}_{xy}|^2 c_{\bm{g}}(z) e^{\ii 2 \pi (\bm{g} + \bm{k}) \cdot \bm{r}}.
$$

### Potential term

The product $V(\bm{r}) \psi(\bm{r})$ leads to:

$$
V(\bm{r}) \psi(\bm{r}) = \sum_{\bm{g}, \bm{g}'} V_{\bm{g}} c_{\bm{g}'}(z) e^{\ii 2 \pi (\bm{g} + \bm{g}' + \bm{k}) \cdot \bm{r}}.
$$

### Combined terms

Substituting these into the Schrödinger equation, and projecting onto a specific plane wave $e^{\ii 2 \pi (\bm{g} + \bm{k}) \cdot \bm{r}}$ isolates the coefficients $c_{\bm{g}}(z)$:

$$
\frac{\partial}{\partial z} c_{\bm{g}}(z) = -\ii \lambda \pi |\bm{g}_{xy} + \bm{k}_{xy}|^2 c_{\bm{g}}(z) + \ii \sigma \sum_{\bm{g}'} V_{\bm{g} - \bm{g}'} c_{\bm{g}'}(z).
$$


### Structure Matrix Formulation

We rewrite the evolution equation for the coefficients $c_{\bm{g}}(z)$ in matrix form. Define the column vector of coefficients as:

$$
\mathbf{C}(z) = \begin{bmatrix}
c_{\bm{g}_1}(z) \\
c_{\bm{g}_2}(z) \\
\vdots
\end{bmatrix},
$$

where $\bm{g}_1, \bm{g}_2, \dots$ represent the reciprocal lattice vectors. The evolution equation then becomes:

$$
\frac{\partial}{\partial z} \mathbf{C}(z) = \mathbf{A} \mathbf{C}(z),
$$

where the $\mathbf{A}$-matrix, often referred to as the structure matrix, has elements given by:

$$
A_{\bm{g}, \bm{g}'} =
-\ii \lambda \pi |\bm{g}_{xy} + \bm{k}_{xy}|^2 \delta_{\bm{g}, \bm{g}'} +
\ii \sigma V_{\bm{g} - \bm{g}'}.
$$

Here:
- The diagonal terms $-\ii \lambda \pi |\bm{g}_{xy} + \bm{k}_{xy}|^2$ account for the free-space dispersion of each wave component.
- The off-diagonal terms $\ii \sigma V_{\bm{g} - \bm{g}'}$ describe coupling between different components due to the periodic potential.

Solving this matrix equation is the most computationally expensive part of a Bloch wave calculation. Once we have obtained the $$


### Solving for the Wavefunction

For a general incident wavefunction, such as a STEM probe, the initial wavefunction can be expanded as:

$$
\psi_0(\bm{r}) = \sum_{\bm{g}} c_{\bm{g}}(0) e^{\ii 2\pi (\bm{g} + \bm{k}) \cdot \bm{r}},
$$

where the initial coefficients $c_{\bm{g}}(0)$ are determined by the Fourier transform of $\psi_0(\bm{r})$, and the solution is given by

$$
\mathbf{C}(z) = \exp(\mathbf{A} z) \mathbf{C}(0).
$$

Here, $\mathbf{C}(0)$ contains the initial coefficients $c_{\bm{g}}(0)$, and $\mathbf{A}$ is the structure matrix. The propagated wavefunction is given by:

$$
\psi(\bm{r}) = \sum_{\bm{g}} c_{\bm{g}}(z) e^{\ii 2\pi (\bm{g} + \bm{k}) \cdot \bm{r}}.
$$

### Advantages

- Exact solution for 3D evolution of electron waves.
- Handles multiple scattering / dynamical diffraction, and can simulate additional sample thicknesses with very cheap calculations.
- Reduced computational complexity by only computing scattering on a finite set of reciprocal lattice vectors which match the parent crystals.


### Disadvantages

- Can only be used for periodic samples.
- Very computationally expensive for samples with a large volume (those with dimensions more than a few unit cells).




## Multislice

In the multislice method, originally proposed by [Cowley and Moodie](https://doi.org/10.1107/S0567739468000653), we model electron scattering by propagating the wavefunction step-by-step through the sample, alternating between transmission through a thin slice and free space propagation between slices.


