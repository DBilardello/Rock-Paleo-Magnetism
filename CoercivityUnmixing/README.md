# CoercivityUnmixing: Skewed Gaussian Unmixing of Magnetic Coercivity

This repository provides Python code for dynamically fitting **skewed Gaussian components** to **Backfield Demagnetization** and **IRM Acquisition** data. It is designed for applications in **rock and environmental magnetism**, enabling precise modeling of coercivity spectra using both **PDF (Probability Density Function)** and **CDF (Cumulative Distribution Function)** representations. All code is written entirely from scratch and reflects the author’s design choices and vision for coercivity unmixing workflows. While it builds on widely used ideas, it has greatly benefited from valuable discussions and suggestions by Ramon Egli. No part of the implementation is copied from existing scripts or software.

---

## Current Version
**v2.1** — Monte Carlo–consistent coercivity unmixing with PDF–CDF joint optimization.

---

## Version History

### v2.1 — Monte Carlo–Consistent Parameter & Residual Definitions

Version 2.1 refines the interpretation and reporting of fit results by ensuring
**full consistency between optimized parameters, Monte Carlo uncertainty analysis,
residual calculations, and reported magnetic metrics**.

Key methodological improvements include:

- **Monte Carlo–based residuals**
  - PDF and CDF residuals are now calculated relative to the *Monte Carlo mean fits*,
    not the single deterministic optimized solution.
  - This ensures that residuals correspond directly to the plotted confidence envelopes
    and represent deviations from the statistically most probable model.

- **Monte Carlo–consistent derived parameters**
  - Final reported component properties (modes, intersections, percent contributions)
    are computed *after* Monte Carlo simulations, using the Monte Carlo mean component curves.
  - This avoids mixing deterministic best-fit quantities with Monte Carlo uncertainty estimates.

- **Separation of optimization vs. uncertainty analysis**
  - Deterministic optimization is used solely to find a stable starting solution.
  - All interpretive quantities used for reporting and visualization are derived from
    Monte Carlo results, aligning the workflow with standard uncertainty propagation practices.

- **Improved internal consistency**
  - Plotting, residual analysis, legend labels, and CSV summaries now refer to the same
    Monte Carlo–derived quantities.
  - This eliminates subtle ambiguities present in earlier versions where optimized and
    Monte Carlo quantities could diverge.

These changes do not alter the underlying skewed Gaussian model, but they significantly
improve the statistical clarity and interpretability of the results.

### v2.0 — Iterative PDF–CDF Optimization Framework

Version 2.0 is a major rewrite of the original v1.0 code, introducing new optimization logic, streamlined workflows, and improved outputs.

- **Iterative PDF–CDF Optimization**
  - Adds a loop alternating between PDF and CDF fits until residuals stabilize.
  - Ensures the same parameter set fits both PDF and CDF simultaneously for consistency.
  - Removes the earlier “optimization performance test” for improved speed while maintaining cost reporting (misfit between model and data).

- **Cleaner Monte Carlo Workflow**
  - Monte Carlo uncertainty estimation is now performed once after final convergence, reducing runtime and smoothing results.
  - Confidence intervals are calculated consistently for both PDF and CDF fits.

- **Refined Plotting & Output**
  - Multi-panel figures (PDF + CDF + residuals) optimized for diagnostics and publication, with user-selectable file formats.
  - Optimized CSV summary of component parameters (mode, center, amplitude, sigma, skew, and % contribution).

- **More Efficient Code Organization**
  - Package version printout added to troubleshoot version-related errors.
  - Clear separation between cells requiring user input and those to “run as is.”
  - Removed the need to comment/uncomment blocks for data type selection (backfield vs. IRM).
  - All user-defined parameters (file name, data type, figure format, number of components, number of Monte Carlo simulations) are clearly marked before the relevant code blocks, allowing users to modify them without rerunning the entire notebook.
  - Optional memory management cell (`plt.close('all')`) to prevent Jupyter kernel overload.

#### Markdown Explanations Throughout
- Embedded math formulas, usage notes, and code explanations make the notebook fully self-contained and instructional.
- PDF and CDF models now fully documented with equations for transparency.

### v1.0 — Initial Skewed Gaussian Unmixing Prototype
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