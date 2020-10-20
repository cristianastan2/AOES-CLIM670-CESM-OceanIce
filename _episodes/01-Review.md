---
title: "Review"
questions:
- "What have we learned so far?"
- "Where are we going?"
objectives:
keypoints:
- ""
---

Midway through the semester is a good time to pause and review what we have learned so far and what we have left.  

## What have we learned so far?

* Some Unix commands
* How to use JupyterLab
* Reading and plotting Climate Model Output
* An overview of Atmospheric Models
* How to run CESM in fully coupled mode (B compset)
* How to modify the CESM to change how long to run, what to output and how often
* How to understand and postprocess CESM output
* How to run diagnostics packages for CESM
* Some advanced Python functions -- groupby
* How to setup and run the Atmosphere and Land components of CESM with DOCN (F Compset)
* How to troubleshoot our model runs 

## Where are we going?

* Overview of Ocean and Ice Modeling
* How to run the Ocean and and Ice components of CESM with DATM
* How to change model code
* How to perform Intervention or Idealized experiments
* Reduced complexity models, why use them? how to run them?
* How to run an Initialized Prediction Experiment (Forecast)
* Regional Modeling
* Land and Land Ice
* Your projects!

## Atmospheric Modeling

1. Dynamics (Equations of motion)
Assumptions: hydrostatic
Linearize, discretize, solve numerically
Predict:  thickness(w),u,v,q,T

2. Physics (Sub grid-scale Parameterized)
* Clouds
* Radiation
* Convection
* Microphysics
* Boundary layer
* Fluxes
* Gravity waves

3. Boundary Forcing
Information exchnaged with or prescried by other components of the climate system (e.g. soil moisture, sea surface temperature)


## CESM Quickstart

A review of our CESM quickstart procedure

1. Create a newcase (this script is located in CIMEROOT/scripts)
~~~
./create_newcase --case CASEROOT --res RESOLUTION --compset COMPSET --project UGMU0032
~~~
{: .language-bash}

2. Setup the case (run from your case directory)
~~~
./case.setup
~~~
{: .language-bash}

3. Make your changes to the namelists, .xml files, and/or source codes

Use `xmlchange` for `.xml` files

Use `user_nl_xxx` for namelist changes

Use `SourceMods/src.xxx` for code changes (we will talk about this today)

4. Build the case
~~~
qcmd -- ./case.build
~~~
{: .language-bash}

4. Submit the run
~~~
./case.submit
~~~
{: .language-bash}

## CESM Compsets we have used so far

* `B` all components are active (fully coupled)
* `F` atmosphere and land are active; ocean and ice are inactive (data)
* `G` ocean and ice are active; atmosphere and land are inactive (data)

