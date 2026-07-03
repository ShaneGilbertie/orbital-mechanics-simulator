SOURCE: https://towardsdatascience.com/use-python-to-create-two-body-orbits-a68aed78099c/
TITLE:  Use Python to Create Two-Body Orbits
SAVED:  June 2026
================================================================================

CORE CONCEPT
------------
Simulating spacecraft motion under gravitational influence using Python.
One mass treated as negligible → only the larger body's gravity matters.
(For equal-mass two-body: use the relative motion formulation instead.)

KEY EQUATIONS
-------------
  ax = -mu * x / r^3
  ay = -mu * y / r^3
  az = -mu * z / r^3
  State vector: [x, y, z, vx, vy, vz]

For Earth problems: mu = 3.986e5 km^3/s^2

STEP-BY-STEP CODE STRUCTURE
-----------------------------
Step 1: Import libraries
  import numpy as np
  from scipy.integrate import odeint
  import matplotlib.pyplot as plt

Step 2: Define ODE function
  def two_body(state, t, mu):
      x, y, z, vx, vy, vz = state
      r = np.sqrt(x**2 + y**2 + z**2)
      ax = -mu * x / r**3
      ay = -mu * y / r**3
      az = -mu * z / r**3
      return [vx, vy, vz, ax, ay, az]

Step 3: Set initial conditions
  # Example: x=-2500 km, vy=7.5 km/s
  state0 = [-2500, 0, 0,  0, 7.5, 0]

Step 4: Run ODE solver
  t = np.linspace(0, 21600, 200)   # 6-hour simulation
  solution = odeint(two_body, state0, t, args=(mu,))

Step 5: Visualize
  ax = plt.figure().add_subplot(projection='3d')
  ax.plot(solution[:,0], solution[:,1], solution[:,2])

NOTES
-----
- This is an approximation; real orbits have perturbation forces:
  lunar gravity, solar radiation pressure, atmospheric drag
- For a true two-body equal-mass system: simulate both bodies with
  forces on each from the other, or use reduced-mass formulation
