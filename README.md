# Setting up a site to use Spack

## Site versus individual installation

There will typically be either a group of administrators who manage the
Spack installation, or there will be a service account that is used by
the administrators.

The Spack _configuration scopes_ are important here.  Since Spack can be
used by anyone, not just the system administrators, the system administrators
should be careful when setting defaults for others.

Typically, I think, the central installation will be considered one of
many Spack installations.  That said, the Defaults and Site scopes should
be used wherever possible.  The System scope should be used sparingly,
as it will change the behavior of other people's Spack installations.

There are some examples of site configuration files at

https://github.com/spack/spack-configs/

in particular, the example for
<a href="https://github.com/spack/spack-configs/tree/master/ANL/LCRC">ANL/LCRC</a>
had the best explanations in the README.md and the best comments in the
configuration files themselves and seemed to most closely match what we
are trying to end up with.

Our past experience, and therefore our biases in thinking about things,
comes primarily from TeX, R, Python, and Lmod.

### Design considerations

There are two main goals for installing software that we are trying
to attain: to provide the software typically required to _build other
software_ and to provide software that people use to _do work in their
area of research_.

In what follows, we will start with the former because, as site
administrators, we need that to create the latter.

The list of software that is used to build other software is quite extensive,
so we will start with what we think is a plausible but fairly minimal list.

The first choice that needs to be made is the compiler.  We are going to
choose to use the following compilers.

GCC 4.8.5 because it is the system compiler for our CentOS 7 platform

GCC 8.2.0 because it will be the default version when we update to
CentOS 8.

Intel x.y.z because many people use it; version to be determined later.

For each of the compilers, we will have some standard set of libraries
that are, insofar as possible, available for all installed compilers at
the same version.  Many people compile with multiple compilers to compare
performance, and providing closely matching versions of all libraries
built upward facilitates that task.

Our standard _development libraries_ will include:  FTTW3, HDF5, NetCDF/C
and NetCDF/Fortran, Eigen, Boost, GSL, and some optimized BLAS/LAPACK
implemenation.  All of those will be provided without support for or
dependency on any MPI implementation.

Once those have been installed for use with non-MPI applications, we will
install two flavors of MPI:  OpenMPI and Intel MPI.  Several of the above
libraries need to be build with MPI support for parallel capability: FFTW3,
HDF5, NetCDF/C, and Boost all have MPI support.

There are other libraries that are often needed, for example, libraries
to handle image manipulation (JPEG, et al.), libraries to handle things
like cryptography (SSL), and many other functions.  Some of these will be
installed by the system and used by all compilers; others will be compiled
for each compiler.

We also know that we must provide versions of the interpreted languages
R and Python, and some collection of libraries for each of those.

With all that in mind, we read the
<a href="https://spack-tutorial.readthedocs.io/en/latest/tutorial_basics.html">Spack
Installation Tutorial</a> and the <a href="">Spack Configuration
Tutorial</a>, and we install Spack at least twice according to the former,
so we get an idea of how Spack works.

Then we erase everything and start over trying to build the basics of what
we described above.

We choose here to do this in the following order.

1. We will register the system GCC as a compiler.

1. We will install gcc@8.2.0 using Spack and we will register it as an
   available compiler.

1. We will set up a matrix configuration to install Eigen, FFTW3, GSL, and
   HDF5 for both of those compilers.

Then we will pause and consider the next steps.  Those should be available
for both compilers, they should not produce any problems with Spack
configuration beyond installing and registering them, so we can concentrate
on the _structure_ of our site.


