---
date: 2018-09-14
linktitle: Input files
menu:
  main:
    parent: tutorials
#author: "Tristan Salles"
title: Input files
next: /tutorials/examples
prev: /tutorials/instaallation
weight: 10
cover_image: "images/depo_ero.gif"
---

Input files for **eSCAPE** are based on [YAML](https://circleci.com/blog/what-is-yaml-a-beginner-s-guide/) syntax.

A typical file will look like this:

***

```YAML
name: Description of the your simulation...

domain:
    filename: ['data/inputfileparameters.vtk','Z']
    flowdir: 5
    bc: 'slope'

time:
    start: 0.
    end: 1000000.
    tout: 1000.
    dt: 100.

sea:
    position: 0.
    curve: 'data/sealevel.csv'

climate:
    - start: 0.
      uniform: 1.0
    - start: 500000.
      map: ['data/inputfileparameters.vtk','R']
    - start: 500000.
      uniform: 2.0

tectonic:
    - start: 0.
      map: ['data/inputfileparameters.vtk','T1']
    - start: 100000.
      uniform: 0.
    - start: 50000.
      map: ['data/inputfileparameters.vtk','T2']

spl:
    Ke: 1.e-5

diffusion:
    hillslopeK: 5.e-2
    streamK: 300.
    oceanK: 10.
    maxIT: 2000

output:
    dir: 'outputDir'
    makedir: False

```

The YAML structure is shown through indentation (one or more spaces) and sequence items are denoted by a dash. At the moment the following component are available:

+ `domain`: definition of the unstructured grid containing the vtk grid `filename` and the associated field (here called `Z`) as well as the flow direction method to be used `flowdir` that takes an integer value between 1 (for SFD) and 12 (for Dinf) and the boundary conditions (`bc`: 'flat', 'fixed' or 'slope')
+ `time`: the simulation time parameters defined by `start`, `end`, `tout` (the output interval) and `dt` (the internal time-step).

Follows the optional forcing conditions:

+ `sea`: the sea-level declaration with the relative sea-level `position` (m) and the sea-level `curve` which is a file containing 2 columns (time and sea-level position).
+ `climatic` & `tectonic` have the same structure with a sequence of events defined by a starting time (`start`) and either a constant value (`uniform`) or a `map`.

Then the parameters for the surface processes to simulate:

+ `spl`: for the _stream power law_ with a unique parameter `Ke` representing the The erodibility coefficient which is scale-dependent and its value depend on lithology and mean precipitation rate, channel width, flood frequency, channel hydraulics. It is worth noting that the coefficient _m_ and _n_ are fixed in this version and take the value 0.5 & 1 respectively.
+ `diffusion`: hillslope, stream and marine diffusion coefficients. `hillslopeK` sets the _simple creep_ transport law which states that transport rate depends linearly on topographic gradient. River transported sediment trapped in inland depressions or  internally draining basins are diffused using the coefficient (`streamK`). The marine sediment are transported based on a diffusion coefficient `oceanK`. The parameter `maxIT` specifies the maximum number of steps used for diffusing sediment during any given time interval `dt`.

Finally, you will need to specify the output folder:

+ `output`: with `dir` the directory name and the option `makedir` that gives the possible to delete any existing output folder with the same name (if set to False) or to create a new folder with the give `dir` name plus a number at the end (e.g. outputDir_XX if set to True with XX the run number)

***
