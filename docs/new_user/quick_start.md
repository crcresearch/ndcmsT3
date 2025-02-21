# Quick Start Guide

Detailed below are common methodologies and good practices for access and using the CRC's resources.

## Cluster Computing Format

The organization of a super-computer or cluster is quite different from traditional desktop computing. Instead of all commands executing on the machine in front of you, everything is executed on a remote server, at times with no user interaction. A typical connection will look like:

    Cluster
    +---+---+---+---+---+---+
    +---->+-------+           |   |   |   |   |   |   |
    |     | Front |           |   |   |   |   |   |   |
    |     |  End  |           |   |   |   |   |   |   |
    |     +---+---+           |   |   |   |   |   |   |
    Internet       |         |               +---+---+-------+---+---+
    Connection     |         |                           ^
    |         |                           |
    |         +---------------------------+
    ____|______
    \+--+------+
    \|Personal |
    \|Computer |
    \+---------+

To access the CRC infrastructure, you will connect to a Front end or Head node where you can prepare, submit, check on, and delete your batch jobs. Most jobs executed on CRC servers are batch jobs, which are non-interactive well-defined jobs which are queued and executed on a compute host when resources are available.

------------------------------------------------------------------------

## Front End Systems

> [!NOTE]
> For users connecting from off campus, please review the section `off-campus-connect` first.

In order to submit and run jobs on CRC servers, the first step is to connect to a front end machine. This machine will facilitate connections to the job manager which will handle your job from submission to execution.

The CRC provides the following front-end machines for compilation and job submission. Each machine is configured with *identical* software stacks.

- crcfe01.crc.nd.edu (2 12 core Intel(R) Haswell processors with 256 GB RAM)
- crcfe02.crc.nd.edu (2 12 core Intel(R) Haswell processors with 256 GB RAM)

> [!WARNING]
> Front-Ends are NOT for large long running (\>1hr) jobs. For such jobs please using the queuing system and compute nodes. Any long running on a public front end machine may be killed.

To see how to connect to a front end from your favorite OS, see `connecting_to_crc`.

------------------------------------------------------------------------

### Proper Front End Usage

The front end machines are the interface to the the rest of the computing resources of the CRC. There are both faculty owned and public front ends, the majority of information found here will pertain to public front ends. The public front ends are shared by all of campus and external collaborators to perform a wide variety of tasks related to research.

------------------------------------------------------------------------

#### Logging in

There are 2 general access front ends available.

``` shell
crcfe01.crc.nd.edu
crcfe02.crc.nd.edu
```

The primary interface to these machines is through SSH. To connect to either of these machines, you'd start either a terminal (Mac and Linux) or an ssh client on Windows. For Windows we recommend `MobaXterm`. You'd then either follow the instructions on the ssh client or enter the following command in a terminal, replacing "X" with the desired machine (1 or 2) and replacing "netid" with your Notre Dame NetID:

``` shell
ssh netid@crcfe0X.crc.nd.edu
```

If you are having troubles logging in and have a new account, be sure to read the welcome email which was sent on the activation of your CRC account:

