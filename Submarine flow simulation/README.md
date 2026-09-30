# Drag on a submarine hull (USS Albacore) – STAR-CCM+

Course exercise, Lab of Fluid Mechanics and Technical Flows (LSS), Otto von Guericke University Magdeburg. Based on the hands-on guide by L. Daróczy and G. Janiga, *CFD Hands-on: Drag forces of a submarine using StarCCM+* (01/2025).

A steady RANS simulation of water flowing past the USS Albacore hull at 1.5 m/s, giving drag and lift coefficients, followed by a mesh study over four base sizes.

$$C_D = \frac{F_x}{\tfrac{1}{2}\,\rho\,v^2 A}$$

A stationary hull in a 1.5 m/s stream is equivalent to the hull moving through still water.

## Geometry and domain

| Parameter | Value |
|---|---|
| Geometry | USS Albacore, STL from grabcad.com |
| Domain | Rectangular block from [−40, 0, −25] m to [100, 25, 25] m |
| Symmetry | Half model, symmetry plane at y = 0 |
| Frontal area | 30.25 m² |

A surface wrapper produces a watertight surface before volume meshing.

## Flow conditions

| Parameter | Value |
|---|---|
| Fluid | Water, ρ = 1000 kg/m³ |
| Inlet / outlet | Velocity inlet 1.5 m/s / pressure outlet |
| Turbulence | SST (Menter) k-ω, All y+ wall treatment (target 30 < y+ < 150) |
| Solver | Steady, segregated, RANS |
| Stopping criterion | Continuity residual ≤ 4 × 10⁻⁵ |

## Mesh

Polyhedral cells with 9 prism layers on the hull (total thickness 10 % of base size), hull surface size 10 % of base, volume growth rate 1.2.

## Mesh study

| Base size (m) | Cells | Iterations | C_D | C_L |
|:---:|---:|:---:|:---:|:---:|
| 2.00 | 95,236 | 137 | 0.0857 | −0.0094 |
| 1.75 | 119,341 | 137 | 0.0812 | −0.0042 |
| 1.50 | 164,486 | 148 | 0.0766 | −0.0059 |
| 1.25 | 233,972 | 157 | 0.0708 | −0.0076 |

**C_D** falls steadily as the mesh gets finer, 17 % over the whole range and still 7.5 % between the two finest meshes. The result is **not mesh-independent yet**: coarse meshes under-resolve the boundary layer and overpredict drag. The next step would be at least one finer mesh (base size ≤ 1.0 m) and a Richardson extrapolation.

**C_L** stays near zero (order 10⁻³), as expected for a symmetric hull at zero angle of attack. The small scatter comes from mesh asymmetries at bow and stern.

## Files

`Simulation results/With base size …/` – mesh, residual, drag and lift plots and velocity, pressure and vector fields for each mesh. `CAD STL input data.STL` – hull geometry.
