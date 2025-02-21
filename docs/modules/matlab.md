# Matlab

![image](../images/Matlab.jpg)

MATLAB is a high-performance language for technical computing. It integrates computation, visualization, and programming in an easy-to-use interactive environment where problems and solutions are expressed in familiar mathematical notation. This high-level language and interactive environment enables you to perform computationally intensive tasks faster than with traditional programming languages such as C, C++, and Fortran.

------------------------------------------------------------------------

## Toolboxes

The Matlab Toolboxes licensed by ND changes each year based on user demand (measured by an annual survey). Faculty may also purchase toolbox licenses which were not centrally funded based on broad user demand. To see what toolboxes are currently available simply type **ver** in the Matlab Command Window.

------------------------------------------------------------------------

## Submitting Single Core Jobs

To submit a single core MATLAB job to CRC systems use the following template:

``` shell
#!/bin/bash
#$ -q long
#$ -pe smp 1

module load matlab

matlab -singleCompThread -nodisplay -nosplash < your_file.m
```

By default, recent versions of MATLAB will try to automatically multithread processing on all the cores available on a machine. If you are only requesting one core in your submission script, you need to invoke MATLAB with the `-singleCompThread` option (to avoid interfering with other user's running jobs):

``` shell
matlab -singleCompThread ...
```

> [!WARNING]
> If you request more than 1 core, maxNumCompThreads function should be used to control the maximum number of computational threads.

For example, adding the following line in your Matlab code will limit the number of threads to the same number as that you entered for the `-pe smp` flag in your job script:

``` shell
maxNumCompThreads(str2num(getenv("NSLOTS")));
```

------------------------------------------------------------------------

## Submitting Multicore Jobs

To submit multicore MATLAB jobs (up to 24 cores with current CRC systems) using the PCT (Parallel Computing Toolbox), use the following template. Note that there are normally multiple versions of Matlab available, see the `modules` page for more information on how software is organized within the CRC.

``` shell
#!/bin/bash
#$ -q long
#$ -pe smp 12

export MATLABPATH=~/path/to/matlab_file

cd ~/path/to/matlab_file

module load matlab

matlab -nodisplay -nosplash < matlab_file
```

The matlab file above includes the command `parpool('local',12)`, allowing Matlab to use the 12 cores on the machine.

Parallelization can also be accomplished using a parfor within your matlab file:

``` shell
parpool('local', 12);
parfor i=1:N
end
% In Matlab 8.5 need to use "delete(gcp('nocreate'))"
```

------------------------------------------------------------------------

## MATLAB and Array Jobs

An example for combining MATLAB and job arrays is as follows:

``` shell
#!/bin/csh
#$ -N testarray
#$ -t 1-4:1
#$ -M afs_id@nd.edu
#$ -m ae

setenv MATLABPATH directory_path_to_your_files.m:other_user_contrib_directory_path

matlab -nodisplay -nosplash -nojvm -r "myFunction(${SGE_TASK_ID});exit"
```

The myfunction.m will receive respectively for each task 1,2,3 and 4 as an input parameter.

------------------------------------------------------------------------

## Memory Profiling

User can use the builtin MATLAB profiler to understand memory usage in your scripts. To enable memory profiling add the following line to your scripts:

``` shell
profile -memory on;
```

------------------------------------------------------------------------

## License Information

- Matlab Parallel Computing Toolbox can be currently configured up to 64 cores (SMP) per simulation.
- We currently do not support a license for MATLAB Distributed Computing

------------------------------------------------------------------------

## Tutorials and Training

- For more information, please visit the official Matlab training [page](https://www.mathworks.com/academia/student_center/tutorials/launchpad.html?s_cid=0210_desp_na_224077) for a wealth of video tutorials and training content.

------------------------------------------------------------------------

## Known issue with *parpool* and *matlabpool*

There is a known issue when submitting multiple jobs that use either of the Matlab pool commands. Basically, if more than one task is started close to another task, they may both try and write to the same temporary file and cause problems. One solution involves changing the name of the default file Matlab writes pool information to as described in this link:

[Matlab Pool Solution](https://www.mathworks.com/matlabcentral/answers/97141-why-am-i-unable-to-start-a-local-matlabpool-from-multiple-matlab-sessions-that-use-a-shared-preferen).

------------------------------------------------------------------------

## Dynare Support

Looking to use Matlab with `dynare`? Simply load the `dynare` module, this will automatically bring in all dependencies. You can check which versions of dynare are available with:

    module avail dynare

## Further Information

See the official site: [MATLAB](https://www.mathworks.com/products/matlab/)

Default help files for version "X.Y" (replace X and Y with desired version) may be found at:

``` shell
/afs/crc.nd.edu/x86_64_linux/m/matlab/X.Y/help
```
