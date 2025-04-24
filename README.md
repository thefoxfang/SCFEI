# Self-Consistent Field method for Excitonic Insulators (SCFEI)

[![arXiv shield](https://img.shields.io/badge/arXiv-2503.11563-blue.svg?style=flat)](https://arxiv.org/abs/2503.11563)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.15277150.svg)](https://10.5281/zenodo.15277150)

## Contents

- [Overview](#overview)
- [Repo Contents](#repo-contents)
- [System Requirements](#system-requirements)
- [Installation Guide](#installation-guide)
- [Demo](#demo)
- [Results](#results)
- [License](./LICENSE)
- [Issues](https://github.com/ebridge2/lol/issues)
- [Citation](#citation)

# Overview

In 1967, Nobel laureate W. Kohn and colleagues introduced the concept of the [excitonic insulator](https://journals.aps.org/pr/abstract/10.1103/PhysRev.158.462) (EI), a correlated many-body state arising from the condensation of electron-hole pairs, analogous to Cooper pair condensation in a superconductor. To explore the properties of the EI phase theoretically, we have, to the best of our knowledge, firstly developed an ab initio formalism capable of calculating both the electron–hole order parameter and the single-particle properties of the EI phase in the Bardeen–Cooper–Schrieffer (BCS) regime. Our formalism is based on:

(1) the [*GW* method](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.34.5390) to account for the band energy;  
(2) [*GW* plus Bethe-Salpeter equation](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.62.4927) (*GW*-BSE) approach for the electron-hole interactions;  
(3) [Self-consistent field](https://arxiv.org/abs/2503.11563) (SCF) method for solving the so-called gap equations.  

This project focuses on step 3, while the [`BerkeleyGW`](https://berkeleygw.org/) code for steps 1 and 2 is publicly available and free to download.

# Repo Contents
- [Fortran](./Fortran): `Fortran` package main code for performing SCFEI calculations.
- [Tools](./Tools): `Fortran` package code for analysing the order parameter.
- [Docs](./Docs): package documentation, which contains input files descriptions.
- [Demo](./Demo): a demenstration of the code by calculating the EI phase of monolayer 1T'-MoS<sub>2</sub> 

# System Requirements

## Hardware Requirements

### *GW*-BSE calculations
Please consult the [`BerkeleyGW`](https://berkeleygw.org/) documentation for the recommended hardware to run *GW* and *GW*-BSE calculations.  
> &emsp;&#8226; **Warning**: Our package’s input files depend on (1) band energies and (2) the electron–hole interaction kernel from <u>well-converged</u> *GW* and *GW*-BSE runs. If those calculations are not converged, the results will be <u>physically incorrect and non-interpretable</u>.

### SCFEI calculations
SCFEI calculations cannot be performed on a standard workstation. They require a high-performance cluster with:
> &emsp;&#8226; **Sufficient memory** to store the full electron–hole kernel matrix elements  
> &emsp;&#8226; **High CPU throughput** for the self-consistent calculations

For an full electron–hole kernel *K*<sub>v'c'k'vck</sub> (v: valence band index; c: conduction band index; k: k-point index) with 2 valence bands, 2 conduction bands, and ~ 8,000 k-points (size ~ 250 GB), we recommend:  
> &emsp;&#8226; **CPU**: 8 × AMD EPYC 7763 processors (512 total cores)  
> &emsp;&#8226; **RAM**: 2,048 GB

Please ensure you have access to resources of comparable scale before attempting SCFEI calculations.
