# Computational Fluid Dynamics

How I learned CFD during my M.Sc. at Otto von Guericke University Magdeburg: numerical methods written by hand in MATLAB, a set of STAR-CCM+ course exercises, and a wind study of the Brandenburg Gate that brings them together.

**Software:** Siemens Simcenter STAR-CCM+ (simulations), MATLAB (hand-coded numerics)

---

## Main project – wind loads on the Brandenburg Gate

**Folder:** `Final Project/`  ·  individual course project, submitted to Dr.-Ing. habil. Gabor Janiga, 07/2026

How hard does the wind push on the gate, and how fast does the air move where people stand? I ran a steady RANS simulation around a full-scale 3D model of the gate at two inlet speeds, 9 m/s and 18 m/s (about 32 and 65 km/h), and from two wind directions. For each case I extracted drag and lift forces, their coefficients and the peak velocity near the gate.

### Setup

- Geometry: CAD model resized to the real dimensions, 65.9 m (X) × 26.1 m (Y, height) × 11.1 m (Z); surface wrapper for a watertight surface
- Turbulence: SST (Menter) k-ω, All y+ wall treatment
- Solver: steady, segregated, RANS, constant density
- Air: ρ = 1.225 kg/m³, μ = 1.825 × 10⁻⁵ Pa·s
- Domain: 4H to the inlet, sides and top; 8H (side-on) and 10H (front-on) to the outlet, H = 26 m
- Mesh: polyhedral volume mesh with 10 prism layers (0.65 m total) on the gate and ground

### Grid independence

I ran the 18 m/s side-on case on four meshes from 1.17 to 2.83 million cells. The mesh with a base size of 10 % of the height (1.81 million cells) came within 1.5 % of the finest mesh on drag and lift with 36 % fewer cells, so the production runs use it. Data: `Grid Independence study/`.

| Base size (% of height) | Cells | C_d | Drag (N) | C_l | Change in C_d vs finest |
|---|---:|---:|---:|---:|---:|
| 8.75 % | 2,826,682 | 0.676 | 24,703 | 0.134 | – |
| 10 % | 1,813,842 | 0.666 | 24,348 | 0.134 | −1.4 % |
| 11.25 % | 1,459,182 | 0.704 | 25,744 | 0.133 | +4.2 % |
| 12.5 % | 1,169,684 | 0.719 | 26,293 | 0.132 | +6.4 % |

### Results

| Wind direction | Inlet speed | Drag (N) | C_d | Lift (N) | C_l | Peak velocity near gate (m/s) |
|---|---:|---:|---:|---:|---:|---:|
| Side-on (X, along the 65.9 m length) | 9 m/s | 6,217 | 0.680 | 4,861 | 0.133 | 11.6 |
| Side-on (X) | 18 m/s | 24,314 | 0.665 | 19,683 | 0.135 | 23.0 |
| Front-on (Z, onto the 65.9 m façade) | 9 m/s | 55,435 | 1.312 | 17,965 | 0.493 | 16.3 |
| Front-on (Z) | 18 m/s | 221,414 | 1.310 | 70,039 | 0.481 | 32.1 |

- **C_d hardly changes with speed** (≈ 0.67 side-on, ≈ 1.31 front-on), while the force roughly quadruples when the speed doubles. That's expected for a bluff body at high Reynolds number, where pressure drag from flow separation dominates.
- **Direction matters more than speed.** Front-on wind produces about 9× the drag of side-on wind. About 4.6× of that comes from the larger projected area; the rest from C_d roughly doubling (a wide colonnade façade facing the wind instead of a long, narrow block).
- **The air speeds up near the gate.** In the worst case, an 18 m/s front-on wind reaches 32 m/s in the passages between the columns. That local value, not the free-stream speed, is what a pedestrian-comfort check would use.

The CSV files in `X Direction Flow/` and `Y Direction Flow/` (the front-on, Z-direction case) are the force and coefficient histories per iteration. `CFD_Brandenburg_Gate.pptx` walks through the whole study.

---

## Course exercises

### MATLAB – numerics by hand (`Matlab/`)

- `Temperature_distribution.m` – 1D steady heat conduction with the finite-volume method: assembles the coefficient matrix, applies the boundary conditions and solves the linear system.
- `Velocity_distribution_from_stream_function.m` – 2D flow from a stream function solved with Jacobi, Gauss-Seidel and successive over-relaxation (ω = 1.8), then differentiated to get the velocity field; checks that inlet and outlet flow rates balance.

### Laminar channel (`Laminar Channel/`)

First full STAR-CCM+ workflow: import a mesh, set up laminar channel flow, solve and post-process. The velocity profile develops toward the parabolic shape.

### Elbow (`Elbow/`)

Meshing a pipe bend and reading a temperature mixing field and residuals.

### Backward-facing step (`BFS/`)

k-ε and k-ω compared against experimental velocity profiles at x = 3 and x = 8, plus wall shear stress and the recirculation zone. This comparison is why I used SST k-ω in the main project.

### Submarine hull (`Submarine flow simulation/`)

Steady RANS drag and lift on the USS Albacore hull with a four-level mesh study. Details in its own README.

---

## Repository map

```
Matlab/                     1D heat conduction (FVM), 2D stream function (Jacobi / Gauss-Seidel / SOR)
Laminar Channel/            laminar channel flow
Elbow/                      elbow mixing
BFS/                        backward-facing step, k-ε vs k-ω
Submarine flow simulation/  USS Albacore drag and mesh study
Final Project/              Brandenburg Gate wind study
```

`.sim` files are STAR-CCM+ simulation files, `.msh` is a mesh, `.STL` is CAD geometry; images are exported scenes and plots.

**Author:** Shashank Suresh Srinivasan, M.Sc. Chemical and Energy Engineering, Otto von Guericke University Magdeburg
