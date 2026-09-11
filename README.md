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
<img width="602" height="386" alt="Screenshot 2026-08-31 201028" src="https://github.com/user-attachments/assets/b35d088f-4c85-4d76-b326-fe77246ce45f" />

## III. Simulation Setup & Boundary Conditions

The simulation was executed as a Steady-State analysis using a pressure-based coupled solver. A Pseudo Time Method and Warped-Face Gradient Correction were activated to enhance stability and convergence speed. Spatial discretization for momentum, energy, and turbulence was set to Second Order Upwind to minimize numerical diffusion.

The SST k-omega turbulence model was selected because it seamlessly blends standard k-omega near the pipe walls (capitalizing on the dense inflation layers to capture steep thermal gradients) with standard k-epsilon in the free stream.

### III.1. Inlets & Outlets

* **Hot Fluid Inlet:** Velocity = 0.5 m/s, Static Temperature = 350 K.
* **Cold Fluid Inlet:** Velocity = 0.5 m/s, Static Temperature = 300 K.
* **Outlets:** Gauge Pressure = 0 Pa. Backflow total temperatures were explicitly set to 350 K (hot side) and 300 K (cold side) to prevent artificial thermal shocks and divergence during early calculation instabilities.

### IV. PARALLEL FLOW

The initial phase of the simulation evaluates the heat exchanger in a parallel-flow configuration. In this arrangement, both the hot inner fluid and the cold jacket fluid enter the domain from the same physical end of the pipe and travel in the same direction. 

This setup serves as the baseline performance metric for the project. Thermodynamically, parallel flow generates the highest initial temperature gradient at the inlet boundary. As the fluids travel along the 1-meter copper pipe, this temperature difference exponentially decays as the two streams exchange heat and approach a shared equilibrium temperature.

## IV.1. Results & Convergence Validation

Default automatic convergence monitors were disabled to force the solver to achieve true steady-state thermal equilibrium across 500 complete iterations.

* **Residuals:** Scaled residuals demonstrated excellent asymptotic stabilization. Continuity dropped to near $10^{-9}$ and the energy residual achieved absolute convergence near $10^{-14}$.
* **Conservation:** The domain is perfectly sealed, yielding a net mass imbalance of $1.03 \times 10^{-13}$ kg/s. Absolute energy conservation was verified by a net thermal energy imbalance of $-2.24 \times 10^{-7}$ W across the entire system.

<img width="1091" height="583" alt="Screenshot 2026-09-11 012748" src="https://github.com/user-attachments/assets/67f1549a-cc93-488b-b13f-d3144f9c8706" />

### IV.2. Performance Metrics

| Parameter | Value | Unit |
| :--- | :--- | :--- |
| Hot Fluid Inlet Energy Rate | 18,970.61 | W |
| Hot Fluid Outlet Energy Rate | 15,834.64 | W |
| Net Heat Transfer Rate (Q) | 3,135.97 | W |
| Hot Fluid Outlet Temperature | 340.65 | K |
| Cold Fluid Outlet Temperature | 302.40 | K |
| Average Nusselt Number | 18.92 | - |
| Average Local Heat Transfer Coefficient (h) | 11.35 | W/(m²·K) |
| Pressure Drop (Hot / Cold Lines) | 291.05 / 288.09 | Pa |

Static Pressure

<img width="1430" height="568" alt="Screenshot 2026-09-11 014518" src="https://github.com/user-attachments/assets/63d9211b-d347-40cf-bb9c-289f5a8eea1c" />



Velocity Contour

<img width="1443" height="580" alt="Screenshot 2026-09-11 014619" src="https://github.com/user-attachments/assets/a7996b5c-c092-46c1-9a29-e49c445e53c1" />




Temperature Contour

<img width="1436" height="557" alt="Screenshot 2026-09-11 014634" src="https://github.com/user-attachments/assets/8b109481-7f51-4e9c-871b-235dbbe1192e" />




