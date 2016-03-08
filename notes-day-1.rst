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

FreeFem files
-------------

The source files for FreeFem are plain-text files, typically using the ``.edp`` extension. The syntax is similar to C++, but lacks namespaces.

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

FreeFem lets you write macros, which are useful for circles, etc. This is the same as the typical C precompiler. NB: for comments inside macros, we must use ``/* */`` and not ``//``, because the latter is used to end a macro.

The macro behaviour is just like C/C++: direct substitution of text, so be careful of expressions. For example, the use of macro::

    macro inc(x) x+1 //

    x = inc(x)*2

would give the code::

    x = x+1*2

which is likely not the intended result.

Likewise::

    macro double(x) x*2 //

    x = double(x+1)

gives::

    x = x+1*2

Labels
------

The label attribute is very important, especially when setting boundary conditions. Subdomains also have labels (details are in the ``mesh-subdomains.edp`` demo).

Border Definitions
------------------

Beyond the typical problems of Macro definitions, there are scoping issues. It's block-wise scoping and anything not in the blocks is global-scope. E.g. changing the value of a variable used in a border definition mucks everything up::

    real R = 1;
    border circle(t=0, 2*pi) {
        //...
        x = xc + R*cos(t);
        //...
    }

    R = R/2; // Changes the radius of the ``circle defined above``

    mesh( circle(10)); // gives a disc of radus 0.5
