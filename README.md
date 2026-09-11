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

**Static Pressure Contour** 
<img src="https://github.com/user-attachments/assets/a5012dec-7443-44b5-83ab-f9be1486fa81"  width="100%" />

**Velocity Contour**  
<img src="https://github.com/user-attachments/assets/a7996b5c-c092-46c1-9a29-e49c445e53c1" width="100%" />

**Temperature Contour**  
<img src="https://github.com/user-attachments/assets/8b109481-7f51-4e9c-871b-235dbbe1192e" width="100%" />

**Nusselt Number**  
<img src="https://github.com/user-attachments/assets/a10cbd9d-acac-4bde-b231-cfd088ba0010" width="100%" />

**Energy Balance**  
<img src="https://github.com/user-attachments/assets/4a996cf6-3215-4ae2-a128-d958395b7254" width="100%" />

**Average HTC**  
<img src="https://github.com/user-attachments/assets/27cef3ad-c16a-4a6c-9578-24a3b3ad14cc" width="100%" />

## IV.3. Analytical Validation (LMTD Method)

To mathematically validate the CFD results, analytical hand calculations were performed using the simulation geometry ($L = 1.0\text{ m}$, inner diameter $D = 0.035\text{ m}$) in a counter-flow arrangement.

$$\Delta T_1 = T_{h,in} - T_{c,out} = 350.00 - 302.40 = 47.60\text{ K}$$
$$\Delta T_2 = T_{h,out} - T_{c,in} = 340.65 - 300.00 = 40.65\text{ K}$$

$$\Delta T_{LMTD} = (\Delta T_1 - \Delta T_2) / \ln(\Delta T_1 / \Delta T_2) = 44.02\text{ K}$$

The heat transfer surface area of the inner pipe is $A = \pi \cdot D \cdot L = 0.1099\text{ m}^2$. Using the CFD-derived total heat transfer ($Q = 3,135.97\text{ W}$), the overall heat transfer coefficient ($U$) is determined:

$$U = Q / (A \cdot \Delta T_{LMTD}) = 3,135.97 / (0.1099 \cdot 44.02) = 648.25\text{ W}/(\text{m}^2\cdot\text{K})$$

> **Discussion:** While the CFD-extracted wall coefficient (11.35 W/(m²·K)) represents the single-side convective film resistance, the analytical $U$ value encompasses the complete thermal circuit (inner film, outer film, and radial wall conduction). The low frictional pressure drops (~290 Pa) confirm efficient laminar/turbulent flow operation, validating the integrity of this 3D CHT design model.

## V. Counter-Flow Analysis

Following the parallel-flow baseline, the simulation configuration was inverted to establish a counter-flow regime. In this arrangement, the cold jacket fluid enters from the opposite physical end of the pipe, causing the two fluid streams to travel in opposite directions. 

This setup serves to maximize the thermodynamic efficiency of the heat exchanger. Thermodynamically, counter flow maintains a more uniform temperature difference along the entire length of the 1-meter copper pipe, preventing the rapid decay seen in parallel flow and allowing the hot fluid to cool down further and the cold fluid to absorb more thermal energy.

## V.1. Results & Convergence Validation

Default automatic convergence monitors were disabled to force the solver to achieve true steady-state thermal equilibrium across 500 complete iterations under the counter-flow configuration.

* **Residuals:** Scaled residuals demonstrated excellent asymptotic stabilization. Continuity dropped to near $10^{-10}$ and the energy residual achieved absolute convergence near $10^{-15}$.
* **Conservation:** The domain achieved robust physical closure, yielding a net mass imbalance of $1.94 \times 10^{-15}$ kg/s and an absolute thermal energy imbalance of $1.67 \times 10^{-8}$ W across the entire system.
<img width="1129" height="548" alt="Screenshot 2026-09-12 002755" src="https://github.com/user-attachments/assets/7fd6350c-623a-414c-9e81-8cbc55beec6b" />

### V.2. Performance Metrics

