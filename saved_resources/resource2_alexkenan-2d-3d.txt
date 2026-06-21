SOURCE: https://www.alexkenan.com/pymae/sample/
TITLE:  Chapter 5: Modeling a 2-body orbit in 2D and 3D
SAVED:  June 2026
================================================================================

TOPICS COVERED
--------------
Modeling satellite orbits around Earth using orbital mechanics parameters.

ORBITAL PARAMETERS EXPLAINED
-----------------------------
  a      = semi-major axis (size of orbit)
  e      = eccentricity (shape: 0=circle, 0<e<1=ellipse, e=1=parabola, e>1=hyperbola)
  i      = inclination (tilt of orbital plane relative to reference)
  Omega  = longitude of ascending node (rotation of orbital plane)
  w      = argument of periapsis (rotation within orbital plane)
  per    = orbital period T

ORBIT TYPES BY ECCENTRICITY
-----------------------------
  e = 0:     Circular orbit
  0 < e < 1: Elliptical orbit (satellites, planets)
  e = 1:     Parabolic trajectory (escape)
  e > 1:     Hyperbolic trajectory (escape / flyby)

PYTHON LIBRARIES AND KEY METHODS
---------------------------------
PyAstronomy:
  from PyAstronomy.pyasl import KeplerEllipse
  ke = KeplerEllipse(a=1.0, per=1.0, e=0.5, Omega=0, i=30.0, w=0.0)
  pos = ke.xyzPos(t)     # returns [x, y, z] at time t

Matplotlib (2D animated orbit):
  from matplotlib.animation import FuncAnimation
  ani = FuncAnimation(fig, update_func, frames=n_frames, interval=50)

Matplotlib (3D static orbit):
  from mpl_toolkits.mplot3d import Axes3D
  ax = fig.add_subplot(111, projection='3d')
  ax.plot(x, y, z)

NumPy:
  t = np.linspace(0, T, 1000)   # time samples for one full orbit

VELOCITY BEHAVIOR
-----------------
  Speed increases toward periapsis (closest point)
  Speed decreases toward apoapsis (farthest point)
  This is a consequence of conservation of angular momentum (Kepler's 2nd law)

NUMERICAL METHODS MENTIONED
----------------------------
  Newton's Method, Runge-Kutta, Kepler's Method
  (The PyAstronomy library handles these internally)
