# Magnetic Nanoparticles Granulometry

This repository contains the magnetic data processing and analysis workflows developed for:

**Bondar, D., Canizares, A., Bilardello, D., Valdivia, P., Zandonà, A., Romano, C., Allix, M., Di Genova, D. (2025).**  
*High-temperature Raman spectroscopy and low-temperature rock-magnetic investigation of volcanic glasses.*  
**Geochemistry, Geophysics, Geosystems**, 26, e2024GC011846.  
[https://doi.org/10.1029/2024GC011846](https://doi.org/10.1029/2024GC011846)

## Project Overview

This study examined Fe-Ti-oxide nanolite formation in undercooled anhydrous andesite using high-temperature Raman spectroscopy. My sole responsibility on the interdisciplinary team was the magnetic characterization, which provided independent confirmation of nanolite crystallization and insights into their size and magnetic domain state.

The magnetic analysis included:
- Acquisition of low-temperature frequency-dependent magnetic susceptibility data
- Magnetic hysteresis behavior at room and cryogenic temperatures
- Grain size modeling based on superparamagnetic/ferromagnetic transition thresholds

Using these tools, I demonstrated that the nanocrystals formed during high-T Raman experiments were consistent with superparamagnetic magnetic particles with characteristic diameters near 20 nm. The magnetic results reinforced the Raman findings and provided a complementary constraint on crystallite evolution during thermal treatment.
**Note: LT susceptibility data was acquired by me on a Quantum Designs MPMS-XL system- the code was written for this instrument's data format**

## Methods & Tools

- `numpy`, `pandas`, `scipy.optimize`, `scipy.interpolate`, `matplotlib`
- Custom scripts for coercivity unmixing and fitting of superparamagnetic grain size distributions

---

If citing this work, please refer to the published article linked above.
