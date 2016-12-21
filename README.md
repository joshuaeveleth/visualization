
## Kameleon Vis Tools

This repo tracks the development of tools used to visualize space weather models hosted by the Community Coordinated Modeling Center (CCMC). 

Many space weather researchers have their own visualization pipelines, but often these are not designed for or available to the larger community of space scientists. Moreover, the current paradigms for studying magnetic reconnection require millions of field line traces, which is untennable for many researchers. Instead of maximizing the number of traces performed, we attempt to minimize the traces required using a farthest point seeding method. With a descrete geometric definition of separators, we can readily extract topological structures. Combined with tools like plotly, we can readily visualize the result in the browser, together with cut planes and isosurfaces of other variables of interest.

## Getting started (Mac osx)

First obtain miniconda from here http://conda.pydata.org/miniconda.html

When running the miniconda installer, it'll give the option of prepending miniconda to your path so that all its utilities are available from command line. It's ok if you prefer not to - you'll just have to invoke each conda command using  `/full/path/to/miniconda/bin/command`. 

After installation, run
```console
conda create --name klive --file klive.txt
```
where `klive.txt` for mac is included in this distribution. (check back in a while for a linux and windows version)
This should create an exact replica of the kameleon live environment for mac. It will include its own python linked to kameleon libraries compatible for mac, which are useful for interpolating results from certain models hosted at the ccmc.

Activate the environment with
```
source activate klive
```
(Here activate is available in the miniconda bin directory.) To deactivate, run 

```
source deactivate
```

Working inside the klive environment is a convenience that allows your scripts to load any module installed to that environment. The environment includes pip, so you can update it with packages in the normal way. 

## Running farthest_point.py

After activating the klive environment, invoke python on the farthest_point.py script:

```console
(klive)$ python farthest_point.py Alexa_Halford_062105_2.3df.00720.cdf -vars bx by bz rho -iso_var rho -iso_opacity .5 -sep_iter 1 -init_flines fieldlines.csv
```
Where the sample file `Alexa_Halford_062105_2.3df.00720.cdf` is included in this distribution. -vars specify which variables to plot on the slices (see below for more options). The isosurface value is for density and the value is the mean by default. 

The analysis traces a single fieldline using the point farthest from all the fieldlines stored in a previous invocation (where `fieldlines.csv` was used as the -save_flines argument).

The above command should open temp-plot.html in the browser, which should look something like this:

![screen shot](https://github.com/ccmc/visualization/raw/master/screenshot.png "screenshot")


```console

	usage: farthest_point.py [-h] [-v] [-db DEBUG] [-x xmin xmax] [-y ymin ymax]
	                         [-z zmin zmax] [-r rmin rmax] [-res nx [ny ...]]
	                         [-vars var1 [var2 ...]]
	                         [-slices islice jslice kslice]
	                         [-iso_var ISOSURFACE_VARIABLE]
	                         [-iso_val ISOSURFACE_VALUE]
	                         [-iso_opacity ISOSURFACE_OPACITY]
	                         [-init_seeds INITIAL_SEEDS]
	                         [-init_flines INITIAL_FIELDLINES]
	                         [-save_flines SAVE_FIELDLINES]
	                         [-sep_iter SEPARATOR_ITERATIONS]
	                         full/path/to/input_file.cdf

	Interpolates variables onto grid.

	positional arguments:
	  full/path/to/input_file.cdf
	                        kameleon-compatible file

	optional arguments:
	  -h, --help            show this help message and exit
	  -v, --verbose         verbosity of output
	  -db DEBUG, --debug DEBUG
	                        debugging flag

	grid options:
	  interpolation options for a grid of points

	  -x xmin xmax, --x-range xmin xmax
	                        range of x
	  -y ymin ymax, --y-range ymin ymax
	                        range of y
	  -z zmin zmax, --z-range zmin zmax
	                        range of z
	  -r rmin rmax, --r-range rmin rmax
	                        range of r, default is 0 to inf
	  -res nx [ny ...], --resolution nx [ny ...]
	                        resolution of the grid along each axis
	  -vars var1 [var2 ...], --variables var1 [var2 ...]
	                        list of variables to be interpolated
	  -slices islice jslice kslice
	                        list of slices to take in each dimension

	isosurface options:
	  options for generating isosurface

	  -iso_var ISOSURFACE_VARIABLE, --isosurface_variable ISOSURFACE_VARIABLE
	                        variable to generate isosurface from
	  -iso_val ISOSURFACE_VALUE, --isosurface_value ISOSURFACE_VALUE
	                        isosurface value to generate
	  -iso_opacity ISOSURFACE_OPACITY, --isosurface_opacity ISOSURFACE_OPACITY
	                        opacity of the isosurface

	topology options:
	  options for finding separator surfaces

	  -init_seeds INITIAL_SEEDS, --initial_seeds INITIAL_SEEDS
	                        csv file containing initial seed points - should be
	                        far from separators
	  -init_flines INITIAL_FIELDLINES, --initial_fieldlines INITIAL_FIELDLINES
	                        fieldline file containing output from previous run
	  -save_flines SAVE_FIELDLINES, --save_fieldlines SAVE_FIELDLINES
	                        output file name to store results
	  -sep_iter SEPARATOR_ITERATIONS, --separator_iterations SEPARATOR_ITERATIONS
```

## A work in progress

We are currently updating the 3D interface to allow for more interactivity. There are several bugs to work out, and speed improvements are ongoing.

## A note on Kameleon dependence

While these tools are designed to support models hosted by the ccmc, the algorithms themselves do not depend on them. So, if you have your own python interpolator/field line tracer, it should be relatively easy to plug it in here (provided you can represent your fieldlines as a pandas dataframe). Look for updates that make this easier.

