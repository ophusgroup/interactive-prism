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

Following [Kirkland](doi:10.1007/978-1-4419-6533-2), we first write the Schrödinger equation which describes the evolution along the $z$-axis for an electron wavefunction $\psi(\bm{r})$ over spatial coordinates $\bm{r}=(x,y,z)$ as

```{math}
:label: eq:Shrodinger_electron
\frac{\partial }{\partial z} \psi(\bm{r})
    =
    \frac{\ii \lambda}{4 \pi} {\nabla_{xy}}^2 \psi(\bm{r})
    + 
    \ii \sigma V(\bm{r}) \psi(\bm{r}),
```
where $\ii$ is the imaginary constant, $\lambda$ is the relativistically-corrected electron wavelength, ${\nabla_{xy}}^2 $ is the @wiki:Laplace_operator, $\sigma$ is the electron-matter interaction constant for a given wavelength, and $V(\bm{r})$ is the electrostatic potential of a sample. This equation is a @wiki:Partial_differential_equation (PDE) which is far too complex to solve analytically. It is difficult to solve because the two operators on the right side do satisfy the @wiki:Commutative_property, meaning that the order matters. Each algorithm we consider is a solution to this equation.


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




## Multislice Method

The multislice method, originally proposed by [Cowley and Moodie](https://doi.org/10.1107/S0365110X57002194), models electron scattering by propagating the wavefunction step-by-step through the sample. We compute the scattering by alternating between transmission through a thin slice and free space propagation between slices. The multislice solution to Equation {eq}`eq:Shrodinger_electron` is similar to a @wiki:Split-step_method. The core idea is to remove one of the two operators on the right side of Equation {eq}`eq:Shrodinger_electron`, producing two new PDEs which solve transmission and free space propagation separately.


### Transmission Operator

Ignoring propagation, we can write Equation {eq}`eq:Shrodinger_electron` as
```{math}
:label: eq:transmission
\frac{\partial }{\partial z} \psi(\bm{r})
    =
    \ii \sigma V(\bm{r}) \psi(\bm{r}).
```

Following [Kirkland](doi:10.1007/978-1-4419-6533-2), we can write the solution to this equation as 

```{math}
:label: eq:transmission_step
\psi(\bm{r}) 
=
\exp 
  \left\{
  \int_{z_0}^{z_0 + \Delta z} 
  \ii \sigma 
  V(\bm{r})
  dz
  \right\}
\psi_0(\bm{r}),
```
where $\Delta z$ is the thickness of a single slice, and $\psi_0(\bm{r})$ is the wavefunction before transmission. If we compress all of the sample potential inside this slice into an infinitely thin 2D projected potential $V_{\rm{2D}}(\bm{r}) = \int_{z}^{z+\Delta z} V(\bm{r})\,dz$, we can write the solution as 
```{math}
:label: eq:transmission_operator
\psi(\bm{r}) =
\psi_0(\bm{r}) \exp[
  \ii \sigma V_{\rm{2D}}(\bm{r})
].
```
This expression, known as the `transmission operator`, shows that the electron wavefunction gains a forward phase shift proportional to the potential $V_{\rm{2D}}(\bm{r})$ of the slice.



### Propagation Operator

Setting the sample potential $V(\bm{r})=0$, we can write Equation {eq}`eq:Shrodinger_electron` as
```{math}
:label: eq:transmission
\frac{\partial }{\partial z} \psi(\bm{r})
    =
\frac{\ii \lambda}{4 \pi} {\nabla_{xy}}^2 \psi(\bm{r}).
```

Setting $\Lambda = \lambda \Delta z / 4 \pi$ and Taylor expanding this expression gives
```{math}
:label: eq:prop02
\psi(\bm{r})
    = 
    \left[
      \sum_{m=0}^\infty 
      (\ii \Lambda)^m 
      \frac{\partial^{2m} \psi_0(\bm{r})}{\partial x^{2m}} 
    \right]
    \left[
      \sum_{n=0}^\infty 
      (\ii \Lambda)^n 
      \frac{\partial^{2n} \psi_0(\bm{r})}{\partial y^{2n}} 
    \right].
```
Taking the 2D Fourier transform $\Psi(\bm{k}) = \mathscr{F}_{\bm{r} \rightarrow \bm{k}}\{ \psi(\bm{r}) \}$ of both sides and using the fact that the x and y derivatives are orthogonal, we get
```{math}
:label: eq:prop01
\begin{aligned}
\Psi(\bm{k})

    &= 
    \left[
      \sum_{m=0}^\infty 
      (\ii \Lambda)^m 
      (\ii 2 \pi k_x)^{2m}
    \right]
    \left[
      \sum_{n=0}^\infty 
      (\ii \Lambda)^n 
      (\ii 2 \pi k_y)^{2n}
    \right]
    \Psi_0(\bm{k}) \\
    
    &=
    \left[
      \sum_{m=0}^\infty 
      (-\ii 4 \pi^2 \Lambda {k_x}^2)^m 
    \right]
    \left[
      \sum_{m=0}^\infty 
      (-\ii 4 \pi^2 \Lambda {k_y}^2)^m 
    \right]
    \Psi_0(\bm{k}) \\

    &=
    \left[
      \sum_{m=0}^\infty 
      (-\ii \pi \lambda \Delta z {k_x}^2)^m 
    \right]
    \left[
      \sum_{m=0}^\infty 
      (-\ii \pi \lambda \Delta z {k_y}^2)^m 
    \right]
    \Psi_0(\bm{k}) \\

    &=
    \exp\left(
      -\ii \pi \lambda \Delta z {k_x}^2 
    \right)
    \exp\left(
      -\ii \pi \lambda \Delta z {k_y}^2
    \right)
    \Psi_0(\bm{k}).
\end{aligned}
```
We can now write the final `propagation operator` by combining ${k_x}^2+{k_y}^2=|\bm{k}|^2$ to give
```{math}
:label: eq:prop
\Psi(\bm{k})
  =
  \exp\left(
    -\ii \pi \lambda \Delta z |\bm{k}|^2 
  \right)
  \Psi_0(\bm{k}).
```

In the multislice method, this operator is applied iteratively between slices to propagate the wavefunction through the sample. Once the electron wavefunction has passed through the full sample, we use a detector function to compute the resulting output measurement.


