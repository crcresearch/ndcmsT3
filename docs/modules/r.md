# R

![image](../images/RLogo.png)

R is a language and environment for statistical computing and graphics. It is a GNU project which is similar to the S language and environment which was developed at Bell Laboratories (formerly AT&T, now Lucent Technologies) by John Chambers and colleagues. R can be considered as a different implementation of S. There are some important differences, but much code written for S runs unaltered under R.

R provides a wide variety of statistical (linear and nonlinear modelling, classical statistical tests, time-series analysis, classification, clustering, ...) and graphical techniques, and is highly extensible. The S language is often the vehicle of choice for research in statistical methodology, and R provides an Open Source route to participation in that activity.

------------------------------------------------------------------------

## Sample R Job

``` shell
#!/bin/bash
#$ -M afs_id@nd.edu
#$ -m abe

module load R
R CMD BATCH  your_input_R_file.r  your_output_R_file.out
```

------------------------------------------------------------------------

## Installing Local Packages

Due to the wide range of packages available for **R**, we are unable to install every one. Fortunately, it is easy for users to install additional libraries. To begin with, load the **R** module. If the default version is sufficient, this can be done with the command:

``` shell
module load R
```

For convenience, although not necessary, we suggest making a central directory to hold this and any future packages:

``` shell
mkdir ~/myRlibs
```

Next, there are two different ways of installing R packages:

------------------------------------------------------------------------

### Installing packages within R

Open an R shell and execute the following command:

``` shell
install.packages("package_name", lib="install_location", repos="mirror_location")
library('package_name', lib.loc='install_location')
```

For our example, this would be:

``` shell
install.packages("bizdays", lib="~/myRlibs",repos='https://cran.us.r-project.org')
library('bizdays', lib='~/myRlibs')
```

To avoid having to specify the installation location every time you use this library, you can create an `.Renviron` file in your home directory using any text editor. Then, add the following line to it:

``` shell
R_LIBS=install_location
```

For our example, this would be:

``` shell
R_LIBS=~/myRlibs
```

Now, we can simply do:

``` shell
install.packages("bizdays")
library(bizdays)
```

------------------------------------------------------------------------

### Installing R packages from source code

You will need to obtain the source code for the package you want to install. The most common repository of these are at [The Comprehensive R Archive Network (CRAN)](https://cran.r-project.org/). A simple method to get the package to the CRC is to copy the location of the file, usually through a right click sub-menu, and then use the ''wget'' command:

``` shell
wget https://cran.r-project.org/src/contrib/bizdays_1.0.1.tar.gz
```

Once we have the package, we will need to decide where to install it.

Now, issue the following command to install the package:

``` shell
R CMD INSTALL -l install_location package_name
```

For our example, this would be:

``` shell
R CMD INSTALL -l ~/myRlibs bizdays_1.0.1.tar.gz
```

The last step is to tell **R** the location of our new installation. In a CSH environment, this is:

``` shell
setenv R_LIBS install_location
```

If you are using BASH, it would be:

``` shell
export R_LIBS=install_location
```

Add this command to your .cshrc or .bashrc file, respectively, to permanently set it.

------------------------------------------------------------------------

## Profiling R Code

Profiling R code can help determine which sections in the R code need to be optimized for better performance. In order to profile the R code, one needs to use the **Rprof()** function. **Rprof()** records how many seconds have been spent on each function of the R code. The functions that get timed are the ones that get executed after the **Rprof()** function gets declared. Any function before the **Rprof()** declaration will not be timed. One needs to pass a parameter to Rprof. The parameter is the name of the file that will contain the results.

If only a section of the R code needs to be profiled, one can use the **Rprof()** to specify when to start profiling the functions and when to stop profiling the functions. To start profiling the functions, one should place **Rprof("file_name")** before the functions that need to be profiled get executed. In order to stop profiling the rest of the R Code, one needs to place **Rprof(NULL)** to stop profiling the rest of the R Code that does not need to be profiled. The following is an example on how **RProf()** is used in an actual R script.

``` shell
# load sources
dyn.load("readbfile3_crc.so")
source("readbfile.r")
source("snpsel24_data.r")

Rprof("test1b.out")             #Begin profiling functions

# try to read in data
dat.M <- read.bfile("hapmap_sim_chr1_test.bed")

# try to run snpsel
selmat.M <- snp_sel(dat.M,k=300,b=10,t=.1)

Rprof(NULL)                    #Stop profiling functions

# write selmat for reference
write.table(selmat.M,file="test_selmat_v1.txt",quote=F,sep=" ",col.names=F,row.names=F)
```

In the example above, the functions read.bfile() and snp_sel() as well a the functions within these functions will be profiled. The function write.table() will not be profiled by **Rprof()**.

------------------------------------------------------------------------

## Parallel Computing in R

R itself does not provide parallel execution. Therefore, in order to realize parallel computing in R, an appropriate parallel R package should be invoked.

## Test for Rmpi

Here is an Rmpi test file:

``` shell
# Load the Rmpi pacakge:
library(Rmpi)

# Spawn N-1 workers ==> Don't need this on UGE so that commented out, by ISS on 04012019
# mpi.spawn.Rslaves(nslaves=mpi.universe.size()-1)

# The command we want to run on all the nodes/processors we have
mpi.remote.exec(paste("I am ", mpi.comm.rank(), " of ", mpi.comm.size(), " on ", Sys.info() [c("nodename")]))

# Tell all slaves to close down, and exit the program
mpi.close.Rslaves()
mpi.quit()
```

and save this with "Rmpi-test-on-CRC.R". A job script file for this Rmpi parallel test on the CRC Grid Engine:

``` shell
#!/bin/tcsh
#
#$ -M Your_NetID@nd.edu
#$ -m abe
#
#$ -pe mpi-24 48
#
# Specify a queue name, for example,
#$ -q debug
#

module load R/4.2.0

mpirun -np ${NSLOTS} Rscript Rmpi-test-on-CRC.R > Rmpi-test-on-CRC.out
```

The R/4.2.0 version in the CRC R modules supports "foreach", "parallel", "doParallel", "snow", "snowfall",... parallel packages as a default. For a single node SMP parallel, you can easily download/install on your own space.

For example, you can invoke the library with:

``` shell
>library(parallel)
```

in your R script and then can specify a number of core you want. Typically, we set the `cores` variable to the same value as that we set in our `#$ -pe smp` flag via an environment variable, `NSLOTS`. For example,

``` shell
>options(cores = Sys.getenv("NSLOTS"))
>getOption('cores')
```

Here is a typical example to compare single-core and multi-core parallel computing in R:

``` shell
module load R

R

> library(parallel)
> detectCores()
[1] 24
> options(cores = 24)
> getOption('cores')
[1] 24

> test <- lapply(1:10,function(x) rnorm(100000))

> system.time(x <- lapply(test,function(x) loess.smooth(x,x)))      <<<== single-core running

> system.time(x <- mclapply(test,function(x) loess.smooth(x,x)))    <<<== multi-core (24-core) running
```

------------------------------------------------------------------------

## Related Software

For a GUI IDE for R, see `rstudio`.

------------------------------------------------------------------------

## Further Information

See the official website: [R](https://cran.r-project.org/)
