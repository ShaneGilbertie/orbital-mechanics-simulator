================================================================================
  ORBITAL MECHANICS SIMULATION PROJECT — PIPELINE & ROADMAP
  Shane Gilbert | June 2026
================================================================================

GOAL
----
Build a progressively complex orbital mechanics simulator, starting with a
simple 2D two-body animation and expanding to a fully generalized N-body
simulation in 3D volumetric space. All physics must be realistically simulated
using Newton's laws and numerical integration — no shortcuts or canned libraries
for the core physics.

================================================================================
PHASE 1: 2D TWO-BODY PROBLEM (Core Foundation)
================================================================================

CONCEPT
-------
Two objects with masses m1 and m2 exert gravitational forces on each other.
The goal is to animate their mutual orbit around the shared center of mass
in a 2D plane. Start by entering initial positions and velocities, and
watch the bodies trace elliptical (or other conic section) paths.

PHYSICS SUMMARY
---------------
- Newton's law of gravity governs the force between the two bodies.
- The motion of both bodies can be reduced to tracking:
    (a) The center of mass (which moves at constant velocity)
    (b) The relative position vector r = r2 - r1
- The relative motion obeys a single second-order ODE that is equivalent to
  a one-body problem with the "reduced mass."
- Orbits are conic sections (circle, ellipse, parabola, hyperbola) depending
  on the total energy and angular momentum.

WHAT NEEDS TO BE DONE — PHASE 1
---------------------------------
[ ] Set up a Python project (Jupyter notebook or .py file)
    - Libraries: numpy, scipy, matplotlib, matplotlib.animation

[ ] Define constants and initial conditions
    - Gravitational constant G
    - Masses m1, m2
    - Initial positions r1 = (x1, y1), r2 = (x2, y2)
    - Initial velocities v1 = (vx1, vy1), v2 = (vx2, vy2)
    - (Ensure center-of-mass velocity is set for a stable bound system)

[ ] Build the ODE function
    - State vector: [x1, y1, x2, y2, vx1, vy1, vx2, vy2]
    - Compute r = r2 - r1, distance d = |r|
    - Compute acceleration of each body: a = G*m_other/d^2 * (r_hat)
    - Return derivatives [vx1, vy1, vx2, vy2, ax1, ay1, ax2, ay2]

[ ] Choose and apply a numerical integrator
    - RECOMMENDED: scipy.integrate.solve_ivp with RK45 method
    - Alternative for physics accuracy: Velocity Verlet (symplectic)
    - Euler method: simplest to implement but poor energy conservation
    - Set time span and time steps

[ ] Compute and verify conserved quantities over time
    - Total energy E = KE + PE should be constant
    - Total angular momentum L = sum(r x p) should be constant
    - Plot these vs. time to validate your integrator

[ ] Build the 2D matplotlib animation
    - Use matplotlib.animation.FuncAnimation
    - Plot trajectories of both bodies
    - Mark the center of mass with a crosshair
    - Add a trail (last N positions) for each body
    - Display current time, energy, and distance on the plot

[ ] Add user input for initial conditions
    - Let the user enter: masses, starting distance apart, initial velocities
    - Compute which orbit type results (circular, elliptical, escape)
    - Display orbital parameters: period T, eccentricity e, semi-major axis a

[ ] Implement different orbit types as presets
    - Circular orbit (e = 0)
    - Elliptical orbit (0 < e < 1)
    - Parabolic escape (e = 1)
    - Hyperbolic flyby (e > 1)

[ ] Validate results against analytical Kepler solutions
    - Compare simulated period to T = 2*pi*sqrt(a^3 / mu)
    - Compare orbital shape to r = p / (1 + e*cos(theta))

MILESTONE: Two bodies orbit each other in a correct, animated 2D ellipse.
The center of mass stays fixed, energy and angular momentum are conserved.

================================================================================
PHASE 2: 3D TWO-BODY PROBLEM (Extension to Volumetric Space)
================================================================================

CONCEPT
-------
Extend Phase 1 to 3 dimensions. The physics are identical — only the state
vector grows. Now the orbital plane can be oriented arbitrarily in 3D space
and you can visualize how the plane is defined by the angular momentum vector.

WHAT NEEDS TO BE DONE — PHASE 2
---------------------------------
[ ] Extend state vector to 3D
    - State: [x1,y1,z1, x2,y2,z2, vx1,vy1,vz1, vx2,vy2,vz2]
    - ODE function is the same structure, just with z components added
    - r = sqrt(x^2 + y^2 + z^2)

[ ] Set up Matplotlib 3D plotting (mpl_toolkits.mplot3d)
    - Animate both bodies in a 3D axes
    - Draw the orbital plane using the angular momentum vector as the normal
    - Show axis labels: X, Y, Z with units (AU, km, etc.)

[ ] Add orbital plane visualization
    - Compute h = r x v (specific angular momentum vector)
    - Draw a semi-transparent plane with normal = h
    - Show how inclination affects the visible orbit shape

[ ] Experiment with non-planar initial conditions
    - Give bodies velocity components out of the x-y plane
    - Observe that the orbit stays in a plane (defined by the initial h)
    - This demonstrates why 2-body orbits are always planar

[ ] Add interactive controls (optional but educational)
    - Sliders for inclination angle, initial velocity direction
    - Rotate the 3D view interactively

MILESTONE: The same two-body physics run in 3D. You can rotate the view,
see the orbital plane, and understand inclination as a 3D concept.

================================================================================
PHASE 3: 3D THREE-BODY PROBLEM (Chaos Begins)
================================================================================