- First login to [okta.nd.edu](https://okta.nd.edu).

- If you continue to have trouble logging in:

  > - Check all spelling
  > - Be sure your ssh client and OS are updated/patched.

If all else fails, notify us via email at <CRCSupport@nd.edu> and please provide the following details:

- ND netid
- Which machine you're trying to connect to
- Your computer's Operating System (Windows, Mac, Linux, etc)
- The ssh client you're using (Putty, MobaXterm, terminal, etc).

> [!NOTE]
> If you intend on using `HTCondor` for job submissions, the front end machine to connect to is `condorfe.crc.nd.edu` If you need to compile code with infiniband support, connect to `epycfe.crc.nd.edu`.

------------------------------------------------------------------------

#### Utilizing Front Ends

Once connected to a front end, you may begin preparing jobs for submission to either UGE or HTCondor (on `condorfe.crc.nd.edu`). There are a few important notes about operating within the front end environment:

- There will be other users connected to this machine, any disruptive behaviour will be addressed.
- Any task / process running longer than 1 (ONE) hour may be removed / killed by an administrator. Submit long running jobs to the batch system.
- Do not launch large mpi or smp processes on a front end.
  - More information on the queues and UGE can be found on the `crc_uge_env` page.
- When logged in, you will be within your user AFS space. Initially, there will be a few directories there. **DO NOT** remove your `Public` directory, this contains important login scripts.
- The default shell is `BASH`. If you have an older account, your login shell may be `tcsh`. If you'd like to request bash as your default, email <CRCSupport@nd.edu>.
- Software which is not included in an install of RHEL, can be accessed through the `modules`.
- Small processing which is not disruptive/resource intensive can be done on the front ends. This is normally pre-processing or post-processing after completion of UGE jobs.
- All CRC systems are a Linux variant. For tip on Linux commands, see `Linux Coding Cheat Sheet <doc/fwunixref.pdf>`

------------------------------------------------------------------------

For submitting jobs to UGE, please see `submitting_batch_jobs`. More sample scripts/submissions can normally be found on a module/software's page. Search for the module in question in the search bar on the top left of the page.

## Available Software

The software environment on CRC servers is managed utilizing `modules`. A module is a an easy way to manage the necessary environmental paths and changes to utilize different pieces of software. You can easily modify your programming and/or application environment by simply loading and removing the required modules. The most common module commands are:

    module avail

    module list

    module load module_name

More information on the CRC Modules can be found on the `modules` page.

------------------------------------------------------------------------

## File Systems

The CRC provides two complimentary file systems for storing programs and data.

### AFS

- Distributed file system
- Initial 100GB allocation
- Longer-term storage; backup taken daily
- User file system, your home directory lives here
- You can check your current AFS usage with the following command:

``` shell
quota
```

### Panasas

- High-performance parallel file system
- To obtain an allocation on /scratch365, send us a request at <CRCSupport@nd.edu>.
- Used for runtime working storage; no backup, purged yearly.
- You can check your current /scratch365 usage with the following command:

``` shell
pan_df –H /scratch365/netid
```

Further info: `storage`

------------------------------------------------------------------------

## File Transfers

To transfer files from your local desktop MacOS filesystem to your CRC filesystem space we recommend installing and using the following file transfer (GUI) client: `cyberduck`

For Windows users, we recommend using `mobaxterm` as both an SSH client and a file transfer client.

If you would like to transfer data between the CRC servers and Google Drive, we recommend the `rclone` tool.

------------------------------------------------------------------------

## Job Scripts

A typical batch job is a simple script which contains the commands necessary to execute the job which needs to be accomplished whether this is an R, Matlab, or Python script or a compiled executable.

Jobs are submitted to the compute nodes via the Univa Grid Engine (UGE) batch submission system. Basic UGE batch scripts should conform to the following template:

    #!/bin/bash

    #$ -M netid@nd.edu   # Email address for job notification
    #$ -m abe        # Send mail when job begins, ends and aborts
    #$ -pe smp 24    # Specify parallel environment and legal core size
    #$ -q long       # Specify queue
    #$ -N job_name   # Specify job name

    module load xyz  # Required modules

    mpirun -np $NSLOTS ./app # Application to execute

Further info: `submitting_batch_jobs`

------------------------------------------------------------------------

### Parallel Environments

To use more than one core in a job, you must specify a parallel environment. A parallel environment is a way to specify whether to use multiple cores on one machine or to use multiple nodes (more than 1 server). The two parallel environments are below.

[TABLE]

> [!NOTE]
> \* If no parallel environment is requested (i.e. you do not specify a -**pe** parameter), then the default execution environment is a single-core serial job. \* **Every machine has one thread per core!**

Further info: `crc_uge_env`

------------------------------------------------------------------------

### Queues

CRC provides two general-purpose queues for the submission of jobs (using the **-q** parameter):

Below you'll find a summarization of the available queues.

[TABLE]

> [!NOTE]
> In the above table, the long queue and gpu queue can include any faculty / lab owned machines you have access to. The runtime limit and example machines will most likely be different from the table above. Speak to your lab-mates, PI, or email us at <CRCSupport@nd.edu> if you'd like to know specifics of those machines.

If you wish to target a specific architecture for your jobs, then you can specify a `host group` instead of a general-purpose queue.

------------------------------------------------------------------------

### Host Groups

If you wish to target machines in a finer granularity than queues, there are host groups. For example, the general access machines are the "@crc_d32cepyc" host group. If your research lab or faculty advisor has purchased a machine(s), there is most likely a host group you can target.

Monitoring the status and availability of resources at a glance can be done with the **free_nodes.sh** script.

``` shell
free_nodes.sh -G              # For general access
free_nodes.sh -D              # For Debug nodes
free_nodes.sh @crc_d32cepyc    # You can target host groups
```

For further resource monitoring, consult the help information: `free_nodes.sh --help`

------------------------------------------------------------------------

## Job Submission and Monitoring

Job scripts can be submitted to the SGE batch submission system using the `qsub` command:

``` shell
qsub job.script
```

Once your job script is submitted, you will receive a numerical `job id` from the batch submission system, which you can use to monitor the progress of the job.

Job scripts that are determined by UGE to have made valid resource requests will enter the queuing system with a queue-waiting `qw` status (once the requested resources become available, the job will enter the running `(r)` status). Job scripts that are determined not to be valid will enter the queuing system with an error queue-waiting `(Eqw)` status.

To see the status of your job submissions, use the `qstat` command with your username (netid) and observe the status column:

``` shell
qstat –u username
```

For a complete overview of your job submission, invoke the qstat command with the job id:

``` shell
qstat –j job_id
```

To see all pending and running jobs, simply type `qstat` without any options

``` shell
qstat
```

The main reasons for invalid job scripts (i.e. having Eqw status) typically are:

- Illegal specification of parallel environments and/or core size requests
- Illegal queue specification
- Copying job scripts from a Windows OS environment to the Linux OS environment on the front-end machines (invisible Windows control code-blocks are not parsed correctly by UGE). This can be fixed by running the script through the `dos2unix` command

### Why is my job waiting in the Queue?

The first reason a job could be waiting in a queue is simply because there are no resources available yet for your job to begin running. Note that for the most part (especially if you are using the general access machines), you are sharing resources with every other CRC user.

The first item to check if if there are available resources at the moment for your job, you can use the `free_nodes.sh` script to check, for example:

    free_nodes.sh -G

Note that even if there resource shown as available, the scheduler could be freeing up space for a larger job with higher priority than yours. There is also a maximum number of concurrent jobs to avoid overloading the systems with one person's jobs, this number fluctuates during the year depending on system utilization.

- If you need to push a large amount of jobs consisting of less than 4 cores each, perhaps `condor` is a good fit for you.

#### Parallel Jobs

If you requested a parallel environment with your job, be sure that requested environment adheres to the target resources.

For example, be sure the machines you're targeting through `smp` can support the number of cores requested, a request of `smp 75` will never be served on the general access or `@crc_d32cepyc` host group.

If you're requesting an `mpi-X Y` environment, be sure the `Y` value is a multiple of `X`, and be sure `X` is a valid number for the targeted machines. For example, `mpi-64 128` is the correct requested environment of 2 machines out of the general access queue.

------------------------------------------------------------------------

### Job Deletion

To delete a running or queued (e.g. submissions with `Eqw` status) job, use the following command:

``` shell
qdel –j job_id
```

------------------------------------------------------------------------

### Job Resource Monitoring

To better understand the resource usage (e.g. memory, CPU and I/O utilization) of your running jobs, you can monitor the runtime behavior of your job’s tasks as they execute on the compute nodes.

To determine the nodes on which your tasks are running, enter the following `qstat` command along with your username. Record the machine names (e.g. d12chas400.crc.nd.edu) associated with each task (both MASTER and SLAVE):

``` shell
qstat -u username -g t
```

There are two methods for analyzing the behavior of tasks (once you have a machine name):

- Xymon GUI Tool (detailed breakdown per task on a given machine)

  > Xymon is a GUI tool to analyze the behavior of processes on a given CRC machine. It can be accessed on [CRC Xymon](https://mon.crc.nd.edu). Note that you will need to be on the ND network to see this site.
  >
  > Use Xymon to navigate to the specific machine and then view the runtime resource usage of tasks on the machine.

- `qhost` command (aggregate summary across all tasks on a given machine)

  > You can summarize the resource utilization of all tasks on a given machine using the following qhost command:

``` shell
qhost -h machine_name
```

------------------------------------------------------------------------

### Job Arrays

> [!NOTE]
> If you find that you need to frequently submit 50 or more different jobs, we request that you implement those tasks within a job array. Grid engine is able to handle arrays much more efficiently than tens or hundreds of individual scripts from a single user. Fewer individual tasks reduces load on the job scheduler and improves overall performance.

If you have a large number of job scripts to run that are largely identical in terms of executable and processing tasks, (e.g. a parameter sweep where only the input changes per run) then you may want to use a `job array` to submit your job. You specify how many copies of the script that you need to run with the `-t` parameter. For example, specifying:

    #$ -t 1-3

will run 3 identical job scripts. Job arrays use an internal environment variable, `$SGE_TASK_ID`, to dinstinguish between the different jobs and this can also be used as input to your job.:

    #!/bin/bash

    #$ -pe smp 1           # Specify parallel environment and legal core size
    #$ -q long             # Specify queue 
    #$ -N job_name         # Specify job name
    #$ -t 1-3              # Specify number of tasks in array

    module load python

    # Note that different tasks will use different input files.
    python analyze.py < data.$SGE_TASK_ID

If your tasks do not map directly to consecutive integer numbers, you may use a shell array within the script to do the mapping for you. In this example, we specify three different input strings that are then passed as an argument to the Python script:

    #!/bin/bash

    #$ -pe smp 1           # Specify parallel environment and legal core size
    #$ -q long             # Specify queue 
    #$ -N job_name         # Specify job name
    #$ -t 1-3              # Specify number of tasks in array

    module load python

    fruits=( "orange" "apple" "pair" )

    # Note that different tasks will use different input parameters.
    python pie_recipe.py ${fruits[$SGE_TASK_ID-1]}

> [!WARNING]
> To avoid extraneous emails, please do not use email notification options (-M and -m) when submitting an array job.

More detailed information about job arrays is available from the manual page:

    man qsub

or from this website: [qsub manual](https://gridscheduler.sourceforge.net/htmlman/htmlman1/qsub.html)
