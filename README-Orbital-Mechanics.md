# Orbital Mechanics Simulator 

A progressively complex orbital mechanics simulator built from scratch in Python. Starts with a 2D two-body animation and expands through four phases to a fully generalized N-body simulation in 3D space. All physics are derived from first principles using Newton's laws and numerical integration — no shortcut libraries for the core physics.

---

## What This Project Is

This is a self-directed physics and programming project to deeply understand gravitational orbital dynamics by building a simulator from the ground up. Each phase is a complete, working simulation that extends the previous one. By the end, the code handles an arbitrary number of bodies in 3D space with realistic gravitational interactions.

The motivating question is simple: given the positions and velocities of N masses at time t=0, where will they be at time t? The answer requires solving a system of coupled ordinary differential equations numerically — there is no closed-form solution once you have three or more bodies.

---

## Project Phases

### Phase 1 — 2D Two-Body Problem
Two objects with masses `m1` and `m2` exert gravitational forces on each other. The goal is to animate their mutual orbit around their shared center of mass in a 2D plane. Given initial positions and velocities, both bodies trace conic section paths — circle, ellipse, parabola, or hyperbola — depending on their total energy.

The two-body problem reduces to a single one-body ODE using the **reduced mass** and **relative position vector**, making it analytically tractable. Numerical results are validated against Kepler's analytical solutions.

**Milestone:** Two bodies orbit each other in a correct, animated 2D ellipse. The center of mass stays fixed; energy and angular momentum are conserved.

---

### Phase 2 — 3D Two-Body Problem
Exactly the same physics as Phase 1, extended to three dimensions. The state vector grows (adding z components), but the structure is identical. The orbital plane is now oriented arbitrarily in 3D space and is defined by the angular momentum vector `h = r × v`.

Key addition: visualization of the orbital plane itself, showing how **inclination** works as a 3D concept.

**Milestone:** Two-body physics running in full 3D with rotatable 3D view and orbital plane drawn.

---

### Phase 3 — 3D Three-Body Problem
Add a third body. There is **no closed-form analytical solution** for three or more bodies — numerical simulation is the only option. The system is **chaotic**: infinitesimally small changes in initial conditions produce exponentially diverging trajectories over time (the butterfly effect in gravitational systems).

The generalized acceleration law for each body becomes:

```
a_i = Σ_{j≠i}  G * mj * (rj - ri) / |rj - ri|³
```

Special known solutions (the figure-8 orbit, Lagrange points, Euler's collinear solution) are implemented and used to validate the integrator.

**Milestone:** Three bodies evolve under mutual gravity in 3D. Chaotic divergence is observable. Known special solutions are reproducible.

---

### Phase 4 — N-Body Generalization
The final generalization: an arbitrary number of bodies N. The force equation structure is identical to Phase 3 — the summation simply runs over all N(N-1)/2 pairs. Computational cost scales as O(N²) in the naive implementation; stretch goals include Barnes-Hut tree (O(N log N)) and GPU acceleration.

This phase focuses on **vectorized NumPy implementation** to handle larger systems efficiently, and explores scenarios like a solar system analog, binary star systems, and simplified galaxy mergers.

**Milestone:** Arbitrary N bodies simulate correctly. The code accepts N and a list of initial conditions and animates the full gravitational evolution.

---

## Core Physics

### Newton's Law of Gravitation
```
F = G * m1 * m2 / r²
```

### Reduction to One-Body Problem (Two-Body)
The relative position `r = r2 - r1` satisfies a single second-order ODE:
```
r̈ = -μ * r / |r|³       where μ = G(m1 + m2)
```
This is the equation used in the numerical integrator.

### First-Order System Fed to solve_ivp
```
state = [x, y, z, vx, vy, vz]

dx/dt  = vx           dy/dt  = vy           dz/dt  = vz
dvx/dt = -μx/r³       dvy/dt = -μy/r³       dvz/dt = -μz/r³
```

### Conserved Quantities (used to validate the integrator)
- **Specific angular momentum:** `h = r × ṙ` (constant — defines the orbital plane)
- **Specific orbital energy:** `ε = ½|ṙ|² - μ/r` (constant)
- **Eccentricity vector:** `e = (ṙ × h)/μ - r̂` (constant — points to periapsis)

### Vis-Viva Equation (most useful single equation)
```
v² = μ * (2/r - 1/a)
```
Where `v` = speed, `r` = current distance, `a` = semi-major axis.

### Orbital Period (Kepler's Third Law)
```
T = 2π * √(a³ / μ)
```

### Orbit Types by Eccentricity
| Eccentricity | Orbit Type |
|---|---|
| e = 0 | Circular |
| 0 < e < 1 | Elliptical (bound) |
| e = 1 | Parabolic (escape) |
| e > 1 | Hyperbolic (flyby) |

---

## Technical Stack

| Package | Role |
|---|---|
| `numpy` | Array math, broadcasting, vectorized force calculations |
| `scipy` | ODE solvers (`solve_ivp` with RK45) |
| `matplotlib` | 2D/3D plotting, `FuncAnimation` for live animation |
| `numba` *(optional)* | JIT-compile inner loops for speed |
| `ipywidgets` *(optional)* | Interactive sliders in Jupyter for real-time parameter changes |

**Recommended integrators (in order of preference for gravitational systems):**
1. `scipy.integrate.solve_ivp` (RK45) — easiest to start, good accuracy
2. Velocity Verlet — better long-run energy conservation (symplectic)
3. Leapfrog / Störmer-Verlet — gold standard for N-body gravitational work

---

## Repository Structure

```
orbital-mechanics-simulator/
├── README.md                        # This file
├── notebooks/
│   └── orbit.ipynb                  # Phase 1 sandbox — early gravity functions and setup
├── docs/
│   ├── 01_pipeline_roadmap.txt      # Full checklist for all four phases
│   ├── 02_equations_reference.txt   # Complete equations reference (14 sections)
│   └── 03_resources.txt             # 10 curated learning resources with annotations
└── saved_resources/                 # Archived content from key reference pages
    ├── resource1_orbital-mechanics-space.txt
    ├── resource2_alexkenan-2d-3d.txt
    ├── resource3_towardsdatascience-python.txt
    ├── resource4_libretexts-numerical-methods.txt
    ├── resource5_threebody-medium.txt
    ├── resource9_wikipedia-two-body.txt
    └── resource9b_wikipedia-kepler-problem.txt
```

---

## Linked Files
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/docs/02_equations_reference.txt]]
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/docs/03_resources.txt]]
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/saved_resources/resource1_orbital-mechanics-space.txt]]
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/saved_resources/resource2_alexkenan-2d-3d.txt]]
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/saved_resources/resource3_towardsdatascience-python.txt]]
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/saved_resources/resource4_libretexts-numerical-methods.txt]]
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/saved_resources/resource5_threebody-medium.txt]]
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/saved_resources/resource9_wikipedia-two-body.txt]]
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/saved_resources/resource9b_wikipedia-kepler-problem.txt]] *(resources 6, 7, 8, and 10 don't exist on disk — only 1-5, 9, 9b are present)*
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/notebooks/orbit.ipynb]]
- [[Projects/Code/Personal Coding Projects/orbital-mechanics-simulator/sandbox_Orbit/orbit.ipynb]] *(duplicate copy of the same sandbox notebook)*

## Current Status

Phase 1 is in progress. The notebook (`notebooks/orbit.ipynb`) contains initial gravity function definitions and imports. The full planning documents in `docs/` lay out the complete roadmap from Phase 1 through Phase 4.

---

*Shane Gilbert — June 2026*
[[Personal Coding Projects]]