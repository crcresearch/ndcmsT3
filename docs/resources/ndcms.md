# NDCMS

The page contains information pertaining to the [Notre Dame High Energy Physics Research Group](https://physics.nd.edu/research/research-groups/the-high-energy-elementary-particle-physics-group/). Below you will find navigation links to all content within this page.

Page Navigation

!!! note
    All of this documentation is specific to our NDCMS T3 cluster. It is written and maintained by the users of that cluster, which includes **you.** Please help us keep this up-to-date. The most important way in which you can do this is to send e-mail to <NDT3@listserv.nd.edu> if you try any of the documentation here and find it's out of date. Not on the mailing list? Contact <klannon@nd.edu> to get added.

## Logging In and Setting up your Software Environment

From inside the ND network (being physically on campus or via VPN) or at CERN, one can log directly into the interactive head node:

``` shell
ssh -Y *username*@glados.crc.nd.edu
```

Alternatively, if that doesn't work, try:

``` shell
/usr/bin/ssh -Y -l username@glados.crc.nd.edu
```

From a remote location (e.g. off campus, etc.), one must *first* log into the VPN. More information is available in the section `off-campus-connect`.

``` shell
crcfe01.crc.nd.edu
crcfe02.crc.nd.edu
```

Tip: Don't forget the `-Y` option for both ssh commands if you want any X windows to show up on your screen (i.e. opening TBrowsers, histograms, etc.)

For more comfort, you can create the file ".ssh/config" on your local computer and insert an entry like

``` shell
Host glados, crcfe01, crcfe02
    HostName        %h.crc.nd.edu
    User            *your_username*
    ForwardX11      yes
    ForwardX11Trusted yes
    ControlMaster   auto
    ControlPath     ~/.ssh/control-%r@%h:%p
```

The last two lines are optional. They set up ssh to allow a second session to "tunnel" through the first one. As long as you have one ssh session open, further connections to earth do not require you to enter your password again. This should only be done for Notre Dame, as normally Kerberos authentication and ssh keys work better.

With this setup, you may type just `"ssh glados"` to log into `glados`, and likewise for `crcfe01` and `crcfe02`.

------------------------------------------------------------------------

### Setting up environment

!!! note
    Only the `glados.crc.nd.edu` machine has the software required to use CMSSW, CRAB, etc.

*First login or when experiencing trouble with Condor:* The first time you log into glados, set the AFS permissions of a directory to use for Condor as follows:

Your home directory needs to be readable by anyone on campus because Condor doesn't run the jobs under your username. In your home directory, do the following:

``` shell
cd ~
fs sa . nd_campus rl
fs sa . system:authuser rl
```

If you have any subdirectories which you plan to use for Condor, you'll need to add read permissions for them as well. If you have a large number of subdirectories to modify see \[\[#If You Forgot to Set Your AFS Permissions at First Login\|here\]\].

For the directory where CRAB is going to write outputs, you need to give write permission to any user on campus. Be careful doing this. You don't want to do this for directories where someone else could make a mistake and delete important files (like your home directory). Assuming your condor directory is called "my condor_directory", then you should do the following:

``` shell
cd my_condor_directory/
fs sa . nd_campus rlidwk
fs sa . system:administrators rlidwka
fs sa . system:authuser rlidwk
```

You'll have to configure Condor so that any output files are stored in the specific directory to which you've granted write permissions. This will avoid possible problems in which condor does not have the correct permissions to launch jobs from your AFS space.

> [!WARNING]
> If you didn't do this the first time you logged in or created your Condor directory, this *is* fixable. See `here <if-you-forgot>`

------------------------------------------------------------------------

### Add your proxy DN and Notre Dame username (your username on glados.crc.nd.edu)

1)  Please add your proxy DN and ND username in the google doc below:

<https://docs.google.com/document/d/1piHU3tvAdPEXis-evx1mzOD8YwyQ59SbgeLUZR4U1cE/edit>

2)  Let us know that you've added your DN to the shared google doc file, please email <ijohnso1@nd.edu> and CC <khurtado@nd.edu>

!!! note
    If you would like to switch to a more modern shell, such as bash, you need to contact the CRC to have your shell changed in their database.\*\*

------------------------------------------------------------------------

### If You Forgot to Set Your AFS Permissions at First Login

You can go back and set your AFS permissions recursively after the fact using the "find" command carefully:

Starting in your home directory or one of your subdirectories, you can do the following to set read permissions for all subdirectories below.

``` shell
find . -type d -exec fs sa {} nd_campus rl \;
find . -type d -exec fs sa {} system:authuser rl \;
```

For the Condor directory and any of its subdirectories, you can use the following to set permissions correctly:

``` shell
cd my_condor_directory
find . -type d -exec fs sa {} nd_campus rlidwk \;
find . -type d -exec fs sa {} system:administrators rlidwka \;
find . -type d -exec fs sa {} system:authuser rlidwk \;
```

------------------------------------------------------------------------

## PhEDEx

Back to `Top of the Page <ndcms>`.

