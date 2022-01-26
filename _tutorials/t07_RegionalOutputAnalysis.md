---
layout: tutorial
title:  Regional output analysis
shortID: output
lastUpdated:   2022-01-13
model: MAgPIE
modelVersion: 4.4.0
author:
  - iw
  - fb
level: 3
requirements:
  - Local copy of the MAgPIE model (<https://github.com/magpiemodel/magpie>)
  - Have R installed (<https://www.r-project.org/>)
  - (For validation.pdf creation:) Have Latex installed (e.g., MiKTeX <https://miktex.org/howto/install-miktex>)
  - Have completed a MAgPIE run OR downloaded existing MAgPIE runs (<https://zenodo.org/record/2572620#.X8Zr9RbPw2w>)
lessonsContent:
  - Use model-internal R-scripts for output analysis.
  - Know where to find the automated validation PDF and how it is
    structured.
  - Use the evaluation tool `appResultsLocal` of the library
    `shinyresults`.
  - Use the `magpie4` package for output analysis.
  - Analyse outputs with the `gdx` package.
exercises:
  - task: "Execute the model-internal output script `rds report` via the command
          window. This script collects the results of several report-functions -
          that calculate many key output variables like Production, Land Use or
          Yields - and writes them into one rds file."
    solution: "1. open a command window in the main MAgPIE model folder\n
               2. select model run by typing in number and press `ENTER`\n
               3. select `rds report` by pressing `2` and `ENTER`\n
               4. select `Direct execution` by pressing `3` and `ENTER`\n"
  - task: "Open a validation pdf (either in a folder containing your own simulation
           results or the downloaded MAgPIE simulation runs and\n
           (a) make yourself familiar with the structure of the document and the
           hierarchy of outputs as displayed by the table of contents and; \n
           (b) have a look at some figures displaying model outputs of your
           interest."
published: true
---


## Introduction

After having successfully started and accomplished a simulation run, the
next step is to evaluate the simulation results. In case you have not
yet conducted an own MAgPIE simulation or your simulation is still
running, you can download model runs produced for Mishra et al. (2021)
(<https://doi.org/10.5194/gmd-14-6467-2021>).
These runs were created with version 4.3.5 of the MAgPIE model
(<https://github.com/magpiemodel/magpie/tree/master>).
You can download these runs as tgz folder from Zenodo (**magpie_v4.3.5_gmd-2021-76.tgz**
at <https://zenodo.org/record/5417474#.YeAf8_DMJaQ>)
and copy the folders containing the simulation
results into the **output** folder of your local version of the MAgPIE
model (Note: if you freshly cloned the model and did not set up a run yet, you
will have to create an empty output folder).

There are several ways to assess and evaluate MAgPIE results. This
tutorial gives an overview on different tools and options that can be
used to analyse model outputs.

For each simulation, results are written to a folder that is created
automatically as a combination of the **model title** and other
information (as defined in the default.cfg via the command
cfg$results\_folder) inside the **output** folder of the model.

## Model-internal R-scripts for output analysis{#Scripts}

# Model-internal R-scripts in the config file
In the file “config/default.cfg”, it is possible to indicate which
R scripts are executed for output analysis after a model run is
finished. These R scripts can be found in scripts/output.
In the default MAgPIE configuration, the scripts *output\_check*
*rds\_report* (to be used in appResultsLocal; see explanations below),
*validation\_short* and *extra/disaggregation* are selected via cfg$output:

``` r
cfg$output <- c("output_check", "rds_report", "validation_short",
                "extra/disaggregation")
```
# Manual exection of model-internal R-scripts
Output scripts can also be executed via the command window.
To do so, windows users can open a command
line prompt in the MAgPIE model folder by using **shift** + **rightClick** and
then selecting **open command window here** option.

In the command prompt, use the following command:

``` r
Rscript output.R
```

You are now asked to choose the simulation run for which you would like to
execute an output script.

![Selection of model runs](../assets/img/Rscript_output_runselection.PNG)

Next, you can choose which output scripts you want to execute:

![Selection of model-internal output scripts](../assets/img/Rscript_output_scriptselection.PNG)

The last step is to select the run submission type, e.g. “Direct execution”:

![Selection of run submission type](../assets/img/Rscript_output_submissiontype.PNG)

Now, the selected script is executed. After completion, the results are
written in the respective folder of the simulation run inside the
**output** folder of the model.

## Automated model validation

The automated model validation is an example of output
analysis based on model-internal scripts (see \@ref(Scripts)). If the
validation script is executed (either by selection via `cfg$output` as
explained [above](#model-internal-r-scripts-in-the-config-file) or
by execution via the command window as explained in \@ref(ScriptsManual)),
a standard evaluation PDF is created that validates numerous
model outputs with a validation database containing historical data and
projections for most outputs returned by the model, either visually or
via statistical tests. A standard evaluation PDF consists of hundreds of
evaluation outputs and usually has a length of around 1800 pages. By
evaluating the model outputs on such a broad level rather than focusing
only on key outputs, it allows getting a more complete picture of the
corresponding simulation. As an example of such validation files, you
can download the evaluation documents produced for all runs shown in the
MAgPIE 4 framework paper (<https://doi.org/10.5281/zenodo.1485303>)
or the runs downloaded for this exercise (<https://zenodo.org/record/5417474#.YeAf8_DMJaQ>).

The table of contents of the validation PDF gives a good overview over
the model outputs that can be simulated with a MAgPIE standard
simulation, even though the validation PDF only shows a subset of
possible model
outputs:

![Table of contents of the validation pdf](../assets/img/toc_validationpdf.PNG)

## Interactive scenario analysis

The automated model validation PDF is a good tool for visually
evaluating a broad range of model outputs. However, comparison between
model runs, i.e. between different scenarios, is rather difficult and
inconvenient with the different model results being scattered across
different large PDF files.

To overcome this issue, we developed the interactive scenario analysis
and evaluation tools appResultsLocal (and appResults for the use within
the PIK network) as part of the library **shinyresults**
(<https://github.com/pik-piam/shinyresults>), which show evaluation
plots for multiple scenarios including historical data and other
projections based on an interactive selection of regions and variables.
You can use this tool by running the following R command in the main
folder of your model, which will automatically collect all runs in the
output folder and visualize them:

``` r
shinyresults::appResultsLocal()
```

This command opens an interactive window, where you can select the
simulations that you want to
evaluate.

![Interactive MAgPIE app](../assets/img/appResults_window.png)

You can use filters to select a subset of all runs stored in the output
folder of the model, for example by searching for runs that have been
finished at a certain day, have been created by a certain user
or by searching for keywords in the title of the simulation runs:

![Run selection by using a filter](../assets/img/appResults_runselection.PNG)

### Exercise

Choose *title* as filter and select 2 simulations that are stored in the
output folder of your model, e.g. the SSP2 and SSP5 simulations from the
downloaded set of MAgPIE
runs:

![How to use the title for filtering runs](../assets/img/appResults_runselection_title.png)

After having selected the subset of runs that you want to analyse, click
the button *Load selection*. Now, you can click on the tab *LinePLot*.
You will then see on the right hand side line plots showing the
development of population for historical and future time steps for all
model regions and on the global
scale:

![Regional and global development of population](../assets/img/appResults_LinePlot.png)

Now, choose a variable of your interest, either by scrolling through the
drop-down menu or write a key word in the input field, e.g. “cropland”,
to reduce the options in the
menu.

![Selection of variables for line plots](../assets/img/appResults_LinePlot_variables.png)

Make yourself familiar with the features of the app\! You can, for
example, select a subset of regions or a subset of time steps for which
the results should be plotted. Moreover, you can free the y-axis,
include or exclude validation data (if available) and download the plot.

## Analysis of outputs with the magpie4 library

If you want to go beyond visual output analysis and predefined output
evaluation facilitated by scripts in the model folders
**scripts/output/single** and **scripts/output/comparison**, you can use
the functionality of the R package *magpie4*
(<https://github.com/pik-piam/magpie4>). This library contains a list of
common functions for extracting outputs from the MAgPIE model which are
also the basis for the generation of the automated validation PDF. For a
quick overview on the functions which are included in the library, you
can have a look in the folder **magpie4/R**. The following figure shows
a subset of R-files included in
**magpie4/R**:

![Subset of R functions of the magpie4 library](../assets/img/subset_functions_magpie4.png)

For making yourself familiar with this library, you can open a R/RStudio
session and set the MAgPIE model folder as working directory. This can
be done by using the following command:

``` r
setwd("/path/to/your/magpie/model/folder")
```

Then, load the library and call the help pages:

``` r
library(magpie4)
?magpie4
```

You can click on the index and search for interesting functions, e.g.
**carbonstock**, and read the respective help
page:

![Help page of the carbonstock-function of the magpie4 library](../assets/img/magpie4_help_carbonstock.png)

### Exercise

Apply the function *carbonstock* for calculations in R, by using the
file “fulldata.gdx” in the folder of a model simulation run as input for
the function.

1.  Use the default settings of the arguments of the function
2.  Change the arguments of the function, e.g. set the level from “cell”
    to “reg”.

## Analysis of outputs with the gdx library

The **gdx library** (<https://github.com/pik-piam/gdx>) allows for
directly accessing objects contained in the fulldata.gdx via the
function **readGDX**. A pragmatic way to learn how to use this function
for the extraction of interesting information from the fulldata.gdx is
to open R-files of the magpie4 library within Rstudio. Most of the
magpie4 functions make use of **readGDX**.

In the function *carbonstock* of the **magpie4** library, we see several
instances where **readGDX** is used,
e.g.:

``` r
a <- readGDX(gdx,"ov_carbon_stock",select=list(type="level"),react="silent")
sm_cc_carbon <- readGDX(gdx,"sm_cc_carbon2",react = "silent")
ov_land <- readGDX(gdx,"ov_land",select = list(type="level"))
fm_carbon_density <- readGDX(gdx,"fm_carbon_density")[,t,]
```

It is possible to extract various GAMS objects like *“sets”*,
*“equations”*, *“parameters”*, *“variables”* and *“aliases”* with
**readGDX**.

With the argument *select=list(type=“level”)*, you can select the levels
of endogenous variables, with *select=list(type=“marginal”)* you can
extract the marginal values of these variables.