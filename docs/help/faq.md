# FAQ

Below is a collection of frequently asked questions.

------------------------------------------------------------------------

## CRC Accounts

### How Do I Request a CRC Account?

The easiest way to request a new account is to fill out the [CRC Account Request Form](https://docs.google.com/a/nd.edu/forms/d/1VNXxqdt-r3SUFQCZY9v6kXskmfuGjCXsAi1MegyuCPo/viewform). After we create your account, you will receive an email with further instructions and additional information on synchronizing your password with the rest of campus. Please be sure to read and follow the instructions carefully. Also note that a 1 hour introductory course is a co-requisite.

### How Do I Find Some Quick Start Information?

Consult the `quick-start-guide` for advice on getting started with CRC resources quickly.

### I Can't Login (New User / First Time Login)

Your password for logging into CRC resources is your standard ND account password. HOWEVER, when you first obtain an account you need to login to [okta.nd.edu](https://okta.nd.edu) (as instructed by the CRC Welcome email). If needed, you can reset your password [here](https://accounts.nd.edu/password).

### I've Forgotten My Password

Your CRC password is your standard Notre Dame account password. If your account has just been created, you will need to wait 1 hour before logging into Okta:

[Okta Login](https://accounts.nd.edu/password)

This will synchronize your Notre Dame and CRC accounts. If you are still having difficulties, please email <crcsupport@nd.edu>.

### How Do I Request an Account for an External Collaborator?

A faculty member may request an affiliate account for an external collaborator via this form:

[Requesting NetID Access for Affiliates](https://nd.service-now.com/nd_portal?id=sc_cat_item&sys_id=1ad5a34637d19240f8b78ff1b3990e30)

You may need to login to see the form. All questions regarding the *Affiliate* are for your collaborator's information. You may ignore or select *No* on questions regarding Departmental Access.

If the form is not visible, it can be searched for in the search bar. This will bring you to the form where you may enter in the information. Note that you will need the individual's date of birth and their NDID#. The latter may be obtained by calling Human Resources. If the applicant has never had a Notre Dame account, you may place zeros in the form.

### How Can I Maintain My CRC Account After Graduating/Leaving Notre Dame?

> [!NOTE]
> The university disables staff accounts on the final day of employment, while graduating student accounts are disabled at the conclusion of the current semester.

If you have research related to work here at Notre Dame and are able to justify continued access, you may request a faculty member sponsor your account as an external affiliate. These accounts are valid for up to one year, but may be renewed annualy. This process can be initiated by having your sponsor complete an `external account request <Request External Account>`. To avoid losing your CRC account and all associated data, please complete this process prior to your account being disabled.

### I am Getting A "Module: Command Not Found" Message When I Log In To My CRC Account

This message generally indicates that you have deleted/corrupted your shell configuration scripts which prevent the Module environment from initializing correctly. To repair your scripts please see the `login_scripts` page for instructions.

------------------------------------------------------------------------

## Remote Access

### Why Is My Remote Connection So Slow?

Slow remote GUI response time is actually quite difficult to troubleshoot.

- First because slow in this regard does not have a fully tangible metric

  > - In this case slow means it is impractical for you to work with

- Secondly because it varies on numerous conditions:

  > - OS and Graphics card/drivers on your system
  > - Remote visualization client (remote GUI)
  > - Network reliability (on campus wired is usually pretty good)
  > - Load (CPU, RAM, Network, Disk I/O) on the machine you are trying to access

For the first two you need laptop/desktop support which is provided by your department/college IT support folks.

For the third you can verify if you are getting at least 100Mbps consistently: [ndt speedtest](https://ndt.crc.nd.edu/speedtest)

For the fourth you can monitor the load on CRC machines here: <https://mon.crc.nd.edu/xymon>

### When I try to login I see the error: "Algorithm Negotiation Failed", what does this mean?

The CRC systems only accept connections from an updated form of encryption, therefore if you are using an older ssh client you may see this error.

To solve this issue, first try updating your OS or current ssh client. If you are connecting from Windows, we recommend `mobaxterm`.

If you are connecting from an older Mac Os system which cannot be updated, you can still connect to CRC systems by first connecting to `jumpbox.nd.edu`. If you continue to have login issues, contact CRC Support.

### When I try to connect CRC through VSCode, why does Remote SSH keep asking for password again and again?

This happens because, after your first `ssh` (through the plugin within VSCode) it creates a lock file inside the folder `~/.vscode-server/` within your CRC account and that prevents your subsequent ssh. To resolve this, in your ssh extension settings, you would need to check the option for "Lockfiles In Tmp". You can find this option in the menu-bar following:

`Code -> Preferences -> Extensions -> Extension settings - located inside the circular-shaped 'code engine' -> Turn-on the box for "Lockfiles In Tmp"`

------------------------------------------------------------------------

## Training

### How Do I Receive Training?

CRC User Support conducts introductory training for new users all year round.

Periodically throughout the year, CRC organizes scheduled training classes (typically at the beginning of Fall and Spring semesters). These classes are announced on the `new-user-training` page.

Outside of the scheduled training classes, training is provided on-demand by CRC User Support staff. Please send an email to <crcsupport@nd.edu> to arrange a training session.

------------------------------------------------------------------------

## CRC Software

### What Software Is Available At The CRC?

The CRC organizes installed software as `modules`. More information can be found on the `modules` page.

### How Do I Request Help Installing Software?

Many software packages can be installed in your local home directory and we're happy to assist with this process. Please email <crcsupport@nd.edu> with your request and include any information you have about obtaining the installation package for the software you are interested in.

### How Do I Request A Software Module To Be Installed/Updated?

Software that is installed globally through our `modules` tool must undergo license review by the Notre Dame legal department. For this reason, all requests to add a new module must come from Notre Dame faculty. Please submit a [Software Request Form](https://docs.google.com/a/nd.edu/forms/d/e/1FAIpQLSdzy-F9X6wykK1KhRdsT-8QtlcPvs8nwPLid6fnGXD-QAjQCg/viewform) and review the attached CRC Software Policy. The software policy also contains information on licensing and cost sharing with CRC.

> [!NOTE]
> This process can take up to 7 business days from receipt of the software request. If you require an expedited service, please make that apparent on your request form.

### Notes on sudo/root access

Many websites will give instructions for installing software that uses a command called `sudo`. This command gives the user full adminstrator privileges and along with it the ability to make system wide changes, including altering other user's files. On a personal computer this is rarely a problem, but our systems are shared among more than 2000 students and staff. Attempting to run the `sudo` command will result in a message be sent to CRC staff so that we might help you find an alternate solution.

------------------------------------------------------------------------

## AFS Storage

### How Do I Check My Available AFS Storage?

You can check your AFS disk usage with the ''quota'' command e.g.

``` shell
quota
```

### How Do I Request More AFS Storage Space?

Please consult the CRC Storage policy for rules and limitations for user/group storage quotas.

When requesting a storage increase please email <crcsupport@nd.edu> with the following information:

- Name
- Name of PI
- Brief Justification for Increase

### How Do I Give A Particular User/Group Access To A Directory In My Personal AFS Space?

For instructions, see `user_acls`.

### I Am Receiving A "Locking Authority File" Error

If you receive the following error:

``` shell
> /usr/bin/xauth:  error in locking authority file /afs/crc.nd.edu/user...
```

you have most likely exhausted your AFS storage quota. Please delete unwanted files and/or request more storage space.

### How Do I Add and Remove Users From The Shared Group Space I Administer?

To add a user use the following AFS command:

``` shell
pts adduser netid group
```

To remove a user use the following AFS command:

``` shell
pts removeuser netid group
```

### I'm Not Seeing My Output When Running From AFS!

AFS improves I/O performance by caching program output on the local host. The output data is only written to the file server when the program is complete. To overcome this issue, you should add the ''fsync'' command to your submission script. Please see the following wiki page for more details:

`synchronizing_files_afs`

### I Am Getting The Following Message Using Find

``` shell
> find ./ -type d -print 
find: WARNING: Hard link count is wrong for .: this may be a bug in your file system driver. 
```

Use the `-noleaf` option when using the find command:

``` shell
-noleaf
     Do  not  optimize  by  assuming that directories contain 2 fewer subdirectories than their  hard  link count. 
     This option  is needed  when  searching  file systems that do not follow the Unix directory-link convention, 
     such as CD-ROM or MS-DOS file systems or AFS  volume  mount  points.
```

So use the following command instead:

``` shell
find ./ -noleaf -type d -print
```

------------------------------------------------------------------------

## PANASAS Storage (/scratch365)

### How Do I Check My Available (/scratch365) Storage?

You can check your /scratch365 disk usage with the ''pan_df'' command e.g.

``` shell
pan_df -H /scratch365/netid/
```

### How Do I Request More (/scratch365) Storage Space?

Please consult the CRC Storage policy for rules and limitations for user/group storage quotas.

When requesting a storage increase please email <crcsupport@nd.edu> with the following information:

- ''Name''
- ''Name of PI''
- ''Brief Justification for Increase''

------------------------------------------------------------------------

## Jobs

### My Job Is Aborted At Runtime Due To Excessive Disk-Swapping

If your job is aborted at runtime due to disk-swapping, this typically indicates that your simulation is starved of RAM at runtime. A general rule of thumb is to ensure that you \<font color=red\>request 1 core for every 2 GB of RAM\</font\> your simulation requires e.g. if your simulation requires 16GB of RAM in total, then you need to request 8 cores in your UGE submission script (either -pe smp 16 or -pe mpi-16 16), irrespective of whether your simulation actually uses 8 cores. This ensures that 8 cores (and potentially 16GB RAM) are assigned to your job and not accessible by other users.

If unsure how much RAM your job will require, you can use the Xymon Memory Monitoring tool to see how much RAM your job is using. The Xymon Memory Monitoring tool contains two graphs. The graphs corresponds to the '''Physical/Real''' memory and the '''Actual''' memory. In memory management, no actual memory is being set aside whenever a process requests memory, but the memory management unit knows the process is planning on using the memory it requested. This will allow the memory management unit to use the unused memory for other purposes such as caching other data. The '''Physical/Real''' memory represents the amount of RAM the process requested. After the process begins writing to RAM, the memory management unit will begins giving '''actual''' memory to the process so it can write. If the RAM is being occupied by other data, that data is flushed so it can be used by the process. The RAM that the process is actually using represents the '''Actual''' memory graph. So in summary, there should not be a problem if your job's '''Actual''' graph never reaches the '''Physical/Real''' graph. If it does, that means the machine has run out of RAM for your job and that will cause the system to use the '''Hard Disk''' to read/write data. This can cause your job to take longer than expected as well as potentially crash the system your job is running on if it is excessively reading/writing to disk.

### Do Different Operating System Versions Affect Performance?

Each machine uses a version of Red Hat Enterprise Linux, which has various versions available. These different Red Hat builds can have a slight impact on performance.

According to multiple HPL trials run on machine dqcneh014 using a constant N, it appears that RHEL version 7.1 has the slowest performance, while RHEL version 7.2 has a slightly faster performance than the other versions. Other Red Hat versions tested include 6.4, 6.6, and 6.7.

The complete data for this test, and for the test in 7.2, can be found in the following Google Doc: <https://docs.google.com/spreadsheets/d/19c2if441JywnQy4N8-NhMAwDFkmCwzY275hc7vYg9g4/edit?usp=sharing>

### Unable to run job: error: no suitable queues' Message When Submitting Job Script

By default, UGE applies strict validation of resource requests within your submission script including checks for legal queue names and core counts that match valid parallel environments (pe). These checks are very useful in preventing erroneous jobs entering the batch system and immediately falling into (Eqw) error states.

If an invalid resource request is identified by this validation procedure, your submission script will be rejected immediately with the "Unable to run job:" error message.

For example the mpi-8 machines (machines with 8 cores) are either faculty owned (restricted) or on a restricted access list for codes which require an Infiniband interconnect. Standard user scripts will fail when specifying mpi-8. In the case the user should use mpi-12 as the majority of systems available for general access are 12 core machines.

> [!NOTE]
> Scripts that contain the '''mem_free=''' resource request i.e.
>
> > \#\$ -l mem_free=X
>
> will unfortunately always fail the validation test (due to lack of memory information at the time the validation is undertaken). To overcome this situation, please disable the validation process in your submission script by adding the following line:
>
> > \#\$ -w n

### How Can I Monitor The Behavior Of My Running Jobs?

The behavior of running jobs can monitored using the \<font color=red\>Xymon\</font\> online tool. After using `qstat` to identify the server(s) on which your jobs are running, use the Xymon link below to locate the specific server name. Associated with each server is a series of sub-links that you can use to monitor CPU status, memory usage, communication statistics etc.

[CRC Xymon](https://mon.crc.nd.edu/xymon)

### My Job Has Created Zombies...What Does That Mean?

Zombie Processes are processes that still remain running even though your job has terminated within the Grid Engine batch system. There are various reasons why this happens including ungraceful termination of MPI due to either software or hardware issues.

The CRC runs a script that runs periodically to remove these zombie processes from our systems. If zombie processes are detected during the running of this script, due to one of your jobs, you will be notified via email. If you continue to receive zombie notifications regularly please contact CRC Support for assistance in detecting the cause.

------------------------------------------------------------------------

## Applications

### Xquartz gives me a failed request error:

``` shell
X Error of failed request:  BadValue (integer parameter out of range for operation)
  Major opcode of failed request:  149 (GLX)
  Minor opcode of failed request:  24 (X_GLXCreateNewContext)
  Value in failed request:  0x0
  Serial number of failed request:  20
  Current serial number in output stream:  21
```

Open a terminal and enter the following command:

``` shell
defaults write org.macosforge.xquartz.X11 enable_iglx -bool true
```

You will need to restart your Mac.

### I Need To Install ArcGIS On My System

All GIS software installation and GIS desktop support for the Colleges of Science and Engineering will be handled by Engineering and Science Computing support.

You can get the desktop support team contact info for the Colleges of Science and Engineering from the [ESC website](https://esc.nd.edu/) or send email at <help@esc.nd.edu>.

### I am trying to compile my code with mvapich2 but cannot find -lpsm_infinipath

Please compile your code on `crcfeib01.crc.nd.edu`.

### I See Incomplete Images In COMSOL v4+

The OpenGL graphics libraries on our machines are not compatible with the latest versions of Comsol v4+. This results in distortion or incompleteness of rendered images. To overcome this issue please set your COMSOL graphics configuration to use ''software rendering'' via the following menu option:

``` shell
Option -> Preferences > Graphics > Rendering >Software
```

### My Gaussian Jobs Are Being Aborted Due To /tmp Over-usage

By default GAUSSIAN jobs write temporary working data to the /tmp directory of the compute node. For large GAUSSIAN jobs this /tmp space can fill very quickly causing the machine to slowdown/crash ( note: machine supervisors may pre-empt machine crashes by killing your jobs).

To avoid this situation please direct GAUSSIAN to write temporary working files to your own storage allocation (either AFS or /scratch365). Details on how to set this up in your GAUSSIAN scripts can be found `here <gaussian_scratch_space>`.

### Request of ArcGIS Installation and Support

All GIS software installation and GIS desktop support for the Colleges of Science and Engineering will be handled by Engineering and Science Computing support. You can get the desktop support team contact info for the Colleges of Science and Engineering from the [ESC website](https://esc.nd.edu) or send email at <help@esc.nd.edu>.

------------------------------------------------------------------------

## Grants/Proposals

### How Do I Acknowledge the CRC In My Proposals?

To acknowledge collaboration with the CRC, please add the following citation to your proposals:

``` shell
"This research was supported in part by the Notre Dame Center for Research Computing through [CRC resources]."
"[We specifically acknowledge the assistance of]"
```

### Does The CRC Provide An Example Of A Data Management Plan?

Please email <crcsupport@nd.edu> for more information including sample plans.
