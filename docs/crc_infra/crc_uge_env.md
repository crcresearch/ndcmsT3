# UGE Environment

General access to the CRC computational resources is managed with the Grid Engine software tool set. Users should be aware of 4 key framework concepts/components in order to achieve the most effective access to CRC compute nodes. The 4 components covered here are queues, parallel environments, host groups, and user groups. In this document the terms core, slot, and process can be considered suitably equivalent. Note this documentation only covers the CRC general resource pool. There are additional unique instances for particular research group requirements.

Using the information found within this page, you are able to submit jobs to the CRC's batch system. For more information on the submission process, see `submitting_batch_jobs`.

------------------------------------------------------------------------

## Queues

CRC queues are configured for differing user run time requirements:

- long (15 days, no limit on \# of cores, normal priority)
- a maximum limit of 50 running jobs combined between all queues
- a maximum limit of 2,000 tasks within an array job
- NOTE: The long queue contains many machines with various hardware architectures. The debug queue consists of only Intel based servers each with a total of 24 cores with 64GB of RAM\*

Queues are specified in your submit script with the syntax

``` shell
#$ -q long
```

------------------------------------------------------------------------

## Parallel Environments

Parallel Environments (PE) allow the user to specify the mechanisms by which the target code runs in parallel. If a PE is not specified the job will be treated as a serial job and be allocated 1 core. Note that all CRC machines are configured for **ONE** thread per core.

- Use the MPI PEs (mpi-24, mpi-48, etc...) when you need more cores than available on 1 machine
- Use the SMP PE when you need less than or equal to the number of cores on one machine

Common Parallel Environments include:

    * smp (shared memory processing on a single machine up to 64 cores)

    * MPI is the PE for MPI applications at the CRC (which have been compiled against OpenMPI, MPICH2, and MVAPICH MPI libraries)*

    * mpi-24 (multi [24 core per] machine processing in 24 core increments)

    * mpi-32 (multi [32 core per] machine processing in 32 core increments)

    * mpi-64 (multi [64 core per] machine processing in 64 core increments)

!!! note
    Increments (24,48,64) are mapped to total cores per server. A server with 24 cores will not accept mpi-32 requests.

PEs are specified in your submit script with the syntax:

    #$ -pe mpi-64 128

    #$ -pe smp 8

------------------------------------------------------------------------

## Host Groups

Hosts groups allow for the specification of hosts/servers with differing architectures. For example a user can specify a host group of Intel versus AMD based servers.

Host groups are specified with the syntax, using the general access host group as an example::

    #$ -q *@@crc_d32cepyc

To see all available host groups, you can use the -shgrpl flag with qconf as seen below. Note that you will not be able to run jobs on all host groups, only on those which are a part of your research group / groups you have been granted access to.

``` shell
qconf -shgrpl
```

------------------------------------------------------------------------

## User Groups

User groups allow for the designation resource access to specific individuals. This is most often utilized for faculty or group owned resources accessible through the Grid Engine like host groups as explained above. Users do not need to specify their user group as this is automatically determined by user ID. Users must however make sure via their faculty adviser that their ID is in the user group. If it is not, have your PI / faculy advisor send an email to <CRCSupport@nd.edu>.

To view all user lists, pass the -sul flag to qconf.

``` shell
qconf -sul
```
