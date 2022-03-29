# Testing production release scheme

One of our challenges is that we do not have an actual, real build
environment at our site.  All our software is installed onto one
NFS server which is then shared to both production and development
clusters.

Because we are just starting with Spack, we think it best to be
able to test installation of new software first, then once we know
what needs to be done with respect to dependency resolution to make
things compatible, we then install it onto the multiple production
clusters.

## The first scheme

The first scheme we came up with is to install Spack into a
temporary area, get the configuration set up as we want, then
test installing packages.  Once we have the packages we want
installed and defined in the various Spack configuration files,
we create a Spack environment that includes what we would like
users to see.  We then clone Spack into a production release
location, copy the configuration files there, and use the
environment's `spack.yaml` and `spack.lock` files to reinstall
in the production release location.
