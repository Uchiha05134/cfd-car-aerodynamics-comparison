# CFD Analysis of Aerodynamic Design for Cars

Coursework project for **Fluid Mechanics II (ME 322)**, GIK Institute of Engineering Sciences &
Technology — Instructor: Mr. Faheem Ahmed. Group 8: Muhammad Ahmed Shakeel (2020269),
Muhammad Taha Khan (2020363), Sheikh Abdul Basit (2020458).

**My contribution:** CFD simulation setup and meshing (grid-independence study). Modelling and
results/conclusion were led by Taha Khan, and the literature review by Abdul Basit.

## What this project is for

The goal was to quantify how much automotive aerodynamic design has actually improved over
time, by running the same CFD study — same solver settings, same scaled vehicle length (5 m),
same inlet conditions — on three cars from different eras:

- **Car A** — a 1930s-style boxy vintage car (blunt body, minimal streamlining)
- **Car B** — a Tesla Model S (2010s streamlined production sedan)
- **Car C** — an open-wheel electric formula race car (2020s, purpose-built for minimum drag)

Each was simulated in **ANSYS Fluent** at three air speeds representing real driving conditions —
**60 km/h** (city), **100 km/h** (highway), and **150 km/h** (race track) — using the standard
k-ε turbulence model and second-order-upwind spatial discretization, to compare drag force and
drag coefficient (Cd) across designs and speeds.

Full methodology (solver setup, boundary conditions, literature review) and all result figures
are in [`docs/CFD_Aerodynamic_Design_Report.pdf`](docs/CFD_Aerodynamic_Design_Report.pdf).

## Method summary

- **Mesh / grid-independence study** — run on the box-car model (the other two were assumed
  to share the same mesh-sensitivity behavior, due to limited compute budget). Mesh node count
  was swept from ~51,000 to ~68,400 nodes; average surface pressure was used as the convergence
  metric. A stable region was found between 56,000–60,000 nodes, and **40 edge-sizing
  divisions (57,396 nodes)** was selected as the working mesh. Raw sweep data:
  [`data/grid_independence_study.csv`](data/grid_independence_study.csv).
- **Final mesh sizes** (after geometry-specific refinement): Box car — 57,396 nodes; Tesla —
  378,247 nodes; Formula car — 10,399,078 nodes.
- **Solver** — pressure-based, steady-state, absolute reference frame, turbulent viscosity ratio
  of 10, second-order-upwind discretization throughout.
- Iteration budgets were capped by available compute: 100 iterations (box car, 7 min),
  40 iterations (Tesla, 2 h 20 min), 14 iterations (formula car, 3 h 45 min).

## Results

**Drag force (N) vs. speed:**

| Speed (km/h) | Box car | Tesla | Formula car |
|---:|---:|---:|---:|
| 60  | 729.0  | 314.1 | 211.5 |
| 100 | 2030.5 | 869.9 | 588.2 |
| 150 | 4568.0 | 1982.3 | 1306.8 |

**Coefficient of drag (Cd) vs. speed:**

| Speed (km/h) | Box car | Tesla | Formula car |
|---:|---:|---:|---:|
| 60  | 0.0865 | 0.0877 | 0.0881 |
| 100 | 0.2410 | 0.2429 | 0.2450 |
| 150 | 0.5422 | 0.5537 | 0.5443 |

**Findings:**

- The **formula car had the lowest drag force at every speed tested**, followed by the Tesla,
  with the boxy vintage car producing the highest drag by a wide margin (e.g. at 150 km/h the
  box car's drag force is ~2.3x the Tesla's and ~3.5x the formula car's) — consistent with its
  blunt frontal profile creating a large front/rear pressure differential and a turbulent wake.
- Drag force scales roughly with the square of speed for all three vehicles, as expected from
  drag theory (F ∝ V²).
- Interestingly, the drag **coefficients** for the three vehicles came out close to each other at
  a given speed (all rising from ~0.087 at 60 km/h to ~0.54–0.55 at 150 km/h) — the huge gap in
  drag *force* is driven mainly by frontal area and shape rather than Cd, and the Cd itself rises
  with speed (Reynolds number) rather than staying constant, per this study's simulation results.
- The formula car's much lower drag force despite a similar Cd to the other two vehicles is
  explained by its far smaller frontal area — its shape and Cd are tuned for a purpose-built
  race car, not for the passenger-carrying, taller profile of the other two.

## Repository contents

```
docs/   Full project report — literature review, ANSYS Fluent setup, meshing, all result plots
data/   Raw mesh-sensitivity sweep data used for the grid-independence study
```
