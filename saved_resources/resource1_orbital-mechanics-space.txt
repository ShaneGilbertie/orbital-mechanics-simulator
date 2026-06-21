SOURCE: https://orbital-mechanics.space/the-n-body-problem/two-body-relative-numerical-solution.html
TITLE:  Two-Body Problem in the Co-Moving Frame — Orbital Mechanics & Astrodynamics
SAVED:  June 2026
================================================================================

EQUATIONS OF MOTION
-------------------
The fundamental differential equations governing relative motion:

  x_ddot = -mu * (x / r^3)
  y_ddot = -mu * (y / r^3)
  z_ddot = -mu * (z / r^3)

Where mu = G*(m1+m2) is the gravitational parameter and r is the scalar separation.

REFERENCE FRAME
---------------
Analysis uses "a reference frame attached to m1" — treating one body as stationary
while tracking the other's relative motion.

STATE VECTOR
------------
Six variables must be tracked at each time step:
  [x, y, z, vx, vy, vz]
  Three position components and three velocity components.

NUMERICAL SOLUTION METHOD
--------------------------
Python: scipy.integrate.solve_ivp()
  - Define a function that takes (t, state) and returns derivatives
  - State[0:3] = positions [x, y, z]
  - State[3:6] = velocities [vx, vy, vz]
  - Function returns [vx, vy, vz, ax, ay, az]
  - ax = -mu * x / r^3, etc.

ALTITUDE CALCULATION (for Earth-satellite problems)
----------------------------------------------------
  h = |r| - R_E     where R_E = 6378 km (Earth radius)

EXAMPLE RESULTS
---------------
Satellite simulation yielded:
  Minimum altitude: 3,621.88 km at 7.00 km/s
  Maximum altitude: 9,431.85 km at 4.44 km/s
(Demonstrates vis-viva: faster at periapsis, slower at apoapsis)
