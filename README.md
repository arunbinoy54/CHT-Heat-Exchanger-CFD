# CHT-Heat-Exchanger-CFD
A 3D Conjugate Heat Transfer (CHT) simulation of a double-pipe heat exchanger using Ansys Fluent, featuring parallel vs. counter-flow analysis and LMTD analytical validation.

This repository contains a comprehensive 3D Conjugate Heat Transfer (CHT) analysis of a concentric pipe-in-pipe heat exchanger. Built and simulated in Ansys Fluent utilizing a 1.9M cell conformal poly-hexcore mesh, the project evaluates the thermodynamic efficiency of parallel-flow versus counter-flow configurations.

Key engineering highlights include:

* **Turbulence Modeling:** Implementation of the SST k-omega model with tightly resolved boundary layer inflation to accurately capture near-wall thermal gradients and fluid friction.
* **Thermodynamic Validation:** Direct verification of CFD solver outputs against fundamental analytical heat transfer equations, including Log Mean Temperature Difference (LMTD) and overall heat transfer coefficients ($U$).
* **Performance Analysis:** A critical engineering assessment isolating the impact of flow orientation, fluid residence time, and pipe length constraints on total thermal energy extraction.

## I. Introduction

This project simulates hot water flowing through an inner pipe and cold water flowing through an outer jacket. The geometric configuration consists of two concentric tubes, serving as a classic mechanical engineering benchmark. The primary objective is to evaluate the heat transfer efficiency of the system and validate the numerical solver outputs against exact analytical formulas, such as the Log Mean Temperature Difference (LMTD) method.

Validating numerical results with textbook hand-calculations provides rigorous proof that the 3D computational setup accurately captures the underlying thermodynamic physics.

## II. Mesh Generation & Topology

The domain was discretized using an unstructured poly-hexcore mesh, optimizing cell count while maintaining high orthogonality. A key element of this CHT simulation is the implementation of a conformal mesh.

* **Thermal Continuity:** By enabling "Share Topology" in the CAD stage, Ansys Fluent recognizes the shared 2D boundary between the fluid and solid domains. It automatically generates overlapping wall and shadow surfaces that enforce perfect thermal energy conservation across the interface.
* **Hydrodynamic Boundary:** The fluid-facing side of this plane enforces the no-slip condition, bringing fluid velocity to zero to properly develop the boundary layer.
* **Fluid Domains (Inflation):** To resolve steep velocity and thermal gradients, 7 inflation layers were applied exclusively to the fluid regions.
* **Solid Domain (Body Sizing):** A local body sizing target of 1 mm was applied to the 2 mm thick copper pipe to ensure 3 to 4 cells spanned its thickness, maximizing thermal conduction accuracy.
* **Final Metrics:** The final grid contains 1,903,975 cells with a minimum Orthogonal Quality of 0.34.
<img width="1035" height="522" alt="Screenshot 2026-08-31 202011" src="https://github.com/user-attachments/assets/626647e6-f2ce-4521-b93a-1efcd318019b" />
<img width="602" height="386" alt="Screenshot 2026-08-31 201028" src="https://github.com/user-attachments/assets/aa97b98c-1278-4c72-8d29-6f3475eed395" />

<img width="602" height="386" alt="Screenshot 2026-08-31 201028" src="https://github.com/user-attachments/assets/b35d088f-4c85-4d76-b326-fe77246ce45f" />
<img width="1035" height="522" alt="Screenshot 2026-08-31 202011" src="https://github.com/user-attachments/assets/6602edea-7cfa-4dd3-a778-0cb57a0c97cd" />


