---
date: 2018-09-14
linktitle: Installation
menu:
  main:
    parent: tutorials
#author: "Tristan Salles"
title: Installation
next: /tutorials/inputfile
weight: 10
cover_image: "images/escapezoom.jpg"
---

##### Getting started

For installation information and documentation visit our Github [**wiki page**](https://github.com/Geodels/eSCAPE/wiki) which provides a quick guide on the installation dependencies.

A set of examples are available in the [eSCAPE-demo](https://github.com/Geodels/eSCAPE-demo) repository.

The easiest way to get started is with the **Docker container**
[https://hub.docker.com/u/geodels/](https://hub.docker.com/u/geodels/) using [Kitematic](https://docs.docker.com/kitematic/userguide/). Once **Kitematic** is installed on your computer, open it and look for **Geodels escape-docker** via the *search* menu.

If you want to install it yourself, you can follow the steps provided in the [wiki](https://github.com/Geodels/eSCAPE/wiki/Installation-on-HPC) page.

##### Required libraries

**Scientific computing libraries:**

1. [numpy](http://www.numpy.org)
2. [scipy](https://www.scipy.org)
3. [mpi4py](https://mpi4py.readthedocs.io/en/stable/)
4. [petsc4py](https://petsc4py.readthedocs.io/en/stable/)
5. fortran compiler, preferably [gfortran](https://gcc.gnu.org/wiki/GFortran)
6. [fillit](https://github.com/Geodels/fillit)

**Reading/Writing/Parsing libraries:**

1. [ruamel.yaml](https://yaml.readthedocs.io/en/latest/)
2. [pandas](https://pandas.pydata.org)
3. [meshio](https://github.com/nschloe/meshio)
4. [h5py](https://www.h5py.org)
5. [meshplex](https://github.com/nschloe/meshplex)

###### Docker

To use/test **eSCAPE** quickly, it is recommended to use the `Geodels escape-docker` image that is shipped with all the required libraries.

[https://hub.docker.com/u/geodels/](https://hub.docker.com/u/geodels/)

###### Installation

Once the libraries mentioned above have been installed, **eSCAPE** will need to be cloned and compiled using the following:

```bash
git clone https://github.com/Geodels/eSCAPE.git
python setup.py install
```

An example on how to install it on a _HPC server_ is provided in the [**wiki**](https://github.com/Geodels/eSCAPE/wiki/Installation-on-HPC) page.


***

##### Step-by-step guide

_Content_

1. [Dependencies](#dependencies)
2. [Installing petsc petsc4py locally](#installing-petsc-petsc4py-locally)
3. [Installation on Artemis HPC USyD](#installation-on-artemis-hpc-usyd)
4. [Example of PBS script](#example-of-pbs-script)


##### Dependencies

When installing **eSCAPE** for the first time the following steps are required:

```bash
module purge
module load python/2.7.11
module load gcc/5.4.0
module load openmpi-gcc/1.10.3

pip install numpy --user
pip install scipy --user
pip install pandas --user
pip install h5py --user
pip install mpi4py --user
pip install ruamel.yaml --user
pip install meshio --user
```

Then you will install `meshplex` library:

```bash
git clone https://github.com/Geodels/meshplex.git
cd meshplex
python setup.py install --user
cd ..
```

Now `petsc4py`: in case everything works nicely you should be able to do:

```bash
module load petsc-gcc-openmpi/3.8.4
export PETSC_DIR=/usr/local/petsc-gcc-openmpi/3.8.4/
pip install petsc4py --user
```

(see next section if you need to build `petsc` locally...)

Last required package for running **eSCAPE** is `FILLIT`:

```bash
export F90=gfortran
git clone https://github.com/Geodels/fillit.git
cd fillit
python setup.py install --user
```

OK, now that all dependencies are installed we can proceed with **eSCAPE**:

```bash
git clone https://github.com/Geodels/eSCAPE.git
cd eSCAPE
python setup.py install --user
```

##### Installing petsc petsc4py locally

```bash

module load cmake
git clone https://bitbucket.org/petsc/petsc.git
cd petsc
export PETSC_VERSION=3.9.3
git checkout tags/v$PETSC_VERSION

cd /home/path
export PETSC_DIR=/home/path/petsc

./configure --CFLAGS='-O3' --CXXFLAGS='-O3' --FFLAGS='-O3' --CPPFLAGS=$CPPFLAGS --LDFLAGS=$LDFLAGS  --MPI-DIR=$MPI_DIR  --with-debugging=no  --download-fblaslapack --download-ctetgen --download-metis=yes --download-parmetis=yes --download-triangle

make PETSC_DIR=/home/path/petsc PETSC_ARCH=arch-linux2-c-opt all

pip install petsc4py --user
```

##### Installation on Artemis HPC USyD


```bash
module purge
module load python/2.7.15-intel petsc-intel-mpi hdf5
export F90=ifort
git clone https://github.com/Geodels/fillit.git
cd fillit
python setup.py install --user

git clone https://github.com/Geodels/eSCAPE.git
cd eSCAPE
python setup.py install --user
```

##### Example of PBS script

Here is an example of PBS job for `Artemis` HPC (USyD):

```bash
#!/bin/bash

# Project
#PBS -P BGH

# 40 CPUs
#PBS -l select=48:ncpus=8:mpiprocs=8:mem=8GB

# Time limit
#PBS -l walltime=10:00:00
#PBS -q alloc-dm

# Set up environment
module load python/2.7.15-intel petsc-intel-mpi hdf5

cd $PBS_O_WORKDIR
cd earth

# Launching the job!
mpirun -np 384 python run_espace.py
```

with `run_espace.py` a python script calling **eSCAPE** function:

```python
import eSCAPE as sim

# Reading input file
model = sim.LandscapeEvolutionModel('input_globe.yml',False,False)

# Running model
model.runProcesses()

# Cleaning model
model.destroy()
```

Use `qsub` command to launch.
