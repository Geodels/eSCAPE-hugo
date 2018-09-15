---
#author: "Tristan Salles"
date: 2018-09-14
linktitle: Examples
menu:
  main:
    parent: tutorials
title: Hands-on tutorials
prev: /tutorials/inputfile
weight: 10
cover_image: "images/earthdepo1.gif"
---

To get some additional info in regards to how to use **eSCAPE** a series of examples and tutorials is provided in the docker container (`Geodels escape-docker`) and is also available for  download from the [eSCAPE-demo](https://github.com/Geodels/eSCAPE-demo) repository.

##### Usage

Either via _jupyter notebooks_ or _python_ files.

```bash
python run_eSCAPE.py -i input.yml -v
```

where the `run_eSCAPE.py` script takes one required argument the input filename and an optional verbose command (`-v`).  To run the script in parallel simply use the `mpirun` command. As an example with N processors it will look like:

```bash
mpirun -np N python run_eSCAPE.py -i input.yml
```

`run_eSCAPE.py` consists of a limited number of calls to **eSCAPE**

```python
import eSCAPE
model = eSCAPE.LandscapeEvolutionModel(***)
model.runProcesses()
model.destroy()
```

as shown below:

```python
import argparse
import eSCAPE as sim

# Parsing command line arguments
parser = argparse.ArgumentParser(description='This is a simple entry to run eSCAPE model.',add_help=True)
parser.add_argument('-i','--input', help='Input file name (YAML file)',required=True)
parser.add_argument('-v','--verbose',help='True/false option for verbose', required=False,action="store_true",default=False)
parser.add_argument('-l','--log',help='True/false option for PETSC log', required=False,action="store_true",default=False)

args = parser.parse_args()
if args.verbose:
  print("Input file: {}".format(args.input))
  print(" Verbose is on? {}".format(args.verbose))
  print(" PETSC log is on? {}".format(args.log))

# Reading input file
model = sim.LandscapeEvolutionModel(args.input,args.verbose,args.log)

# Running model
model.runProcesses()

# Cleaning model
model.destroy()
```

##### Examples...


###### 1- Synthetic model

+ repo - `mountain`

###### 2- Regional model

+ repo - `bluemountains`


###### 3- Continental model

+ repo - `australia`

###### 4- Global model

+ repo - `earth`
