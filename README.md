# Physics-Informed Neural Networks for Chameleon Screening

**Research code and numerical benchmarks**  
**Evangelos I. Sfakianakis**

This repository documents work on physics-informed neural networks (PINNs) for nonlinear chameleon screening, including numerical validation of scalar-field solutions and fifth-force gradients, parameter-conditioned surrogates, and related research presentations.

## Physics and numerical challenge

Chameleon models exhibit density-dependent scalar screening. In the dimensionless benchmark with potential index $n=3$, the static field equation is

$$
\nabla^2\varphi=-\varphi^{-4}+q(\mathbf{x}).
$$

Sharp interfaces between dense matter and vacuum generate thin boundary layers, making gradient accuracy particularly important for fifth-force predictions. Our high-contrast benchmarks use a spherical source inside a concentric vacuum chamber, with

$$
\rho_{\mathrm{metal}}/\rho_{\mathrm{vac}}=10^{17}.
$$

The neural networks solve the axisymmetric field equation in cylindrical spatial coordinates $(r,z)$; the independent reference is a one-dimensional spherical boundary-value problem (BVP). Although the source is spherical, spherical symmetry is not imposed on the neural-network correction.

## Preliminary benchmark results

| Benchmark | Vacuum field relative $L^2$ error | Vacuum radial-gradient relative $L^2$ error |
| --- | ---: | ---: |
| Single-configuration, 2D axisymmetric PINN | **0.088%** | **0.557%** |
| Radius-conditioned, 2D axisymmetric PINN at held-out $R_{\mathrm{obj}}=0.25$ | **0.057%** | **0.390%** |

Errors are evaluated against independently computed spherical BVP solutions, using volume-sampled points in the vacuum region. BVP solutions are **not used as training targets**. The gradient is the relevant fifth-force proxy for the fixed matter coupling in this benchmark.

The radius-conditioned surrogate covers $R_{\mathrm{obj}}\in[0.20,0.30]$. The interval $0.245<R_{\mathrm{obj}}<0.255$ was excluded from training and physics-validation sampling. The result at $R_{\mathrm{obj}}=0.25$ therefore tests **interpolation across a held-out parameter interval**, not extrapolation to a new physical regime.

## Physics-enriched PINN approaches

We have explored complementary approaches to sharp-interface screening:

1. Domain decomposition with analytically informed interface matching.
2. Global-network pretraining on a semi-analytic field profile.
3. A global neural correction to an explicit semi-analytic Compton-screening baseline.

The third approach provides the strongest accuracy in the reported high-contrast single-configuration benchmark and is also the basis of the radius-conditioned surrogate.

## Repository materials

- **Research poster:** *Chameleon Solutions with Physics-Informed Neural Networks*, presented at the 2026 NSF IAIFI Summer Workshop. This earlier proof of concept includes radial solutions and a three-dimensional two-sphere surrogate at moderate density contrasts (approximately $10^3$) with smoothed interfaces. It is distinct from the subsequent $10^{17}$ sharp-interface benchmarks.
- **Colab notebooks:** High-accuracy PINN and radius-conditioned surrogate implementations, with validation scripts and saved numerical diagnostics, will be added here.
- **Benchmark figures:** Additional validation plots will be added alongside the notebooks.

## Running the notebooks

The notebooks are designed for Google Colab with Python, PyTorch, NumPy, SciPy, pandas, and Matplotlib. A CUDA GPU is recommended for full training, which uses double-precision automatic differentiation and second spatial derivatives. Once notebooks are uploaded, open them in Colab and follow their setup and execution cells.

## Scope and reproducibility

These are preliminary research benchmarks, not a claim that PINNs outperform conventional numerical methods in all regimes. In particular:

- The current high-contrast surrogate varies **one geometric parameter** while retaining a spherical physical configuration.
- Its held-out test assesses interpolation within the trained parameter range.
- Reported surrogate accuracy comes from a single training run; broader seed and interface-convergence studies remain useful.
- A radial BVP is substantially cheaper than training a surrogate for a small number of spherical configurations. Future benchmarks will compare radial BVPs, radial PINNs, axisymmetric PINNs, and full 3D calculations at matched accuracy and account for interpolation and warm starts.

## Citation

If you use these materials, please cite this repository and the relevant notebook or research output:

> E. I. Sfakianakis, *Physics-Informed Neural Networks for Chameleon Screening*, research code and numerical benchmarks (2026).  
> https://github.com/evangelos-sfakianakis/chameleon-pinn

*Repository under active development; files and documentation will be updated as validated experiments are released.*