The CMS experiment is managing hundreds petabytes of data recorded by the detector and simulated physics events. Data are transferred to distributed CMS sites for storage, processing and analysis. PhEDEx (Physics Experiment Data Export) maintains the knowledge of data replicas locations and manages data transfers between the sites.

### Requesting a PhEDEx Transfer to the Notre Dame Tier 3 Site (US_T3_NotreDame)

1.  use DAS [https://cmsweb.cern.ch/das/\]](https://cmsweb.cern.ch/das/]) to find the dataset you're interested in transferring

In the search field, use this format to search for your favorite dataset:

``` shell
dataset dataset=/Tau/Run2011A-PromptReco-v4/AOD
```

Click on "Subscribe to PhEDEx"

2.  Request a transfer to the US_T3_NotreDame site

Under "Destinations" check the box next to

``` shell
T3_US_NotreDame
```

At the bottom of the page, click "Submit Request", and click "confirm" on the following page. You will receive an email notification with a link to the status of your request.

3.  You will receive a confirmation email when your request is approved by the site admin.

------------------------------------------------------------------------

## Monitoring Tier 3 components and Jobs

Back to `Top of the Page <ndcms>`.

Below are a collection of links that can be used to try to monitor the health of the Tier 3 components. If you see unexpected behavior--fewer than normal running jobs, slower job I/O, failing file transfers, etc.--please check here for indications of what might be wrong.

Note: Unless otherwise noted, the links below only work for those on campus or connected with a VPN.

### HTCondor Monitoring

To check the general status of HTCondor within the CRC, you can view the [HTCondor utilization matrix](http://condor.cse.nd.edu/condor_matrix.cgi), for more information on HTCondor itself view the `HTCondor <condor>` page.

- The official documentation for condor command line utilities can be found [here](http://www.cs.wisc.edu/condor/manual/v7.0/9_Command_Reference.html).

#### HTCondor Job Monitoring

To view the status of all jobs in the condor queue (or for a specific user), use the following command:

``` shell
condor_q username
```

*Note: if you have submitted jobs from outside ND, condor maps your username to "uscms01".*

If you have jobs in the "Idle" state, you can ask condor why they are not running, using the following command:

``` shell
condor_q -better {jobID}
```

This command will give you a list of all the reasons a job can be rejected, and tell you how many of the 600 nodes are rejecting your job for that reason.

It is possible to manually abort your jobs using the following command:

``` shell
condor_rm (jobID or username)
```

However, it is recommended to cancel the jobs via CRAB, because CRAB will often get confused if its jobs suddenly disappear. To abort jobs using CRAB, use:

``` shell
crab -kill (job_range or "all")
```

The memory restriction per node on the condor queue is set to 1 GB by default. This is measured against the virtual memory of the process, meaning once a job's image size (virtual memory) becomes larger than 1 GB it could be idled indefinitely. Many CMSSW jobs tend to accumulate much more virtual memory than physical memory (up to 2x in some cases). This causes jobs to be idled when they don't necessarily need to be. You can change the default 1 GB memory restriction through the command condor_qedit. If you choose to change this value, try to be reasonably sure that your jobs won't be using more than 1 GB of real memory as this could cause problems for the machine the job is running on (including possibly crashing it). In order to change the memory limit, do the following:

1)  find the job requirements string for one of your jobs:

``` shell
> condor_q -l  <condor_job_id> | grep Requirements
```

2)  copy the string in quotes that comes after "Requirements = " in the output.

The requirements string will have an element like

``` shell
( ( TARGET.Memory * 1024 ) >= ImageSize )
```

Since we can't edit the value of 'TARGET.Memory', we'll have to change the proportionality factor...from 1024 to, say, 2048.

3)  reset the Requirements string for a job or group of jobs, using the old requirements string with the new proportionality factor.

``` shell
> condor_qedit  <user/condor_job_id> Requirements <new_req_string>
```

Note that since the default string contains double quotation marks (""), you'll need to surround your \<new_req_string\> with single qoutes (''). In addition, if you copy and paste the output from step one, you'll need to remove the space in "\> =" to have instead "\>=".

Remember that 'condor_qedit' will work on a specific job ID (xxx.y), a job cluster ID (xxx), or a username.

------------------------------------------------------------------------

## Storage 

