# Installing and configuring Spack for a site

We start by installing Spack onto a shared file system.  At our site, this
is at /sw/core.  

```
$ cd /sw/core
$ git clone https://github.com/spack/spack
$ cd spack
$ git checkout releases/v0.15
$ source share/spack/setup-env.sh
```

Check that all came out right with

```
$ spack --version
```

We declared at the outset that we know we want both the system GCC and GCC
8.2.0 for compatibility with what we expect our next system GCC to be after
an OS upgrade.  So, we will start by installing and registering `gcc@8.2.0`.
We are on a four-core machine, so we will explicitly say to use four cores
to build this package.

```
$ spack install gcc@8.2.0 -j 4
```

Once that has completed, we will add it as an available compiler with

```
$ spack compiler add $(spack location -i gcc@8.3.0)
```