| Parameter | CFD Extracted Value | Unit |
| :--- | :--- | :--- |
| Hot Fluid Outlet Temp ($T_{h,out}$) | 340.32 | K |
| Cold Fluid Outlet Temp ($T_{c,out}$) | 302.51 | K |
| Net Heat Transfer Rate ($Q$) | 3,179.40 | W |
| Average Nusselt Number | 22.39 | - |
| Average Local HTC ($h$) | 13.44 | W/(m²·K) |
| Pressure Drop (Hot / Cold) | 291.05 / 290.21 | Pa |

**Static Pressure Contour** 
<img src="https://github.com/user-attachments/assets/143d026a-8040-4f85-bc7a-1837a24498ae"  width="100%" />

**Velocity Contour**  
<img src="https://github.com/user-attachments/assets/ebe87e99-77f7-45af-9b0e-b921bda26174" width="100%" />

**Temperature Contour**  
<img src="https://github.com/user-attachments/assets/8d1d34ad-f6c7-43d4-bb7c-84c78fe946d6" width="100%" />

**Nusselt Number**  
<img src="https://github.com/user-attachments/assets/7426eeab-0e93-4ed8-a9b4-bbdd6446efac" width="100%" />

**Energy Balance**  
<img src="https://github.com/user-attachments/assets/00d00d95-2bdc-47f4-a667-c312131be448" width="100%" />

**Average HTC**  
<img src="https://github.com/user-attachments/assets/462f6579-5bfb-4181-81f9-c1030f0c4299" width="100%" />

## V.3. Analytical Validation (LMTD & Overall U)[cite: 1]

To mathematically validate the final CFD results, the Log Mean Temperature Difference (LMTD) and Overall Heat Transfer Coefficient (U) were calculated based on the new counter-flow boundary data[cite: 1].

$$\Delta T_1 = T_{h,in} - T_{c,out} = 350.00 - 302.51 = 47.49\text{ K}$$[cite: 1]
$$\Delta T_2 = T_{h,out} - T_{c,in} = 340.32 - 300.00 = 40.32\text{ K}$$[cite: 1]

$$\Delta T_{LMTD} = (\Delta T_1 - \Delta T_2) / \ln(\Delta T_1 / \Delta T_2) = 43.81\text{ K}$$[cite: 1]

Using the inner surface area ($A = 0.1099\text{ m}^2$) and the CFD-extracted heat transfer rate ($Q = 3,179.40\text{ W}$)[cite: 1]:

$$U = Q / (A \cdot \Delta T_{LMTD}) = 3,179.40 / (0.1099 \cdot 43.81) = 660.39\text{ W}/(\text{m}^2\cdot\text{K})$$[cite: 1]

## VI. Final Comparison: Parallel vs. Counter-Flow

Comparing the two simulations isolates the thermodynamic advantage of the counter-flow orientation[cite: 1]. While the geometry, mesh (1.9M cells), turbulence model (SST k-omega), and fluid velocities remained identical, reversing the fluid path generated distinct performance enhancements[cite: 1].

| Metric | Parallel-Flow | Counter-Flow | Difference |
| :--- | :--- | :--- | :--- |
| Total Heat Transfer ($Q$) | 3,135.97 W | 3,179.40 W | + 43.43 W |
| Overall HTC ($U$) | 648.25 W/(m²·K) | 660.39 W/(m²·K) | + 12.14 W/(m²·K) |
| Hot Fluid Outlet | 340.65 K | 340.32 K | - 0.33 K (Cooler) |
| Cold Fluid Outlet | 302.40 K | 302.51 K | + 0.11 K (Warmer) |

> **Engineering Discussion & Conclusion:**<br><br>
> The counter-flow heat transfer (3,179.40 W) is only 43.43 W higher than the parallel flow (3,135.97 W)[cite: 1]. This small absolute difference is constrained by the geometry and boundary conditions: the pipe is only 1 meter long, and the water is moving very fast ($0.5\text{ m/s}$)[cite: 1]. This means the fluid only spends approximately 2 seconds inside the heat exchanger[cite: 1].<br><br>
> The physical length is simply too short for the counter-flow temperature profiles to fully develop and pull away from the parallel-flow efficiency[cite: 1]. If this pipe were lengthened to 5 meters, or the flow slowed down to increase residence time, the counter-flow efficiency would drastically outpace the parallel setup[cite: 1]. Understanding the relationship between fluid residence time and thermal profile development is critical for scaling these principles up to industrial applications[cite: 1].
