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

This is where it gets tricky.  Your compilers are now defined in
`~/.spack/linux/compilers.yaml`, which doesn't help your site.  So, you 
must first remove that file, then run

```
$ rm ~/.spack/linux/compilers.yaml
$ spack compiler find --scope site
==> Added 1 new compiler to /sw/core/spack/etc/spack/compilers.yaml
    gcc@4.8.5
==> Compilers are defined in the following files:
    /sw/core/spack/etc/spack/compilers.yaml
```

Then we can come back to the tutorial and add 

```
$ spack compiler add --scope site $(spack location -i gcc@8.2.0)
==> Added 1 new compiler to /sw/core/spack/etc/spack/compilers.yaml
    gcc@8.2.0
==> Compilers are defined in the following files:
    /sw/core/spack/etc/spack/compilers.yaml
```
and verify that we have both with

```
$ spack compilers
==> Available compilers
-- gcc centos7-x86_64 -------------------------------------------
gcc@8.2.0  gcc@4.8.5

```

