SOURCE: https://math.libretexts.org/.../3.5.05:_The_Two-body_Problem
TITLE:  The Two-body Problem — LibreTexts Differential Equations
SAVED:  June 2026
================================================================================

FUNDAMENTAL EQUATIONS
----------------------
Newton's Law of Gravitation:
  F = -G*m1*m2 / r^2 * r_hat

Individual equations of motion:
  m1 * r1_ddot = G*m1*m2*(r2-r1) / |r2-r1|^3
  m2 * r2_ddot = G*m1*m2*(r1-r2) / |r2-r1|^3

Relative motion (Kepler problem):
  r_ddot = -G*(m1+m2) * r / r^3

First-order ODE system (state variables x, y, vx=u, vy=v):
  dx/dt = u
  dy/dt = v
  du/dt = -mu * x / r^3
  dv/dt = -mu * y / r^3
  where mu = G*(m1+m2) and r = sqrt(x^2 + y^2)

CONSERVATION LAWS
-----------------
Energy:
  (1/2)*(vx^2 + vy^2) - mu/r = -mu/(2*a) = constant

Angular momentum:
  x*vy - y*vx = sqrt(mu * a * (1-e^2)) = constant

Orbital equation (Kepler's 1st law):
  r = a*(1-e^2) / (1 + e*cos(phi))

Kepler's 3rd law:
  T^2 = (4*pi^2 / mu) * a^3

INITIAL CONDITIONS AT PERIAPSIS
---------------------------------
  r(0) = (a*(1-e),  0)
  v(0) = (0,  sqrt(mu*(1+e) / (a*(1-e))))

NUMERICAL METHODS COMPARISON
-----------------------------

1. EULER'S METHOD (first-order, AVOID for orbits)
   v_n = v_{n-1} + dt * a(r_{n-1})
   r_n = r_{n-1} + dt * v_{n-1}
   Problem: Orbit spirals outward, energy not conserved.

2. SYMPLECTIC EULER / IMPLICIT EULER (first-order symplectic, ACCEPTABLE)
   v_n = v_{n-1} + dt * a(r_{n-1})
   r_n = r_{n-1} + dt * v_n          <- uses UPDATED velocity
   Much better energy conservation than standard Euler.

3. VELOCITY VERLET / STORMER-VERLET (second-order symplectic, RECOMMENDED)
   r_n   = r_{n-1} + v_{n-1}*h + (h^2/2)*a(r_{n-1})
   v_half = v_{n-1} + (h/2)*a(r_{n-1})
   a_n   = a(r_n)
   v_n   = v_half + (h/2)*a_n

   Properties:
   - Superior energy AND angular momentum conservation
   - Requires ~50x fewer timesteps vs. Euler for same accuracy
   - Standard choice for gravitational N-body simulations

UNIT SYSTEMS
------------
Astronomical units (AU, yr, solar masses):
  G = 4*pi^2  AU^3 yr^-2 M_sun^-1
  Earth: a=1 AU, T=1 yr  -->  mu = 4*pi^2