CONCEPT
-------
Add a third body. There is NO closed-form analytical solution for 3+ bodies.
Numerical simulation is required. The system is chaotic: tiny changes in initial
conditions produce wildly different long-term trajectories.
Famous special cases: figure-8 orbit, Lagrange points, horseshoe orbits.

WHAT NEEDS TO BE DONE — PHASE 3
---------------------------------
[ ] Generalize the ODE function to 3 bodies
    - State vector: 18 elements (3 bodies * 3 positions + 3 velocities)
    - Each body's acceleration = sum of forces from the other 2 bodies
    - Force from body j on body i: F_ij = G*m_i*m_j*(r_j - r_i)/|r_j - r_i|^3
    - Add a softening parameter epsilon to avoid singularities at close approach:
      |r| -> sqrt(|r|^2 + epsilon^2)

[ ] Handle close encounters (collision prevention)
    - Check minimum separation distance each step
    - Apply softening or a collision/merge event

[ ] 3D animation of three bodies with trails
    - Use three different colors
    - Plot center of mass (should still move at constant velocity)

[ ] Implement and compare numerical methods
    - RK45 (scipy solve_ivp) — good general purpose
    - Velocity Verlet — better energy conservation for long runs
    - Compare energy conservation across methods

[ ] Implement known special solutions as validation
    - Euler's collinear solution
    - Lagrange's equilateral triangle solution (stable for specific mass ratios)
    - Figure-8 orbit (use known initial conditions from Chenciner & Montgomery 2000)

[ ] Explore chaos
    - Run two simulations with initial conditions differing by 1e-10
    - Plot the divergence of trajectories over time (Lyapunov exponent concept)
    - Demonstrate the butterfly effect in gravitational systems

[ ] Add energy/momentum monitoring
    - Total energy and total angular momentum should remain constant
    - Plot conservation errors to assess integrator quality

MILESTONE: Three bodies evolve under mutual gravity in 3D. Chaotic behavior
is observable. Known special solutions (figure-8) are reproducible.

================================================================================
PHASE 4: N-BODY GENERALIZATION (Arbitrary Number of Bodies)
================================================================================

CONCEPT
-------
Generalize to an arbitrary number of bodies N. The structure of the equations
is the same but the summation runs over all pairs. Computational cost scales
as O(N^2) naively; advanced algorithms (Barnes-Hut tree, Fast Multipole Method)
scale as O(N log N) or O(N) but are much more complex to implement.

WHAT NEEDS TO BE DONE — PHASE 4
---------------------------------
[ ] Refactor ODE function to accept a list of N bodies
    - Input: array of shape (N, 6) — [x, y, z, vx, vy, vz] per body
    - Compute all pairwise forces (double loop or vectorized with numpy)
    - Return accelerations for all N bodies

[ ] Vectorize using numpy for performance
    - Compute all pairwise distance vectors using broadcasting
    - Avoid explicit Python loops for the inner force calculation
    - This gives O(N^2) but in fast C-level numpy operations

[ ] Extend animation to N bodies
    - Dynamically allocate colors, trail arrays
    - Label each body with its mass or index

[ ] Add body spawning/removal at runtime (optional)
    - Let user add bodies mid-simulation with mouse clicks
    - Handle merge events (combine bodies when they get too close)

[ ] Simulate interesting N-body scenarios
    - Small solar system analog (star + planets)
    - Binary star system with orbiting planet (hierarchical triple)
    - Cluster of stars with similar masses
    - Galaxy merger (simplified: 2 clusters of point masses)

[ ] Performance profiling and optimization
    - Compare numpy vectorized vs. Python loop timings
    - Introduce Barnes-Hut tree as a stretch goal for N > 1000
    - Consider GPU acceleration (cupy or numba CUDA) for very large N

[ ] Add dimensionless units for scalability
    - Choose units so G = 1 (or scale masses/distances to simplify)
    - This avoids floating-point issues with very large/small numbers

MILESTONE: Arbitrary N bodies simulate correctly. The code accepts N and a
list of initial conditions and animates the full gravitational evolution.

================================================================================
TECHNICAL STACK
================================================================================

Core:
  Python 3.x (your system Python or conda base)
  numpy          — array math, broadcasting, vectorized operations
  scipy          — ODE solvers (solve_ivp, odeint)
  matplotlib     — 2D/3D plotting, FuncAnimation for animation

Optional upgrades:
  numba          — JIT-compile Python loops for speed
  vispy / mayavi — faster 3D rendering than matplotlib for large N
  ipywidgets     — interactive sliders in Jupyter for real-time parameter changes
  rebound        — professional N-body integrator (for comparison/validation)
  astropy        — unit handling and physical constants

Numerical integrators (in order of recommendation for this project):
  1. scipy solve_ivp (RK45)    — easiest to start, good accuracy
  2. Velocity Verlet            — better for long-run energy conservation
  3. Leapfrog / Störmer-Verlet  — gold standard for gravitational N-body
  4. RK4 (hand-coded)           — educational, understand what scipy does

================================================================================
LEARNING PROGRESSION SUMMARY
================================================================================

Phase 1: 2D two-body  →  Understand Newton's gravity, ODE setup, animation
Phase 2: 3D two-body  →  Understand 3D space, orbital planes, inclination
Phase 3: 3D three-body →  Meet chaos theory, numerical limitations
Phase 4: N-body        →  Generalize everything, scale to complex systems

Each phase builds directly on the last. The core ODE function structure never
changes — you only add more bodies and more dimensions to the state vector.

================================================================================
END OF DOCUMENT
================================================================================
