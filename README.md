# Self-Consistent Field method for Excitonic Insulators (SCFEI)

[![arXiv shield](https://img.shields.io/badge/arXiv-2503.11563-blue.svg?style=flat)](https://arxiv.org/abs/2503.11563)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.15277150.svg)](https://10.5281/zenodo.15277150)

## Contents

- [Overview](#overview)
- [Repo Contents](#repo-contents)
- [System Requirements](#system-requirements)
- [Installation Guide](#installation-guide)
- [Documentation](#documentation)
- [File Description](#file-description)
- [Demo](#demo)
- [Results](#results)
- [License](./LICENSE)
- [Issues](https://github.com/ebridge2/lol/issues)
- [Citation](#citation)

# Overview

In 1967, Nobel laureate W. Kohn and colleagues introduced the concept of the [excitonic insulator](https://journals.aps.org/pr/abstract/10.1103/PhysRev.158.462) (EI), a correlated many-body state arising from the condensation of electron-hole pairs, analogous to Cooper pair condensation in a superconductor. To explore the properties of the EI phase theoretically, we have, to the best of our knowledge, firstly developed an ab initio formalism capable of calculating both the electron–hole order parameter and the single-particle properties of the EI phase in the Bardeen–Cooper–Schrieffer (BCS) regime. Our formalism is based on:

> (1) [*GW* method](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.34.5390) to account for the band energy;  
> (2) [*GW* plus Bethe-Salpeter equation](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.62.4927) (*GW*-BSE) approach for the electron-hole interactions;  
> (3)  [Self-consistent field](https://arxiv.org/abs/2503.11563) (SCF) method for solving the so-called gap equations.  

This project focuses on step (3), while the [`BerkeleyGW`](https://berkeleygw.org/) code for steps (1) and (2) is publicly available and free to download.

# Repo Contents
- [Fortran](./Fortran): `Fortran` package main code for performing SCFEI calculations.
- [Tools](./Tools): `Fortran` package code for analysing the order parameter.
- [Docs](./Docs): package documentation, which contains input files descriptions.
- [Demo](./Demo): a demenstration of the code by calculating the EI phase of monolayer 1T'-MoS<sub>2</sub> 

# System Requirements

## Hardware Requirements

### *GW*-BSE calculations
Please consult the [`BerkeleyGW`](https://berkeleygw.org/) documentation for the recommended hardware to run *GW* and *GW*-BSE calculations.  
> **Warning**: Our package’s input files depend on (1) band energies and (2) the full electron–hole interaction kernel from <u>well-converged</u> *GW* and *GW*-BSE runs. If those calculations are not converged, the results will be <u>physically incorrect and non-interpretable</u>.

### SCFEI calculations
SCFEI calculations cannot be performed on a standard workstation. They require a high-performance cluster with:
> **High CPU throughput** for the self-consistent calculations  
> **Sufficient memory** to store the full electron–hole kernel matrix elements

For an full electron–hole kernel *K*<sub>v'c'k'vck</sub> (v: valence band index; c: conduction band index; k: k-point index) with 2 valence bands, 2 conduction bands, and ~ 8,000 k-points (size ~ 250 GB), we recommend:  
> **CPU**: 8 × AMD EPYC 7763 processors (512 total cores)  
> **RAM**: 2,048 GB

Please ensure you have access to resources of comparable scale before attempting SCFEI calculations.

## Software Requirements
The package requires a Linux operating system with an MPI-capable Fortran compiler (Fortran 2003 compatible). You will also need the following libraries:
> **[LAPACK95](https://www.netlib.org/lapack95/)** for matrix diagonalization  
> **[HDF5](https://www.hdfgroup.org/solutions/hdf5/)** for file I/O

Ensure that both `LAPACK95` and `HDF5` are installed with Fortran interfaces before proceeding.

#  Installation Guide
Here is the procedure to install the main code under `/Fortran` directory. The installation procedure for the utilities under the `/Tools` directory is the same.
1. Change to the `/Fortran` directory:  
```cd Fortran```
2. Edit arch.mk to set:  
`FC`: your Fortran compiler command (e.g., `mpif90`)  
`MY_HDF5_DIR`: path to the HDF5 installation  
`MY_LAPACK_DIR`: path to the LAPACK95 installation
3. In the /Fortran directory, compile the code:  
`make`
4. After compilation, the executable `delta.x` will be placed in `/Fortran/bin/`. Run it with:  
`srun delta.x -i delta.inp > delta.out`

# Documentation
The format of all input files—including the `delta.inp` file used to run `delta.x`—can be found in the `/Doc` directory.

# File Description

## Input files
To run `delta.x`, you need the following outputs from prior GW–BSE calculations using the BerkeleyGW package:

1. **`eqp.dat`**: contains the quasiparticle energies (band energies) produced by BerkeleyGW’s `sigma.x` code.  
2. **`kernel.h5`**: contains the electron–hole interaction kernel produced by BerkeleyGW’s `kernel.x` code.  
3. **`eigenvectors.h5`**: contains the exciton envelope functions produced by BerkeleyGW’s `absorption.x` code.

Please note Filenames **must** be exactly `eqp.dat`, `kernel.h5`, and `eigenvectors.h5`, and the `eqp.dat` file **must** include all k-points present in `kernel.h5` (i.e., do not fold k-points using symmetry).  

## Output files
After executing `delta.x` (for example, with `srun delta.x -i delta.inp > delta.out`), the following output files are produced:
1. **convergence.dat**: records the SCF convergence behavior at each iteration.
2. **kpt.dat**: lists all k-points used in the calculation. 
3. **qmat.dat**: contains the Bogoliubov transformation matrix from the final SCF step. 
4. **gapmat.dat**: contains the order parameter (gap matrix) from the final SCF step.  
5. **eig.dat**: lists the single-particle excitation energies of the EI phase from the last SCF iteration.  
6. **delmat.h5**: consolidates all of the above data into a single `HDF5` file. 

# Demo
