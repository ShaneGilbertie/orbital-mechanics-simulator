================================================================================
  10 CURATED RESOURCES — TWO-BODY / N-BODY ORBITAL MECHANICS
  Shane Gilbert | June 2026
================================================================================

These resources are ordered from most immediately practical (Python tutorials)
to more theoretical (textbooks). Work through them roughly in this order
as you progress through the project phases.

================================================================================
RESOURCE 1 — BEST STARTING POINT (Python, Phase 1)
================================================================================
Title:   Two-Body Problem in the Co-Moving Frame — Orbital Mechanics & Astrodynamics
URL:     https://orbital-mechanics.space/the-n-body-problem/two-body-relative-numerical-solution.html
Type:    Free interactive textbook (web)

Why use it:
  The single best starting resource for this project. It walks through the
  exact ODE system you need to implement:
    x_ddot = -mu*x/r^3
    y_ddot = -mu*y/r^3
    z_ddot = -mu*z/r^3
  Uses scipy.integrate.solve_ivp with a user-defined function. Shows you
  how to define the state vector and extract the solution. Has runnable
  examples with real numbers (satellite altitude, orbital speed).

Relevant to: Phase 1 core ODE setup, Phase 2 3D extension

================================================================================
RESOURCE 2 — PYTHON ANIMATION TUTORIAL (Phase 1 & 2)
================================================================================
Title:   Chapter 5: Modeling a 2-body orbit in 2D and 3D
URL:     https://www.alexkenan.com/pymae/sample/
Type:    Free chapter from Python book

Why use it:
  Covers exactly the 2D → 3D progression you're building. Uses:
    - PyAstronomy KeplerEllipse class for generating orbit positions
    - matplotlib FuncAnimation() for animated 2D orbits
    - mpl_toolkits.mplot3d for 3D plots
  Explains orbital parameters: semi-major axis (a), eccentricity (e),
  inclination (i), argument of periapsis (w). Good for understanding
  what your initial conditions mean physically.

Relevant to: Phase 1 animation, Phase 2 3D visualization

================================================================================
RESOURCE 3 — STEP-BY-STEP PYTHON (Phase 1)
================================================================================
Title:   Use Python to Create Two-Body Orbits
URL:     https://towardsdatascience.com/use-python-to-create-two-body-orbits-a68aed78099c/
Type:    Towards Data Science article

Why use it:
  Practical walkthrough using numpy + scipy.odeint + matplotlib.
  Shows the exact code structure:
    1. Import libraries
    2. Define ODE function (ẍ = -mu*x/r^3, ÿ = -mu*y/r^3)
    3. Set initial conditions (position vector + velocity vector)
    4. Run odeint
    5. Plot results
  Provides working initial conditions with real numbers. Good for getting
  something running on day one.

Relevant to: Phase 1 (entire phase)

================================================================================
RESOURCE 4 — NUMERICAL METHODS DEEP DIVE (Phase 1-2)
================================================================================
Title:   The Two-body Problem (LibreTexts Differential Equations)
URL:     https://math.libretexts.org/Bookshelves/Differential_Equations/A_First_Course_in_Differential_Equations_for_Scientists_and_Engineers_(Herman)/03:_Numerical_Solutions/3.05:_Numerical_Applications/3.5.05:_The_Two-body_Problem
Type:    Free online textbook section

Why use it:
  This is the most mathematically rigorous free resource on NUMERICAL METHODS
  specifically for the two-body problem. Covers and compares:
    - Euler's Method           (first-order, poor energy conservation)
    - Implicit/Symplectic Euler (first-order but much better conservation)
    - Velocity Verlet          (second-order symplectic, RECOMMENDED)
  Includes the complete equations for each integrator. Explains why
  symplectic integrators are superior for orbital mechanics vs. standard
  Runge-Kutta. Has initial conditions at periapsis:
    r(0) = (a*(1-e), 0),  v(0) = (0, sqrt(mu*(1+e)/(a*(1-e))))

Relevant to: Phase 1 integrator choice, Phase 3-4 long-run accuracy

================================================================================
RESOURCE 5 — THREE-BODY PROBLEM TUTORIAL (Phase 3)
================================================================================
Title:   2-D Three-Body Problem Simulation Made Simpler with Python
URL:     https://medium.com/better-programming/2-d-three-body-problem-simulation-made-simpler-with-python-40d74217a42a
Type:    Medium / Better Programming article

Why use it:
  Direct practical tutorial for Phase 3. Uses scipy.integrate.solve_ivp
  with a 12-element state vector (3 bodies * 2D * 2 state vars each).
  Shows how to structure the ODE function for multiple bodies:
    - First 6 elements: velocity components (dr/dt = v)
    - Last 6 elements: acceleration from pairwise forces (dv/dt = F/m)
  Demonstrates the chaotic behavior and sensitivity to initial conditions.

