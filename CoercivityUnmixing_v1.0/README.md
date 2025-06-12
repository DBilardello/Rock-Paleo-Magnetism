# CoercivityUnmixing: Skewed Gaussian Unmixing of Magnetic Coercivity

This repository provides Python code for dynamically fitting **skewed Gaussian components** to **Backfield Demagnetization** and **IRM Acquisition** data. It is designed for applications in **rock and environmental magnetism**, enabling precise modeling of coercivity spectra using both **PDF (Probability Density Function)** and **CDF (Cumulative Distribution Function)** representations. All code is written entirely from scratch and reflects the author’s design choices and vision for coercivity unmixing workflows. While it builds on widely used ideas, it has greatly benefited from valuable discussions and suggestions by Ramon Egli. No part of the implementation is copied from existing scripts or software.

---

## Overview

The code implements two core model types:

### PDF Model (for fitting and optimization)
```python
def skewed_gaussian(x, amplitude, center, sigma, skew):
    norm = amplitude / (np.sqrt(2 * np.pi) * sigma) * np.exp(-(x - center) ** 2 / (2 * sigma ** 2))
    erf_term = (1 + erf(-skew * (x - center) / (np.sqrt(2) * sigma)))
    return norm * erf_term
```

### CDF Model (for scaling and interpretation)
```python
def skewed_gaussian_cdf(x, amplitude, center, sigma, skew):
    skew = -skew
    erf_term_cdf = (1 + erf((x - center) / (np.sqrt(2) * sigma)))
    owen_term = 2 * owens_t((x - center) / sigma, skew)
    return amplitude * (0.5 * erf_term_cdf - owen_term)
```

---

## Key Features

- Self-contained **Jupyter Notebook** with embedded Markdown instructions, model explanations, and clearly annotated code blocks for guided use.
- **Interactive parameter control** via sliders for component parameter input.
- **Optimization** and **Monte Carlo uncertainty estimation** performed on the PDF for accurate component separation.
- **Optimization performance test** included to evaluate fit convergence, stability, and residual behavior across runs.
- **Amplitude rescaling** to apply optimized PDF parameters to the original CDF data.
- **Intersection detection** between adjacent PDF components, with field values reported for sample-specific component-based metrics (e.g., **S-ratios**, **HIRM**, **goethite test**).
- **Component summary** including mode, center, amplitude, and percentage contribution.
- **Residual analysis** for both PDF and CDF domains.
- **Dynamic plot generation** offering multiple outputs for diagnostics and publication.

---

## Plotting Options

The notebook provides a **flexible suite of plots** to visualize fit quality and model behavior:

- **Plot 1:** Full diagnostics, including both CDF and PDF with fits, error bounds, and residuals.
- **Plots 2–4:** Focused outputs ideal for interpretation and publication– PDF (gradient data)-fit with confidence bounds and CDF (data) residuals, CDF-fit, and combined summary.

These options help balance model evaluation, parameter reporting, and visual clarity depending on the use case.

---

## Citation & Licensing

If you use this code, or adapt any part of it for your research or publications, please cite it as:

> **Bilardello, D. (2025).** Rock-Paleo-Magnetism: Tools for processing magnetic data in paleomagnetism and rock magnetism, *GitHub*, https://github.com/DBilardello/Rock-Paleo-Magnetism

This repository does not accept external code contributions. Please do not submit pull requests.

**Please Note:**

- Incorporation of any **new ideas, algorithms, or implementations** from this code into third-party software packages is **strictly prohibited without permission**.
- **Attribution is required** for any use or adaptation of this code, whether the components used are **standard or novel**.
- **Reuse, reproduction, or redistribution** of any part of this code or its conceptual structure for inclusion in shared software tools, repositories, or frameworks **requires prior approval from the author**.