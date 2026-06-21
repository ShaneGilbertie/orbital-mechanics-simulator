SOURCE: https://medium.com/better-programming/2-d-three-body-problem-simulation-made-simpler-with-python-40d74217a42a
TITLE:  2-D Three-Body Problem Simulation Made Simpler with Python
SAVED:  June 2026
================================================================================

CORE APPROACH
-------------
Newton's second-order nonlinear differential equation decomposed into
two first-order ODEs per body. Uses scipy.integrate.solve_ivp (RK45).

STATE VECTOR
------------
12-element array for three bodies in 2D:
  Y = [x1, y1, vx1, vy1,
       x2, y2, vx2, vy2,
       x3, y3, vx3, vy3]

ODE FUNCTION STRUCTURE (Planets function)
-----------------------------------------
Inputs: Y (12-element state), t (time), masses m1, m2, m3

First 6 outputs (position derivatives = velocities):
  dY[0] = Y[2]   # dx1/dt = vx1
  dY[1] = Y[3]   # dy1/dt = vy1
  dY[2] = Y[4]   # dx2/dt = vx2
  ... etc.

Last 6 outputs (velocity derivatives = accelerations):
  For body 1:
    r12 = sqrt((x2-x1)^2 + (y2-y1)^2)
    r13 = sqrt((x3-x1)^2 + (y3-y1)^2)
    ax1 = G*m2*(x2-x1)/r12^3 + G*m3*(x3-x1)/r13^3
    ay1 = G*m2*(y2-y1)/r12^3 + G*m3*(y3-y1)/r13^3
  Repeat for bodies 2 and 3 (sum forces from the other two).

SOLUTION APPROACH
-----------------
  sol = solve_ivp(Planets, t_span, Y0, method='RK45', ...)
  x1 = sol.y[0], y1 = sol.y[1]
  x2 = sol.y[4], y2 = sol.y[5]
  x3 = sol.y[8], y3 = sol.y[9]

KEY INSIGHT
-----------
"An N-body system where N>2 exhibits great sensitivity to initial conditions."
This is deterministic chaos: tiny differences in starting positions lead to
completely different long-term trajectories.

LIBRARIES USED
--------------
  numpy      - array math
  scipy      - solve_ivp (RK45)
  matplotlib - animation and plotting
