# Setting up a site to use Spack

## Site versus individual installation

There will typically be either a group of administrators who manage the Spack
installation, or there will be a service account that is used by the
administrators.

The Spack _configuration scopes_ are important here.  Since Spack can be
used by anyone, not just the system administrators, the system administrators
should be careful when setting defaults for others.

Typically, I think, the central installation will be considered one of many
Spack installations.  That said, the Defaults and Site scopes should be used
wherever possible.  The System scope should be used sparingly, as it will
change the behavior of other people's Spack installations.

There are some examples of site configuration files at

https://github.com/spack/spack-configs/

in particular, the example for
<a href="https://github.com/spack/spack-configs/tree/master/ANL/LCRC">ANL/LCRC</a>
had the best explanations in the README.md and the best comments in the
configuration files themselves and seemed to most closely match what we are
trying to end up with.

Our past experience, and therefore our biases in thinking about things, comes
primarily from TeX, R, Python, and Lmod (hierarchies).
