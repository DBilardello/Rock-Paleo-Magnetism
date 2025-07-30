# CoercivityUnmixing: Skewed Gaussian Unmixing of Magnetic Coercivity

This repository provides Python code for dynamically fitting **skewed Gaussian components** to **Backfield Demagnetization** and **IRM Acquisition** data. It is designed for applications in **rock and environmental magnetism**, enabling precise modeling of coercivity spectra using both **PDF (Probability Density Function)** and **CDF (Cumulative Distribution Function)** representations. All code is written entirely from scratch and reflects the author’s design choices and vision for coercivity unmixing workflows. While it builds on widely used ideas, it has greatly benefited from valuable discussions and suggestions by Ramon Egli. No part of the implementation is copied from existing scripts or software.

---

## What’s New in v2.0

Version 2.0 is a major rewrite of the original v1.0 code, introducing new optimization logic, streamlined workflows, and improved outputs.

### Iterative PDF–CDF Optimization
- Adds a loop alternating between PDF and CDF fits until residuals stabilize.
- Ensures the same parameter set fits both PDF and CDF simultaneously for consistency.
- Removes the earlier “optimization performance test” for improved speed while maintaining cost reporting (misfit between model and data).

### Cleaner Monte Carlo Workflow
- Monte Carlo uncertainty estimation is now performed once after final convergence, reducing runtime and smoothing results.
- Confidence intervals are calculated consistently for both PDF and CDF fits.

### Refined Plotting & Output
- Multi-panel figures (PDF + CDF + residuals) optimized for diagnostics and publication, with user-selectable file formats.
- Optimized CSV summary of component parameters (mode, center, amplitude, sigma, skew, and % contribution).

### More Efficient Code Organization

- Package version printout added to troubleshoot version-related errors.
- Clear separation between cells requiring user input and those to “run as is.”
- Removed the need to comment/uncomment blocks for data type selection (backfield vs. IRM).
- All user-defined parameters (file name, data type, figure format, number of components, number of Monte Carlo simulations) are clearly marked before the relevant code blocks, allowing users to modify them without rerunning the entire notebook.
- Optional memory management cell (`plt.close('all')`) to prevent Jupyter kernel overload.

### Markdown Explanations Throughout
- Embedded math formulas, usage notes, and code explanations make the notebook fully self-contained and instructional.
- PDF and CDF models now fully documented with equations for transparency.

---

## Features (All Versions)
- Interactive sliders for intuitive parameter initialization.
- Skewed Gaussian PDF model for gradient (backfield/IRM) data.
- Skewed Gaussian CDF model for scaling and interpretation.
- Intersection detection between adjacent components for key magnetic metrics (e.g., S-ratios, HIRM).
- Residual analysis for both PDF and CDF domains.
- Flexible plotting suite for diagnostics, interpretation, and publication.
- CSV output with log and linear values that are rounded consistently.

---

## Overview of Models

### PDF Model (for fitting and optimization)
```python
def skewed_gaussian(x, amplitude, center, sigma, skew):
    norm = amplitude / (np.sqrt(2 * np.pi) * sigma) * np.exp(-(x - center) ** 2 / (2 * sigma ** 2))
    erf_term = (1 + erf(-skew * (x - center) / (np.sqrt(2) * sigma)))
    return norm * erf_term

### CDF Model (for scaling and interpretation)
```python
def skewed_gaussian_cdf(x, amplitude, center, sigma, skew):
    skew = -skew
    erf_term_cdf = (1 + erf((x - center) / (np.sqrt(2) * sigma)))
    owen_term = 2 * owens_t((x - center) / sigma, skew)
    return amplitude * (0.5 * erf_term_cdf - owen_term)
```

---

## Plotting Options
The notebook provides a flexible suite of plots to visualize fit quality and model behavior:

- Plots 1–2: Full diagnostics, including both CDF and PDF with fits, error bands, residuals, component intersections, and peak locations.
- Plots 3–4: Focused outputs for publication — separate, cleanly labeled plots for PDF (gradient data) and CDF fits.

These options balance model evaluation, parameter reporting, and visual clarity depending on the user’s needs.

---

## Citation & Licensing
If you use this code, or adapt any part of it for your research or publications, please cite it as:

> **Bilardello, D. (2025).** CoercivityUnmixing: Skewed Gaussian Unmixing of Magnetic Coercivity, *GitHub*, DOI: 10.13140/RG.2.2.10602.43208, https://github.com/DBilardello/Rock-Paleo-Magnetism/tree/main/CoercivityUnmixing

This repository does not accept external code contributions. Please do not submit pull requests.

**Please Note:**
- Incorporation of any **new ideas, algorithms, or implementations** from this code into third-party software packages is **strictly prohibited without permission**.
- **Attribution is required** for any use or adaptation of this code, whether the components used are **standard or novel**.
- **Reuse, reproduction, or redistribution** of any part of this code or its conceptual structure for inclusion in shared software tools, repositories, or frameworks **requires prior approval from the author**.