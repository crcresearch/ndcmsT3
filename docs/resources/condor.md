# HTCondor

The University of Notre Dame has a Condor pool which harnesses the idle computational cycles from workstations and servers all over campus to run batch jobs. Condor provides access to over 20,000 cores. The Condor pool is maintained by the Notre Dame Cooperative Computing Lab (CCL) and CRC; most of the information in this article is taken directly from the [CCL website](https://ccl.cse.nd.edu/).

## Visual Status of the ND Condor Pool

[ND Condor Utilization Matrix](http://condor.cse.nd.edu/condor_matrix.cgi)

------------------------------------------------------------------------

## Online Short Course

[Short Course In Data Centric Computing](https://www3.nd.edu/~dthain/courses/disc/)

------------------------------------------------------------------------

## About Condor

Condor is a distributed batch computing system used at many institutions around the world. Condor allows many users to share their computing resources, thus giving everyone access to more computing power than any one person could afford to buy. In particular, Condor is good at harnessing idle cycles from under-used servers.

Users that submit jobs to Condor can specify their own requirements on what machines to use. You can require the use of one particular machine, or any from a particular cluster, or any with a certain amount of memory or processor type. Of course, if you are borrowing machines owned by other people, then you must accept the possibility that some jobs may be kicked off and must run elsewhere.

For more information about Condor, please visit the Condor project page at:

- <https://research.cs.wisc.edu/htcondor/>.

------------------------------------------------------------------------

## Getting Started With Condor at Notre Dame

Log into `condorfe.crc.nd.edu` to submit jobs.

To submit a batch job to Condor, you must create a submission file and then run the `condor_submit` command. Try creating this sample submit file in `/tmp/YOURNAME/test.submit`.

``` shell
universe = vanilla
executable = /bin/echo
arguments = hello condor
output = test.output
should_transfer_files = yes
when_to_transfer_output = on_exit
log = test.logfile
queue
```

Now, to submit the job to Condor, execute:

``` shell
cd /tmp/YOURNAME
condor_submit test.submit
Submitting job(s)...
1 job(s) submitted to cluster 2.
```

Once the job is submitted, you can use `condor_q` to look at the status of the jobs in your queue. If you run `condor_q` quickly enough, you will see your job idle:

``` shell
-- Submitter: hedwig.cse.nd.edu : <129.74.154.241:33593> : hedwig.cse.nd.edu
 ID      OWNER            SUBMITTED     RUN_TIME ST PRI SIZE CMD
   2.0   dthain          8/26 17:21   0+00:00:00 I  0   0.0  echo hello world
```

If you job remains in state I 'idle' for a significant amount of time you should check to see why it has not started. There is most likely a problem matching your request with an available resource which will be shown with the following command.

``` shell
condor_q  yourjobnumberhere  -better-analyze
```

The command below shows the relative priority of everyone who has recently run jobs. Those with lower priority numbers have their waiting jobs start before others.

``` shell
condor_userprio
```

If you decide to cancel a job, use `condor_rm` and the job id:

``` shell
condor_rm 2.0
Job 2.0 marked for removal.
```

> [!NOTE]
> Despite what the Condor manual says, you will NOT receive email when a job is complete. This feature has been disabled at Notre Dame due to our email security configuration.

------------------------------------------------------------------------

## Important note about AFS

In the example above, the submit file and all of the job's details were stored in `/tmp/YOURNAME` on your local disk. Condor simply moved the necessary files back and forth in order to run your jobs. If instead you store your data files in AFS (i.e. your home directory), Condor cannot access them because it will not have your AFS token or Kerberos ticket.

If you want Condor to be able to **read** any data out of AFS, you must change the ACLs on the necessary directories to allow the security group **nd_campus to read** the data. Note that this will allow anyone with a valid account to read the data within the directory.

Here's how:

``` shell
fs setacl ~/my/data/directory nd_campus read
```

If you want Condor to be able to **write** to AFS, you must change the ACLs to the following:

``` shell
fs setacl ~/my/data/directory nd_campus rlidw
```

Because AFS permissions are hierarchical, in both of these examples nd_campus must have at least lookup 'l' permission in the destination directory's parent directories. For example,

``` shell
fs setacl ~/my nd_campus l
```

``` shell
fs setacl ~/my/data nd_campus l
```

If there are several directories, the `find` command may be used to set ACLs recursively:

``` shell
find ~/my -type d -exec fs setacl {} nd_campus read \; 
```

------------------------------------------------------------------------

## Submitting Large Numbers of Jobs

Because you will certainly want to run many jobs at once via Condor, you can easily modify your submit file to run a program with tens or hundreds of variations. Change the `queue` command to queue several jobs at once, and the `$(PROCESS)` macro to modify the parameters with the job number.

``` shell
universe = vanilla
executable = /bin/echo
arguments = hello $(PROCESS)
output = test.output.$(PROCESS)
error = test.error.$(PROCESS)
should_transfer_files = yes
when_to_transfer_output = on_exit
log = test.logfile
queue 10
```

Now, when you run condor_submit, you should see something like this:

``` shell
condor_submit test.submit
Submitting job(s)..........
10 job(s) submitted to cluster 9.
```

Note in this case that "cluster" means "a bunch of jobs", where each job is named 9.0, 9.1, 9.2, and so forth. In this next example, `condor_q` shows that cluster 9 is halfway complete, with job 9.5 currently running.

``` shell
condor_q

-- Submitter: hedwig.cse.nd.edu : <129.74.154.241:33593> : hedwig.cse.nd.edu

    ID      OWNER            SUBMITTED     RUN_TIME ST PRI SIZE CMD
   9.5   dthain          8/26 17:46   0+00:00:01 R  0   0.0  echo hello 5
   9.6   dthain          8/26 17:46   0+00:00:00 I  0   0.0  echo hello 6
   9.7   dthain          8/26 17:46   0+00:00:00 I  0   0.0  echo hello 7
   9.8   dthain          8/26 17:46   0+00:00:00 I  0   0.0  echo hello 8
   9.9   dthain          8/26 17:46   0+00:00:00 I  0   0.0  echo hello 9
```

------------------------------------------------------------------------

## To Submit Multicore SMP jobs to the ND Condor Pool

The condor pool is configured to enable dynamic slots. Therefore, if you want to submit multi-threaded or multicore SMP Condor jobs (e.g. using OpenMP), you can add below lines in your Condor job script:

``` shell
...
environment = OMP_NUM_THREADS=4;LD_LIBRARY_PATH=${LD_LIBRARY_PATH}
request_cpus = 4
...
```

For example, if you compile below hellow_world.c:

``` shell
#include <omp.h>
#include <stdio.h>
#include <stdlib.h>
int main (int argc, char *argv[]) {

int nthreads, tid;

/* Fork a team of threads giving them their own copies of variables */
#pragma omp parallel private(nthreads, tid)

     {

     /* Obtain thread number */
     tid = omp_get_thread_num();
     printf("Hello World from thread = %d\n", tid);

     /* Only master thread does this */
     if (tid == 0)
     {
      nthreads = omp_get_num_threads();
     printf("Number of threads = %d\n", nthreads);
     }

     } /* All threads join master thread and disband */

}
```

with

``` shell
icc -openmp hello_world.c -o hello_world.x
```

and then submit your Condor job script with

``` shell
executable = hello_world.x
```

and above options, then you will get an output, for example, like:

``` shell
Hello World from thread =            0
Hello World from thread =            1
Hello World from thread =            3
Hello World from thread =            2
Number of threads =                  4
```

------------------------------------------------------------------------

## Example of Submitting 10 Matlab jobs

Following is a Condor submission script template for running Matlab with 4 cores:

``` shell
universe = vanilla
getenv = true
executable = Path_to_your_executable_file/executable.sh
output = test.output
error = test.error
request_cpus = 4
should_transfer_files = yes
when_to_transfer_output = on_exit
log = logfile
queue 10
```

with 'executable.sh':

``` shell
#!/bin/bash

if [ -r /opt/crc/Modules/current/init/bash ]; then
    source /opt/crc/Modules/current/init/bash
fi

module load matlab

# If needed, specify the location of additional .m files.  Note that Condor must be able to read this directory.
export MATLABPATH=/path/to/your/matlab/files

#  Substitute your ID below to ensure Matlab can write configuration files to a unique directory:
export MATLAB_PREFDIR=/tmp/AFS_ID
export TMPDIR=/tmp/AFS_ID

# Determine the full path to a matlab version
export MLAB=`which matlab`

${MLAB} -nodisplay -nosplash -nojvm -r "myFunction('$1');exit"
```

------------------------------------------------------------------------

## Example of Submitting an Amber job to a GPU node

Following is a Condor submission script template for running Amber with 1 GPU:

``` shell
universe = vanilla
getenv = true
executable = Path_to_your_executable_file/executable.sh
output = test.output
error = test.error
request_gpus   = 1
should_transfer_files = yes
when_to_transfer_output = on_exit
log = logfile
queue 1
```

with 'executable.sh':

``` shell
#!/bin/bash

if [ -r /opt/crc/Modules/current/init/bash ]; then
    source /opt/crc/Modules/current/init/bash
fi

cd  Path_to_your_executable_file

module load amber
pmemd.cuda -O -i mdin -o mdout -p prmtop -c inpcrd
```

------------------------------------------------------------------------

## Condor Status

You can view the total number of CPUs currently in the Condor pool [here](http://condor.cse.nd.edu/condor_totals.cgi) or you can view a list of all the machines in the pool [here](http://condor.cse.nd.edu/condor_machines.cgi).

In addition, you can view information about users with jobs currently in the queue [here](http://condor.cse.nd.edu/condor_queues.cgi), and you can view a list of all the machines currently running jobs [here](http://condor.cse.nd.edu/condor_running.cgi).

For a full visual status of the Condor pool, please see the Condor Visual Status Page, located [here](https://ccl.cse.nd.edu/viz/) (requires Java).

More information about the status of the Condor pool, including historical data, can be found at the CCL web page, located [here](http://condor.cse.nd.edu/).

------------------------------------------------------------------------

## More Information

For a more in-depth tutorial, please take a look at the [full tutorial](http://www.cse.nd.edu/~dthain/talks/condor-crc-april-2009.ppt).
