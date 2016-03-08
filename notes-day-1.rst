Freefem is an implementation of FEM
===================================

The focus of the workshop is implementation (not numerical methods, or physics). So, we'll gloss over the numerical analysis and theory.

Freefem is supposed to be an easy-to-use software that is very close to the mathematical theory.
- check theories,
- perform experiments with different techniques
- collaborate with physics and industry.

Selling pitch: solving Gross-Pitaevskii equation in about 50 lines of code.
- PDE solver using FEM in 2d and 3d
- syntax close to variational formulation
- includes a mesh generator that can also do adaptive meshes.
- uses P1, P2, P4 and "more exotic" spaces
- complex matrices supported *shrug*

Mesh geration can be given metrics/heuristics to control it. E.g. axi-symmetric.

One example: ice formation: navier-stokes-boussinesq, with phase-change. The mesh is adapted to be finer near the interfaces: two counter-roatating vortices, and solid-liquid. Ironically, nothing mentioned about the difficulty of the ice interface.


Meshes
------

Most of the details of execution are contained in the ``mesh-*.edp`` files. They can be executed by running ``FreeFem++ <file-name>`` on the command line (if FreeFem++ is installed in the ``$PATH``.

1) mesh of a circle
2) circle with a subdomain
3) anulus (holes)
4) Anulus using macros
5) multiple connecting segments (not done for time)

    When composing meshes, be aware of orientation of borders. If composing multiple border sebments together, the ``+`` operator must go in the couter-clockwise orientation as well.

    Plotting the borders (not mesh) will generally show if corners are missing, or orientations are correct.
6) "Smiley mesh"

Macros
------

FreeFem lets you write macros, which are useful for circles, etc.)

Labels
------

The label attribute is very important, especially when setting boundary conditions.
