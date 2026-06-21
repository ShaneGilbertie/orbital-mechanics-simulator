SOURCE: https://en.wikipedia.org/wiki/Kepler_problem
TITLE:  Kepler problem — Wikipedia
SAVED:  June 2026
================================================================================

DEFINITION
----------
The Kepler problem is the special case of the two-body central force problem
where the force varies as the inverse square of the distance:
  F = k / r^2  (k = -G*m1*m2 for gravity, attractive)

FORCE AND POTENTIAL
-------------------
Central force:   F = (k/r^2) * r_hat
Scalar potential: V(r) = k/r

EQUATIONS OF MOTION (POLAR FORM)
---------------------------------
Radial equation:
  m*(d^2r/dt^2) - m*r*omega^2 = m*(d^2r/dt^2) - L^2/(m*r^3) = -dV/dr

Angular velocity: omega = d(theta)/dt

Angular momentum (conserved):
  L = m * r^2 * omega

TIME-TO-ANGLE SUBSTITUTION
----------------------------
  d/dt = (L / (m*r^2)) * d/d(theta)

Let u = 1/r (inverse distance variable):
  This transforms the radial ODE into the orbital shape equation.

ORBITAL SHAPE EQUATION (Conic Section)
---------------------------------------
  u = 1/r = -(k*m/L^2) * [1 + e*cos(theta - theta_0)]

Rearranged:
  r = -L^2/(k*m) / [1 + e*cos(theta - theta_0)]
    = p / [1 + e*cos(theta)]    where p = -L^2/(k*m) = h^2/mu

ECCENTRICITY-ENERGY RELATION
------------------------------
  e = sqrt(1 + 2*E*L^2 / (k^2*m))

For circular orbit (minimum energy for given L):
  E_circular = -k^2*m / (2*L^2)
  e = 0 (as expected)

ORBIT TYPES
-----------
  E < 0  →  e < 1  →  ellipse or circle (bound orbit)
  E = 0  →  e = 1  →  parabola (escape with zero speed at infinity)
  E > 0  →  e > 1  →  hyperbola (escape with nonzero speed at infinity)
