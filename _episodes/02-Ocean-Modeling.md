---
title: "Ocean and Sea Ice Modeling"
questions:
- "How to Ocean and Ice Models work?"
objectives:
keypoints:
- ""
---

Just like the atmosphere and land models are typically run together, the ocean and sea ice models are also typically run together.  We will learn about what ocean and sea ice models predict, what they explicitly resolve, what they paramaterize, and some of the challenges of ocean and sea ice modeling.

## Ocean Models

The CESM2 ocean component is called POP (Parallel Ocean Program).  The next version of CESM (CESM3) will use a different ocean model called MOM (Modular Ocean Model).  Regardless of the exact model used, the fundamentals of ocean modeling are similar and consist of dynamics and physics. They may also consist of biological and processes as well.

1. Dynamics

* Seven Equations and 7 unknowns: U,V,W,T,S,rho,P
* Plus equations for tracers (chemical, biological, isotopes indicated age of water masses)
* Approximations: Hydrostatic, Boussinesq
* Linearize, discretize, solve numerically 

2. Parameterized Physics

* Vertical mixing: surface boundary layer, interior

* Lateral mixing: mesoscale eddies
Such as the Kuroshio, Gulf Stream, Aghulas, Brazil, etc.  This is weather in the ocean.  Unlike in the atmosphere, current climate resolution ocean models do not resolve ocean weather. 

* Horizontal viscosity

* Overflows (Deep channel and Continental shelves)
Parameterizes the acceleration of dense water down the slope of bottom topography in deep channels (Denmark Strait, Faroe Bank Channel) and continental shelves (Ross and Weddell Seas).  This acceleration of dense water causes vigorious mixing and entrainment that is smaller than the resolution of ocn models.

* Submesoscale eddies

* Estuary box model parmeterization (Rivers)

* Solar absorption: how is radiation absorbed at different depths?

## Some Challenges in Ocean Modeling

* Irregular Domain, including narrow straights and channels
* Spatial vs. Temporal Scales: cannot represent eddies ("ocean weather") in climate resoultion ocean models; Rossby radius of deformation is 10s-100s km vs. 1000 km in Atm.
* Equilibrium timescale: small amount of mixing across density surfaces means that the ocean takes a long time to come into equilibrium; have to accept that the deep ocean is not in equilibrium in many simulations
* Bottom topography: discretization creates step functions
* Large heat capacity