Relevant to: Phase 3 (entire phase)

================================================================================
RESOURCE 6 — THREE-BODY CLASSICAL MECHANICS (Phase 3)
================================================================================
Title:   Modelling the Three Body Problem in Classical Mechanics using Python
URL:     https://medium.com/data-science/modelling-the-three-body-problem-in-classical-mechanics-using-python-9dc270ad7767
Type:    Towards Data Science / Medium article

Why use it:
  Approaches the three-body problem from a classical mechanics perspective,
  grounding the Python code in the physics. Uses numpy + scipy + matplotlib
  with 3D visualization. Discusses the chaotic nature and why general
  analytical solutions don't exist. Good complement to Resource 5.

Relevant to: Phase 3

================================================================================
RESOURCE 7 — COMPLETE THREE-BODY SIMULATOR (Phase 3)
================================================================================
Title:   Three-Body Problem Simulator (GitHub)
URL:     https://github.com/jman4162/Three-Body-Problem-Simulator
Type:    GitHub repository (Python source code)

Why use it:
  A complete working Python implementation you can study, run, and compare
  against your own. Uses RK45 (scipy solve_ivp) with interactive plots.
  Good for:
    - Seeing how a complete project is structured
    - Having a reference to test your own results against
    - Understanding visualization choices for multi-body systems
  Don't copy-paste — use it to understand and then write your own.

Relevant to: Phase 3

================================================================================
RESOURCE 8 — N-BODY GENERALIZATION (Phase 4)
================================================================================
Title:   N-body Problem (GitHub)
URL:     https://github.com/arpmay/N-body-Problem
Type:    GitHub repository (Python source code)

Why use it:
  Python implementation of N-body gravitational interactions in a 2D plane
  using numpy and matplotlib. Shows how to generalize from 3 bodies to N.
  Key pattern: nested loops (or vectorized numpy) over all pairs of bodies.
  Relevant equations: a_i = sum_{j≠i} G*mj*(rj-ri)/|rj-ri|^3

Relevant to: Phase 4

================================================================================
RESOURCE 9 — THEORY & ALL EQUATIONS (Reference)
================================================================================
Title:   Two-body problem — Wikipedia
URL:     https://en.wikipedia.org/wiki/Two-body_problem
Type:    Wikipedia (free)

Why use it:
  The most complete single-page reference for the theory behind everything
  you're building. Covers:
    - Reduction to one-body problem
    - Center of mass equations
    - Reduced mass derivation
    - Angular momentum conservation
    - Energy conservation
    - Orbital shape (conic sections)
    - Kepler's laws
  Use alongside the Kepler problem page:
    https://en.wikipedia.org/wiki/Kepler_problem

Relevant to: All phases (reference)

================================================================================
RESOURCE 10 — GRADUATE TEXTBOOK (Deep Theoretical Grounding)
================================================================================
Title:   Classical Mechanics — Herbert Goldstein, Poole, Safko (3rd Edition)
URL:     https://archive.org/details/ClassicalMechanicsGoldstein3ed/Classical_Mechanics_Goldstein_3ed
Type:    Free borrow on Internet Archive (free account required)

Why use it:
  THE standard graduate textbook on classical mechanics. Chapter 3 covers
  the two-body central force problem in complete mathematical detail:
    - Lagrangian formulation
    - Effective potential derivation
    - Complete orbital equation derivation
    - Kepler's laws from first principles
    - Scattering / hyperbolic orbits
  Not needed for the Python coding — but if you want to deeply understand
  WHERE every equation in this reference sheet comes from, this is the source.
  Chapter 3 is the relevant section.

Relevant to: All phases (theory depth)

================================================================================
BONUS — ADDITIONAL REFERENCES
================================================================================

Vis-viva equation (Wikipedia)
  https://en.wikipedia.org/wiki/Vis-viva_equation
  Short, focused article on the single most useful orbital speed equation.

Kepler problem (Wikipedia)
  https://en.wikipedia.org/wiki/Kepler_problem
  Derivation of the orbital shape equation from the equations of motion.

Python N-body simulation (The Cyber Omelette)
  https://www.cyber-omelette.com/2016/11/python-n-body-orbital-simulation.html
  Simple, readable N-body simulation code in Python.

Orbital Mechanics — FIU Lecture Notes (PDF)
  https://faculty.fiu.edu/~vanhamme/ast3213/orbits.pdf
  University lecture notes covering all the orbital mechanics equations
  with derivations at an astrophysics course level.

================================================================================
END OF RESOURCES
================================================================================
