---
title: "CESM G compset"
questions:
- "How to run Ocean and Ice models?"
objectives:
keypoints:
- ""
---

We will setup and run 4 ocean/sea-ice experiments to learn how to work with these components.

Ocean/ice active compsets start with `G`.

Just like the `F` compsets needed SST data as the boundary forcing, `G` compsets require boundary forcing from the atmosphere.  The standard atmospheric forcing provided to a `G` compset is called CORE (Coordinated Ocean-ice Reference Experiments), version 2, atmopsheric datasets (Large and Yeager 2009)

* `G`: this is a standard ocean/ice with data atmosphere/land compset in which the boundary forcing is called `normal year forcing (NYF)`, meaning 12 months of CORE forcing data that repeats (think like a climatology).
* `GIAF`: this is ocean/ice activate with data atmosphere/land in which the boundary forcing is interannually varying from CORE forcing data.


## Our Experiments

### Control Case: default settings, 1 year__

Setup, build, and run a control case for 1 year. Use the following:
* `compset`=`G`
* `resolution`=`T62_g37`


### Overflow Parameterization Case

Create a case exactly the same as the control case, but with the overflow parameterization turned off. 

This means we will not attempt to parameterize the mixing associated with dense water sinking in the Denmark Strait, Faroe Bank Channel, and Weddell and Ross Seas.

Modifying parameterizations is done in the namelists.  

After we build the model or run `./preview_namelists`, the namelists that the model uses are located in:

`/glade/scratch/USERNAME/CASENAME/run/xxx_in`


Remember, when we want to make a change to a namelists, we use `user_nl_xxx` to override the default behavior.
We can always run `./preview_namelists` to check our namelists get resolved properly.  

To create an exact replica of another case, you can use `create_clone` rather than `create_newcase`, as follows, replacing `NEWCASE` and `OLDCASE` with the correct case names:

~~~
./create_clone --case ~/cases/NEWCASE --clone ~/cases/OLDCASE
~~~
{: .language-bash}

Setup and build the case.

Modify the `user_nl_pop` namelist as follows:

`overflows_on = .false.`

`overflows_interactive = .false.`

Run the case for 1 year. 

> ## Where do you expect the overflow parameterization to matter?
>
> What variables would you look at in your output?
>
{: .challenge}

### Change the snow albedo on sea ice case

Create a clone of your control simulation.

Modify `user_nl_cice` as follows:

`r_snw = 2.00`

This is a tuning parameter that specifies the number of standard deviations away from the base optical properties in the shortwave 
radiative transfer code.  The default value is -2.00.  It is used in the equation:  `rsnw_nonmelt = 500 - r_snw * 250` (in microns).

Higher values of r_snw -> lower rsnw_nonmelt -> higher albedo.

Run the model for 1-year.

>## What do you think will happen by increasing the albedo of the ice? 
> How can you check? 
> 
> Some variables to look at: 
>
> albedo to convince yourself this worked as expected
>
> ice fraction to see how changing albedo impacted the ice
{: .challenge}


### Increase the zonal wind stress in the ocean case

Create a clone of your control simulation
Run `case.setup`

This exercise demonstrates how to change the source code (i.e. Fortran) of the model.  The original source code resides in:

`/glade/work/USERNAME/cesm2.1.3/`

Similar to namelists, we don't change the original, we make changes in an alternate location, then modify it to override the default.  Changes to source code go in `SourceMods/src.xxx`

Also, source code changes must be included in the build, so we make the changes before building the model.

Copy the file `/glade/work/USERNAME/cesm2.1.3/components/pop/source/forcing_coupled.F90` to `CASEDIR/SourceMods/src.pop/`

~~~
cp /glade/work/USERNAME/cesm2.1.3/components/pop/source/forcing_coupled.F90 ~/cases/gwindstress/SourceModes/src.pop/
~~~
{: .language-bash}

Now let's look at the code. 

Find the subroutine `rotate_wind_stress`

This code rotates the wind stress vector to map it onto the correct location in the model local grid coordinates.  We can think of the first component (array index 1) as the zonal direction (on the model grid) and the second component (array index 2) as the meridional component on the model local grid coordinates.  
Add a line of code to increase the first component by 25%.

Build and run the model for 1-year.

Be sure to check that the model built successfully before you run it.  If you had a typo, you will likely get a build error and your model will not run because it did not build. 

> ## Where do you think there will be the biggest impacts of increased zonal wind stress?
>
>  How can you check the impact?
> 
>  What variables would you look at?
>
{: .challenge}


## Looking at our output

Once you have submitted your model runs, they will run relatively quickly if the queue is not too busy. In the meantime, you can look at the output of my model runs to explore the results of these experiments. 

Experiment 1 (control): `/glade/scratch/cstan/archive/gcontrol/ocn/hist/`

Experiment 2 (overflow): `/glade/scratch/cstan/archive/goverflow/ocn/hist/`

Experiment 3 (snow albedo): `/glade/scratch/cstan/archive/gsnowalb/ice/hist`

Experiment 4 (wind stress): `/glade/scratch/cstan/archive/gwindstress/ocn/hist`


How would we read this data?

Login to casper and launch Jupyter

~~~
module load python/3.6.8
ncar_pylib 
start-jupyter
~~~
{: .language-bash}

follow the screen for the rest of the steps.

~~~
import xarray as xr

path='/glade/scratch/kpegion/archive/gcontrol/ocn/hist/'
files='gcontrol.pop.h.0001-*.nc'
ds=xr.open_mfdataset(path+files,combine='nested',
                    concat_dim='time')
ds
~~~
{: .language-python}

We can look at annual mean values by taking the mean in time.

~~~
ds_amean=ds.mean(dim='time')
~~~
{: .language-python}

