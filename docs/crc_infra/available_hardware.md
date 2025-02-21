# Available Hardware

The CRC maintains a mix of compute, interactive, and storage machines. Below is a short descriptive list of the CRC owned machines available.

## Interactive Front End(s)

**crcfe01.crc.nd.edu**

``` shell
Dell PowerEdge R6525 Server (64 total cores)
Dual 32 core AMD EPYC 7543 CPU @ 2.80GHz
256 GB RAM  -  1 TB Solid State Disk - SSD
```

**crcfe02.crc.nd.edu**

``` shell
Dell PowerEdge R6525 Server (64 total cores)
Dual 32 core AMD EPYC 7543 CPU @ 2.80GHz
256 GB RAM  -  1 TB Solid State Disk - SSD
```

**daccssfe.crc.nd.edu**

``` shell
Dell PowerEdge R930 Server (64 total cores)
Quad Intel(R) Xeon(R) CPU E7-4850 v4 @ 2.10GHz (64 total cores)
3 TB RAM
6 TB drive - Using Dell H730P hardware RAID controller - RAID 10 using 11 x 1 TB SAS drives
```

------------------------------------------------------------------------

## CRC Owned Compute Clusters

**d32cepyc182-d32cepyc247.crc.nd.edu**

``` shell
Dell PowerEdge R6525 Server (64 total cores)
Dual 32 core AMD EPYC 7543 CPU @ 2.80GHz
256 GB RAM  -  1 TB Solid State Disk - SSD

**Usage:** Queue syntax for job submission script:
   #$ -q long
   or
   #$ -q *@@general_access 
```

**d24cepyc067-d24cepyc133.crc.nd.edu**

``` shell
HPE ProLiant DL385 Gen10 Servers
Dual 24 core AMD EPYC 7451 CPU @ 2.30GHz
128 GB RAM  -  2 TB Solid State Disk - SSD

**Usage:** Queue syntax for job submission script:
     #!/bin/bash
     #$ -q hpc
```

**d24cepyc134-d24cepyc137.crc.nd.edu**

``` shell
HPE ProLiant DL385 Gen10 Servers
Dual 24 core AMD EPYC 7451 CPU @ 2.30GHz
128 GB RAM  -  2 TB Solid State Disk - SSD

**Usage:** Queue syntax for job submission script:
     #!/bin/bash
     #$ -q hpc-debug
```

------------------------------------------------------------------------

## Large Memory Systems

**d32cepyc254-d32cepyc257.crc.nd.edu**

``` shell
Dell PowerEdge R6525 Server (64 total cores)
Dual 32 core AMD EPYC 7543 CPU @ 2.80GHz
2 TB RAM  -  1 TB Solid State Disk - SSD

**Usage:** Queue syntax for job submission script:
        #!/bin/bash
        #$ -q largemem
```

------------------------------------------------------------------------

## GPU Systems

**qa-a10-023-qa-a10-034.crc.nd.edu**

``` shell
12 Dell PowerEdge R750xa Server
Dual 16 core 2.9GHz Intel(R) Xeon(R) Gold 6326 processors - 32 total cores
256 GB RAM
4 NVIDIA A10 GPU accelerators

**Usage:** Command for interactive access requesting 1 GPU:
         qrsh -q gpu -l gpu_card=1

       Queue syntax for job submission script requesting 1 GPU:
         #!/bin/csh
         #$ -q gpu
         #$ -l gpu_card=1
```
