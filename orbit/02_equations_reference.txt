================================================================================
  COMPLETE EQUATIONS REFERENCE — TWO-BODY STELLAR / ORBITAL MOTION
  Shane Gilbert | June 2026
================================================================================

NOTATION USED THROUGHOUT
-------------------------
  G        = gravitational constant = 6.674e-11 N m^2 kg^-2
  m1, m2   = masses of the two bodies
  r1, r2   = position vectors of body 1 and body 2
  r        = relative position vector = r2 - r1  (or r1 - r2, sign convention varies)
  |r|      = r = scalar distance between the two bodies
  r_hat    = r / |r|  (unit vector pointing from one body to the other)
  v1, v2   = velocity vectors
  a1, a2   = acceleration vectors
  mu       = G * (m1 + m2)   = standard gravitational parameter of the system
  mu_r     = m1*m2/(m1+m2)   = reduced mass
  M        = m1 + m2         = total mass
  dot      = d/dt (time derivative)
  ddot     = d^2/dt^2 (second time derivative)

================================================================================
SECTION 1: FUNDAMENTAL FORCE LAW
================================================================================

1.1 Newton's Universal Law of Gravitation (scalar)
    F = G * m1 * m2 / r^2

1.2 Gravitational Force (vector) on body 2 from body 1
    F_21 = G * m1 * m2 * (r1 - r2) / |r2 - r1|^3

    On body 1 from body 2:
    F_12 = -F_21       (Newton's third law)

1.3 Potential Energy
    U(r) = -G * m1 * m2 / r

================================================================================
SECTION 2: EQUATIONS OF MOTION (INDIVIDUAL BODIES)
================================================================================

2.1 Equation of Motion for Body 1
    m1 * r1_ddot = G * m1 * m2 * (r2 - r1) / |r2 - r1|^3

2.2 Equation of Motion for Body 2
    m2 * r2_ddot = G * m1 * m2 * (r1 - r2) / |r2 - r1|^3

2.3 Cartesian Components (body i, attracted by body j)
    ai_x = G * mj * (rj_x - ri_x) / r^3
    ai_y = G * mj * (rj_y - ri_y) / r^3
    ai_z = G * mj * (rj_z - ri_z) / r^3
    where r = sqrt((rj_x-ri_x)^2 + (rj_y-ri_y)^2 + (rj_z-ri_z)^2)

================================================================================
SECTION 3: CENTER OF MASS
================================================================================

3.1 Center of Mass Position
    R_cm = (m1 * r1 + m2 * r2) / (m1 + m2)

3.2 Center of Mass Velocity
    V_cm = (m1 * v1 + m2 * v2) / (m1 + m2)

3.3 Center of Mass Acceleration
    R_cm_ddot = 0     (no external forces — center of mass moves at constant velocity)

3.4 Center of Mass Motion
    R_cm(t) = R_cm(0) + V_cm * t

3.5 Individual Positions from CM and Relative Position
    r1(t) = R_cm(t) - [m2 / (m1 + m2)] * r(t)
    r2(t) = R_cm(t) + [m1 / (m1 + m2)] * r(t)

================================================================================
SECTION 4: REDUCTION TO ONE-BODY PROBLEM
================================================================================

4.1 Relative Position Vector
    r = r2 - r1

4.2 Relative Velocity
    r_dot = v2 - v1

4.3 Reduced Mass
    mu_r = m1 * m2 / (m1 + m2)

4.4 Equation of Motion for Relative Vector (THE KEY EQUATION)
    r_ddot = -G * (m1 + m2) * r / |r|^3
           = -mu * r / |r|^3
           = -mu * r_hat / r^2
    where mu = G * (m1 + m2)

    This is a single second-order ODE for the relative position r.
    It looks exactly like the equation of a particle orbiting a fixed mass mu/G.

4.5 Component Form (for numerical integration)
    x_ddot = -mu * x / r^3
    y_ddot = -mu * y / r^3
    z_ddot = -mu * z / r^3
    where r = sqrt(x^2 + y^2 + z^2)

4.6 First-Order System (what you actually feed to solve_ivp)
    Let state = [x, y, z, vx, vy, vz]
    dx/dt  = vx
    dy/dt  = vy
    dz/dt  = vz
    dvx/dt = -mu * x / r^3
    dvy/dt = -mu * y / r^3
    dvz/dt = -mu * z / r^3

================================================================================
SECTION 5: CONSERVED QUANTITIES
================================================================================

5.1 Specific Angular Momentum (per unit reduced mass) — VECTOR
    h = r x r_dot           (cross product)
    |h| = r^2 * theta_dot   (in 2D polar)
    dh/dt = 0               (h is conserved — defines the orbital plane)

    This means: the orbit always stays in a plane whose normal is h.

5.2 Angular Momentum (full)
    L = mu_r * h = mu_r * (r x r_dot)
    dL/dt = 0

5.3 Specific Orbital Energy (per unit reduced mass)
    epsilon = (1/2) * |r_dot|^2 - mu / r

    Note: epsilon = -mu / (2a) where a is the semi-major axis.
    Negative epsilon: bound orbit (ellipse/circle)
    Zero epsilon:     parabolic escape trajectory
    Positive epsilon: hyperbolic flyby

5.4 Total Mechanical Energy
    E = (1/2) * m1 * |v1|^2 + (1/2) * m2 * |v2|^2 - G*m1*m2 / r
      = (1/2) * M * |V_cm|^2 + (1/2) * mu_r * |r_dot|^2 - G*m1*m2 / r

5.5 Eccentricity Vector (Laplace-Runge-Lenz Vector) — ALSO CONSERVED
    e_vec = (r_dot x h) / mu  -  r_hat
          = (|r_dot|^2/mu - 1/r)*r  -  (r . r_dot)/mu * r_dot

    |e_vec| = e (the orbital eccentricity)
    e_vec points toward the periapsis (closest approach point)

================================================================================
SECTION 6: ORBITAL SHAPE (CONIC SECTIONS)
================================================================================

6.1 Orbital Equation in Polar Form (Kepler's First Law)
    r = p / (1 + e * cos(theta))
    or equivalently:
    r = a*(1-e^2) / (1 + e*cos(theta))

    where:
      p = semi-latus rectum = h^2 / mu
      e = eccentricity
      theta = true anomaly (angle from periapsis)
      a = semi-major axis

6.2 Orbit Types by Eccentricity
    e = 0          : circular orbit
    0 < e < 1      : elliptical orbit (bound)
    e = 1          : parabolic trajectory (marginally unbound)
    e > 1          : hyperbolic trajectory (unbound, flyby)

6.3 Eccentricity from Energy and Angular Momentum
    e = sqrt(1 + 2 * epsilon * h^2 / mu^2)
    where epsilon = specific orbital energy, h = |specific angular momentum|

================================================================================
SECTION 7: ORBITAL PARAMETERS (ELLIPTIC ORBIT, e < 1)
================================================================================

7.1 Semi-Major Axis
    a = -mu / (2 * epsilon)          (from energy)
    a = p / (1 - e^2)                (from shape)

7.2 Semi-Minor Axis
    b = a * sqrt(1 - e^2)

7.3 Semi-Latus Rectum
    p = h^2 / mu = a * (1 - e^2)

7.4 Periapsis Distance (closest approach)
    r_peri = a * (1 - e)

7.5 Apoapsis Distance (farthest point)
    r_apo  = a * (1 + e)

7.6 Periapsis from apoapsis and vice versa
    a = (r_peri + r_apo) / 2
    e = (r_apo - r_peri) / (r_apo + r_peri)

================================================================================
SECTION 8: ORBITAL VELOCITY
================================================================================

8.1 Vis-Viva Equation (THE most useful single equation in orbital mechanics)
    v^2 = mu * (2/r - 1/a)
    where:
      v = speed at distance r
      mu = G*(m1+m2)
      r = current distance between bodies
      a = semi-major axis

    Special cases:
      Circular orbit (r = a):  v_c = sqrt(mu/r)
      Periapsis:               v_peri = sqrt(mu*(1+e) / (a*(1-e)))
      Apoapsis:                v_apo  = sqrt(mu*(1-e) / (a*(1+e)))
      Parabolic escape (a=inf): v_esc = sqrt(2*mu/r)

8.2 Escape Velocity (from distance r)
    v_esc = sqrt(2 * G * M / r) = sqrt(2*mu/r)

8.3 Circular Orbital Speed
    v_c = sqrt(G * M / r) = sqrt(mu/r)

8.4 Velocity Components in Polar Form
    v_r     = (mu/h) * e * sin(theta)          (radial component)
    v_theta = (mu/h) * (1 + e*cos(theta))      (tangential component)
    |v|^2   = v_r^2 + v_theta^2

================================================================================
SECTION 9: ORBITAL PERIOD AND TIME
================================================================================

9.1 Orbital Period (Kepler's Third Law)
    T^2 = (4 * pi^2 / mu) * a^3
    T   = 2 * pi * sqrt(a^3 / mu)
    T   = 2 * pi * a * b / h         (from angular momentum)

    In SI units with m1>>m2: T = 2*pi * sqrt(a^3 / (G*m1))

9.2 Mean Motion (average angular rate)
    n = 2*pi / T = sqrt(mu / a^3)

9.3 Areal Velocity (Kepler's Second Law)
    dA/dt = h / 2 = constant
    (equal areas swept in equal times)

9.4 Mean Anomaly (uniformly increasing angle)
    M = n * (t - t_peri)     where t_peri = time of periapsis passage

9.5 Kepler's Equation (relates time to position in orbit)
    M = E - e * sin(E)
    where E = eccentric anomaly (angle in circumscribed circle, not orbital angle)

    Must be solved numerically for E given M (Newton-Raphson method):
      E_{n+1} = E_n - (E_n - e*sin(E_n) - M) / (1 - e*cos(E_n))

9.6 True Anomaly from Eccentric Anomaly
    tan(theta/2) = sqrt((1+e)/(1-e)) * tan(E/2)
    or:
    cos(theta) = (cos(E) - e) / (1 - e*cos(E))
    sin(theta) = sqrt(1-e^2)*sin(E) / (1 - e*cos(E))

================================================================================
SECTION 10: EFFECTIVE POTENTIAL (INSIGHT INTO ORBITAL MOTION)
================================================================================

10.1 Radial Equation of Motion (in polar)
     mu_r * r_ddot = -dV_eff/dr
     where the effective potential combines gravity and centrifugal repulsion:

10.2 Effective Potential
     V_eff(r) = -G*m1*m2/r  +  L^2 / (2 * mu_r * r^2)
                ^gravity         ^centrifugal barrier

     The second term (L^2/2mu_r r^2) prevents collapse to r=0 for L≠0.

10.3 Turning Points (periapsis and apoapsis)
     Solve V_eff(r) = E_total for r
     These give r_peri and r_apo directly.

================================================================================
SECTION 11: KEPLER'S THREE LAWS (SUMMARY)
================================================================================

Law 1: The orbit of each planet is an ellipse with the Sun at one focus.
  → Derived from the inverse-square force law (conic section solution)

Law 2: The line joining a planet to the Sun sweeps equal areas in equal times.
  → Equivalent to conservation of angular momentum: dA/dt = h/2 = const

Law 3: The square of the orbital period is proportional to the cube of
       the semi-major axis.
  → T^2 = (4*pi^2 / mu) * a^3

================================================================================
SECTION 12: INITIAL CONDITIONS FOR A STABLE CIRCULAR ORBIT
================================================================================

To start two bodies in a stable circular orbit around their common center of mass:

12.1 Separate bodies at distance d apart (along x-axis):
     r1 = (-m2/(m1+m2)) * d * x_hat    (body 1 to the left of CM)
     r2 = (+m1/(m1+m2)) * d * x_hat    (body 2 to the right of CM)

12.2 Required orbital speed for each body:
     omega = sqrt(G * M / d^3)          (angular velocity of orbit)
     v1 = -omega * |r1| * y_hat         (perpendicular to separation)
     v2 = +omega * |r2| * y_hat

     Or equivalently using the relative orbit:
     v_relative = sqrt(mu / d)          (circular orbital speed of relative motion)

12.3 Check: total momentum = m1*v1 + m2*v2 should = 0 if CM is at rest.

================================================================================
SECTION 13: THREE-BODY EXTENSION (Phase 3)
================================================================================

For three bodies (1, 2, 3), the equations generalize directly:

13.1 Acceleration of body i from all other bodies
     a_i = sum over j≠i of [G * mj * (rj - ri) / |rj - ri|^3]

13.2 For N bodies:
     a_i = sum_{j=1, j≠i}^{N}  G * mj * (rj - ri) / |rj - ri|^3

13.3 Softening (avoids singularity at close approach)
     Replace |rj - ri|^3 with (|rj - ri|^2 + epsilon^2)^(3/2)
     where epsilon is a small softening length (e.g., 1e-3 in your units)

13.4 State vector for N bodies
     Shape: (N * 6,) = [x1,y1,z1,vx1,vy1,vz1, x2,y2,z2,...,xN,yN,zN,vxN,vyN,vzN]

13.5 Total energy (N bodies)
     E = sum_i (1/2)*m_i*|v_i|^2  -  sum_{i<j} G*m_i*m_j / |r_i - r_j|

13.6 Total momentum (N bodies) — conserved
     P = sum_i m_i * v_i = constant

================================================================================
SECTION 14: USEFUL UNIT SYSTEMS
================================================================================

SI Units
  G = 6.674e-11 N m^2 kg^-2
  Masses in kg, distances in m, time in s

Astronomical Units (for solar-system scale)
  1 AU = 1.496e11 m
  1 solar mass = 1.989e30 kg
  In AU/yr with solar masses: G = 4*pi^2  (nice and clean)
  Earth orbit: a = 1 AU, T = 1 yr  ->  mu = 4*pi^2 AU^3/yr^2

Dimensionless / N-body Units (most flexible for simulation)
  Set G = 1, M = 1, characteristic length L = 1
  Time unit = sqrt(L^3 / (G*M))
  All other quantities follow from these three.

================================================================================
END OF EQUATIONS REFERENCE
================================================================================
