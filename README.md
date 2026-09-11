# CHT-Heat-Exchanger-CFD
A 3D Conjugate Heat Transfer (CHT) simulation of a double-pipe heat exchanger using Ansys Fluent, featuring parallel vs. counter-flow analysis and LMTD analytical validation.

This repository contains a comprehensive 3D Conjugate Heat Transfer (CHT) analysis of a concentric pipe-in-pipe heat exchanger. Built and simulated in Ansys Fluent utilizing a 1.9M cell conformal poly-hexcore mesh, the project evaluates the thermodynamic efficiency of parallel-flow versus counter-flow configurations.

Key engineering highlights include:

* **Turbulence Modeling:** Implementation of the SST k-omega model with tightly resolved boundary layer inflation to accurately capture near-wall thermal gradients and fluid friction.
* **Thermodynamic Validation:** Direct verification of CFD solver outputs against fundamental analytical heat transfer equations, including Log Mean Temperature Difference (LMTD) and overall heat transfer coefficients ($U$).
* **Performance Analysis:** A critical engineering assessment isolating the impact of flow orientation, fluid residence time, and pipe length constraints on total thermal energy extraction.

Introduction

This project simulates hot water flowing through an inner pipe and cold water flowing through an outer jacket. The geometric configuration consists of two concentric tubes, serving as a classic mechanical engineering benchmark. The primary objective is to evaluate the heat transfer efficiency of the system and validate the numerical solver outputs against exact analytical formulas, such as the Log Mean Temperature Difference (LMTD) method.
Validating numerical results with textbook hand-calculations provides rigorous proof that the 3D
computational setup accurately captures the underlying thermodynamic physics.
