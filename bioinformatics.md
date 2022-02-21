# Using Spack to provide bioinformatics packages

This page, and those referred to by it, reflect the changed goal of
providing a Spack managed installation of bioinformatics software on
clusters maintained by ARC at the University of Michigan.  It should
be replicable (reproducible?) on both Great Lakes and on Armis.

## Testing phase

The testing phase began in mid-Feb, 2022, with Jim Kenyon,
Ken Weiss, Richard Merkle participating from UM HITS, and
Shelly Johnson and Bennet Fauber participating from ARC.

The initial list of software required was provided by
HITS, and was

```
bamtools/2.5.1
bedtools2/2.29.2
blat/34
bowtie2/2.3.4.3
bowtie2/2.4.1
bwa/0.7.15
fastqc/0.11.8
gatk/4.1.0.0
gatk/4.2.0.0
ghostscript/9.27
htslib/1.9
metaphlan3/3.0
minimap2/2.17
monocle/3.0
ncbi-blast/2.9.0
samtools/1.9
sratoolkit/2.9.6
star/2.7.5a
```

All of the packages excepting monocle (R package) and metaphlan
(Python package) were installable in some version.

```
bamtools@2.5.1
bedtools2@2.29.2
blast-plus@2.12.0
blat@35
bowtie2@2.3.4.3
bowtie2@2.4.1
bwa@0.7.15
fastqc@0.11.9
ghostscript@9.54.0 ^meson@0.59.2
gatk@4.1.0.0
gatk@4.2.2.0
htslib@1.9
htslib@1.13
minimap2@2.17
samtools@1.9
samtools@1.13
sratoolkit@2.9.6
sratoolkit@2.10.9
star@2.7.6a
```

Note that some packages were sufficiently old that the compiler
(`gcc@10.3.0`) was incompatible with a source build.  To provide
exact, old versions, we would need to install a second, older
version of `gcc` (possibly `9.x`, but `8.x` would more certainly
work).


```
# ghostscript failed with meson @0.6.0,
# specifically at gtk-pixbuf, so we had to install
# meson and specify it for ghostscript and other packages
# that might use it.
meson@0.59.2
itk@5.2.1
mothur@1.46.1
vtk@9.0.3 ^hdf5+hl~mpi
```

A second selection of packages was added 21 Feb by Ken Weiss.

```
angsd/0.935
bamtools/2.5.2
bcl2fastq2/2.20.0.422
beast1/1.10.4
bedtools2/2.25.0
canu/2.1
clustalo/1.2.4
cufflinks/2.2.1
fastqc/0.11.5
gatk/3.7
macs2/2.1.2
mothur/1.43.0
mothur/1.45.3
RAxML/8.2.11
sratoolkit/2.10.4
stringtie/2.1.7
```

## Some notes on the first, test installation

Bennet tested installation of the first set of packages.  Some
versions were not available already from Spack.  For the most
part, adding a version appears to be a matter of downloading the
distribution file, calculating the `sha256sum` for it, and adding
it to the relevant place in the package file (`package.py` within
the package's `$spackrootvar/spack/repos/builtin/packages/<pkg_name>`
directory.

For the `blat` package, a change of GCC's behavior required a
change to the package file to inject the old value of the flag
(`-fcommon` rather than the new default `-fno-common`).

As you can see from the spec line for `meson`, the newest version also
had a behavior change, so we are using an older version to install
`ghostscript`.  Finally, while installing `vtk`, we discovered that
a dependency with a default can add a significant number of packages
to the installation list.  In this case, `hdf5` defaulted to
including MPI (`+mpi`), when we wanted it excluded.

The `vtk` package had a conditional to automatically handle using
`netcdf-c` without MPI when `vtk~mpi` was specified, so adding
comparable lines for `hdf5+hl~mpi` solved that problem.  It may
also have been solvable by adding `^hdf5+hl~mpi` to the spec
for `vtk` installation.

## Things to be investigated

Here are some open questions.

1. We would like to have only Spack-built compilers visible.
   If we install `gcc@10.3.0`, can we then remove the default
   compiler from the system (`gcc@8.5.0`)?
1. If we have only `gcc@10.3.0` in `compilers.yml`, will it get
   automatically built?
1. Understand the use of Spack environments and the difference
   between those and modules.
1. Use the `packages.yml` file, and of the `modules.yml` files.
1. Setting the default to try to use what's already installed
   rather than building the most recent.

There are more, but that's plenty for now.
