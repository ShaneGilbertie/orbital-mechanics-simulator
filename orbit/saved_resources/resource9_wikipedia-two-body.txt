SOURCE: https://en.wikipedia.org/wiki/Two-body_problem
TITLE:  Two-body problem — Wikipedia
SAVED:  June 2026
================================================================================

EQUATIONS OF MOTION (INDIVIDUAL)
---------------------------------
Newton's second law for each body:
  F_12 = m1 * r1_ddot
  F_21 = m2 * r2_ddot

CENTER OF MASS
--------------
Acceleration: R_cm_ddot = 0
Position:     R = (m1*x1 + m2*x2) / (m1 + m2)

(Center of mass moves at constant velocity, no external forces.)

REDUCED PROBLEM
---------------
Reduced mass: mu_r = m1*m2 / (m1 + m2)

Equation for relative displacement vector r = r2 - r1:
  mu_r * r_ddot = F(r)

For gravity: F(r) = -G*m1*m2 * r_hat / r^2
So: r_ddot = -G*(m1+m2) * r / r^3

RECOVERING INDIVIDUAL TRAJECTORIES
------------------------------------
Once R_cm(t) and r(t) are known:
  x1(t) = R(t) + [m2/(m1+m2)] * r(t)
  x2(t) = R(t) - [m1/(m1+m2)] * r(t)

ANGULAR MOMENTUM
----------------
  L = r x p = r x mu_r*(dr/dt)
  dL/dt = r x F    (= 0 for central forces, so L is conserved)

ENERGY
------
Total:
  E_tot = (1/2)*m1*v1^2 + (1/2)*m2*v2^2 + U(r)

In center-of-mass frame:
  E = (1/2)*mu_r*r_dot^2 + U(r)

KEY INSIGHT
-----------
The two-body problem ALWAYS reduces to a one-body problem:
a particle of reduced mass mu_r moving in the potential U(r).
The motion is always confined to a plane (defined by the initial
angular momentum vector). This is exact — no approximations needed.