Back to [Top of the Page](<https://crcresearch.github.io/ndcmsT3/>)

Users have access to a variety of storage locations. The key is knowing the tradeoffs of each, so you can identify what's the right resource for you to use. If you don't have a directory in any of the spaces listed, ask for help at <ndt3-list@nd.edu>.

[Detailed information regarding Storage can be found here](<https://docs.crc.nd.edu/transition/netapp/netapp.html#>)

### Home Area

Each user has a home directory on `/users` with 100GB of personal disk space that is backed up nightly and cannot be increased. This is where you should keep software that you're developing, papers that you're writing, your thesis draft, etc. Basically, use this for anything that you would be very sad to have to recreate if it were accidentally deleted or lost due to hardware failure.

To check the quota use `df -h /users/username`

------------------------------------------------------------------------

### Scratch Space

Also users get 250GB of non-backed up space in `/scratch365/<username>` and non-backed up space for small files in `/store/smallfiles`. There is no quota on `/store/smallfiles` but the total space is 80 TB and must be shared by all users. In general, this storage is useful to use for temporary files of intermediate sizes. If you need reasonable access performance for multiple jobs to the files (e.g. you're going to run more than ~100 jobs reading or writing files in the batch system) then don't use `/store/smallfiles` as the performance degrades severely. In that case, either use `/scratch365` or `/cms/cephfs/data/store/user` (see below).

------------------------------------------------------------------------

#### Accessing the scratch space from grid jobs

Grid jobs running at ND (e.g: via CMS Connect) need to prepend `/cms` to the /scratch365 and /store/smallfiles directories (`/cms/scratch365/<username>` and `/cms/store/smallfiles`) due to standard requirements in the CMS Global Pool for singularity-enabled sites for non-hadoop storage spaces to be visible, but they point to the same storage location.

------------------------------------------------------------------------

### CEPH Space

CEPH is mounted at `/cms/cephfs/data/store/user`. 

To access CEPH data, please use `skynet013.crc.nd.edu on port 1094`. The XRootD workers are `hactar01, hactar02, hactar03, skynet014 and skynet015`.

``` shell
xrdmapc skynet013:1094 --list all
0**** skynet013.crc.nd.edu:1094
      Srv hactar03.crc.nd.edu:1094
      Srv hactar02.crc.nd.edu:1094
      Srv hactar01.crc.nd.edu:1094
      Srv skynet015.crc.nd.edu:1094
      Srv skynet014.crc.nd.edu:1094
```

To access the external data (xcache), please use `skynet013.crc.nd.edu on port 1096`. 

``` shell
xrdmapc skynet013:1096 --list all
0**** skynet013.crc.nd.edu:1096
      Srv primeradiant01.crc.nd.edu:1094
      Srv primeradiant02.crc.nd.edu:1094
      Srv primeradiant03.crc.nd.edu:1094
      Srv primeradiant04.crc.nd.edu:1094
      Srv primeradiant05.crc.nd.edu:1094
      Srv primeradiant06.crc.nd.edu:1094
```

[OUTDATED - TO BE UPDATED]
The Hadoop file system (hdfs) is a different sort of files system than most. Hadoop breaks your data up into blocks of ~128 MB and scatters two copies of each block across multiple physical disks. It does this for two reasons: The replication makes the system more resilient against hardware failures and it also provides better performance when many different jobs are reading or writing to the system. Like `/store/smallfiles`, Hadoop doesn't have per user quotas, and there is a lot of space available (at the time of this writing, 644 TB of raw space, but remember that every TB you store takes up ~2 TB of space because of replication). Hadoop is also the file system that is accessible with CMS/grid tools like gfal and XRootD. You should use Hadoop whenever you have very large datasets, when you need to access your data using gfal or XRootD, or when you will be accessing your data with many parallel running jobs (anything more than 100). There are some caveats: \* Hadoop doesn't handle very small files well. If you write large numbers of files with sizes on the order of MB, don't use Hadoop. For files in that size range, use `/store/smallfiles` or `/scratch365` \* Hadoop doesn't provide posix access directly. This means that normally you can't use commands like ''ls'' or ''cp''. We use something called FUSE to provide posix access to Hadoop, but FUSE can be broken if you try to read too much data too quickly. So, when running many batch jobs, its better to access the data directly using hdfs commands, or to use a tool, like XRootD or Lobster that does this for you. If you're running jobs on data in `/hadoop` and they are getting stuck or having Input/Output errors, you've probably crashed FUSE on some of the nodes. If this happens, ask for help \[[mailto:ndt3-list@nd.edu](mailto:ndt3-list@nd.edu) <ndt3-list@nd.edu>\]. \* ROOT cannot write directly into `/hadoop/store/user`. If your job is producing ROOT output, write it first to local disk (every worker node has local disk for this purpose) and then at the end of the job, copy the output to `/hadoop/store/user` (possibly using gfal to avoid problems with FUSE!). Again, if you have questions, ask on <ndt3-list@nd.edu>.

------------------------------------------------------------------------

### Lobster Working Storage

Lobster doesn't do well working out of your AFS home directory. When you run Lobster jobs, you should tell Lobster to make your working area in `/tmpscratch/users/<username>`. Space is limited and there are no user quotas, so monitor carefully and clean up old files. We reserve the right to clean out this space if someone is using too much and not playing nice with others!

!!! note
    To Add you proxy DN and Notre Dame username, go to: <https://wiki.crc.nd.edu/w/index.php/NDCMS_SettingUpEnvironment#Add_you_proxy_DN_and_Notre_Dame_username_.28your_username_on_earth.crc.nd.edu.29>

------------------------------------------------------------------------

### Storage space for PhEDEx

All CMS data are stored using the /store convention, Therefore we only need to map:

``` shell
/+store/(.*)
```

Translation rules for PFN to LFN (Physical File Name to Logical File Name):

``` shell
/cms/cephfs/data/store ==> /store
```

To see how much space is available in the CEPH /store area, you can type the following from earth:

``` shell
df -h /cms/cephfs/data/store
```
