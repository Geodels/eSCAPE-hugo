---
title: "About..."
date: "2019-04-12"
menu: "main"
#author: "Tristan Salles"
cover_image: "images/FA.png"
---

**eSCAPE** is a parallel TIN-based landscape evolution model, built to simulate topography dynamic at various space and time scales. The model accounts for hillslope processes (soil creep using _linear diffusion_), fluvial incision (_stream power law_), spatially and temporally varying tectonics (horizontal and vertical displacements) and climatic forces (temporal and spatial precipitation changes and/or sea-level fluctuations).

##### The specs

The model is based on the following approaches:

1. an adaptation of the implicit, parallelizable method for calculating drainage area for both single (D8) and multiple flow direction (Dinf) from [**Richardson et al. (2014)**](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2013WR014326),
2. the methods developed in [**pyBadlands**](https://github.com/badlands-model/pyBadlands_serial) ([**Salles et al. (2018)**](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0195557)) for marine sediment distribution.

##### License

This program is free software: you can redistribute it and/or modify it under the terms of the GNU Lesser General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public License along with this program.  If not, see <http://www.gnu.org/licenses/lgpl-3.0.en.html>.
