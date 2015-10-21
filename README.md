# LibLSM

<p>Copyright &copy; 2015 <a href="http://lesterhedges.net">Lester Hedges</a>
<a href="http://www.gnu.org/licenses/gpl-3.0.html">
<img width="80" src="http://www.gnu.org/graphics/gplv3-127x51.png"></a></p>

## About
A simple C++ library to implement the Level Set Method (LSM) for peforming
structural topology optimisation. Based on code written by
[Peter Dunning](http://www.abdn.ac.uk/engineering/people/profiles/peter.dunning).

The aim is to provide a robust and exstensible, object-oriented topology
optimisation framework.

## Installation
A `Makefile` is included for building and installing LibLSM.

To compile LibLSM, then install the library, documentation, and demos:

```bash
$ make build
$ make install
```

By default, the library installs to `/usr/local`. Therefore, you may need admin
priveleges for the final `make install` step above. An alternative is to change
the install location:

```bash
$ PREFIX=MY_INSTALL_DIR make install
```

Further details on using the Makefile can be found by running make without
a target, i.e.

```bash
$ make
```

Due to a bug in `clang` it's likely that the code will not compile on a
default OS X build (due to complaints abount ambiguous calls to abs). If
`g++` is also installed, you can override the default compiler by running:

```bash
$ CXX=g++ make build
```

Note that `g++` may well be aliased to `clang`, in which case you will
need to pass the full name of the `g++` executable, e.g. for the latest
`g++` from MacPorts

```bash
$ CXX=g++-mp-4.9 make build
```

Alternatively, don't use OS X :-)

## Compiling and linking
To use LibLSM with a C/C++ code first include the LibLSM header file somewhere
in the code.

```cpp
//example.cpp
#include <lsm/lsm.h>
```

Then to compile, we can use something like the following:

```bash
$ g++ -std=c++11 example.cpp -llsm
```

This assumes that we have used the default install location `/usr/local`. If
we specify an install location, we would use a command more like the following:

```bash
$ g++ -std=c++11 example.cpp -I/my/path/include -L/my/path/lib -llsm
```

Note that the `-std=c++11` compiler flag is needed for `std::function` and
`std::random`.

## Dependencies
LibLSM uses the [Mersenne Twister](http://en.wikipedia.org/wiki/Mersenne_Twister)
psuedorandom number generator. A C++11 implementation using `std::random` is
included as a bundled header file, `MersenneTwister.h`. See the source code or
generate Doxygen documentation with `make doc` for details on how to use it.

## Tests
A test suite is provided in the `tests` directory. To run all unit tests:

```bash
$ make test
```

Note that this will compile the tests (and library) using the default compilation
flags (`release`). To build the library and tests in a specific mode, run, e.g.

```bash
$ make devel test
```

## Demos
Current there is a single demo showing how to instantiate objects and perform
calculations.

* `demos/lsm.cpp`

The makefile will build an executable, which can be run (from the top level
directory) as follows

```bash
$ ./demos/lsm
```

## Completed

### Mesh
Stores and initialises the two-dimensional finite element mesh. This
creates the required elements and nodes and stores information regarding their
connectivity. Support is provided for periodic and non-periodic meshes.

### LevelSet
Holds information relating to the level set function. Methods are
provided to initialise the signed distance function (either using a vector of
holes, or in a default "Swiss cheese" configuration) and to reinitialise it
using the fast marching method.

### FastMarchingMethod
An implementation of the fast marching method to find approximate solutions
to boundary value problems of the Eikonal equation. This is adapted from
[scikit-fmm](https://github.com/scikit-fmm/scikit-fmm), for which I've added
a few performance tweaks and bug fixes. The object provides functionality for
calculating signed distances and extension velocities.

### Boundary
An object for the discretised boundary. The `discretise` method solves for
a set of boundary points and segments given the current mesh and level set.

### InputOutput
Provides functionality for reading and writing data structures. Currently only
writes level set information in ParaView readable VTK format. Methods should
be able to read/write files in the current directory, or from a user defined
path.

## To Do
Next on the agenda...

* Add tests for `LevelSet`, `FastMarchingMethod`, and `Boundary` objects.
* Add method to compute element are fractions to `Boundary` object.
* Start working on `Sensitivity` object.

## Limitations
* Limited to two-dimensional systems.
* Finite element mesh is assumed to be a fixed two-dimensional grid comprised
of square elements of unit side.