# Apptainer/Singularity

![image](../images/ApptainerLogo.png)

------------------------------------------------------------------------

Apptainer/Singularity is a software application that allow users to have ‘full control’ over their operating system without the need for any ‘super-user’ privileges using the notion of containers or images, the two words are used interchangeably. A container is simply an empty room that the users can configure and customize as it suits their needs. It is intended to provide portability, such that it may be run on any flavor of Linux distribution, and its containers can be shared and shipped easily.

------------------------------------------------------------------------

## Why Use Apptainer

Oftentimes, it is the case that our users would need to use some software packages that we do not have installed in our systems. As a result, they would need to submit a request for software installation. However, with Apptainer, users will be able to configure their own environments in their containers, then ship it to one of our front-end machines and be able to run any software they have installed in it. Hence, this gives the user more convenience and flexibility.

------------------------------------------------------------------------

## Downloading Apptainer

There are multiple ways to download and install Apptainer in your local machines, and they are very well explained at [Apptainer’s main website](https://apptainer.org/docs/user/main/quick_start.html#quick-installation-steps).

Apptainer is already installed on CRC frontends and compute nodes. There is no module, simply run `apptainer --help`.

------------------------------------------------------------------------

## Getting Started

Below is a brief summary of the basics for Apptainer. For a more in depth review, see the [Quick Start Guide](https://apptainer.org/docs/user/main/quick_start.html) and other pages from Apptainer's documentation.

------------------------------------------------------------------------

### Obtaining Containers

Once you have successfully installed Apptainer on your local machine, then there are two ways to get started. One is to use pre-built containers that are available from different sources such as SingularityHub or Docker, and another option is create your own customized containers. Note that you must be on your own machine where you have root / sudo to build a apptainer container. You do not need root to pull a prebuilt container while on CRC resources.

------------------------------------------------------------------------

#### Create Your Own Custom Container

The command `build` is used to create containers. To create your own defined containers, you need to first create a recipe file. A recipe file is used to define a custom container. A basic example recipe file example is:

``` shell
Bootstrap: docker
From: ubuntu:16.04

%post
    apt-get -y update
    apt-get -y install fortune cowsay lolcat

%environment
    export LC_ALL=C
    export PATH=/usr/games:$PATH

%runscript
    fortune | cowsay | lolcat
```

If this recipe file is saved as "ubuntuBuild", a apptainer container can be built with the command:

``` shell
$ sudo apptainer build myOwn.img ubuntuBuild
```

Running the above command will create a container titled "myOwn.img" with the specifications in the build script. Utilizing a recipe file, a container can be created from a starting point such as any container on apptainer hub or any docker container. For more information on recipe files, see [apptainer definition file docs.](https://apptainer.org/docs/user/main/definition_files.html?highlight=definition%20file#definition-files) Apptainer defaults to creating images in SIF format, which is read only. To create a writable image, there are two options. An image could be created as writeable:

``` shell
$ sudo apptainer build --writable myImg.img myrecipeFile
```

Or, an image could be created as a sandbox directory:

``` shell
$ sudo apptainer build --sandbox myImg.img myrecipeFile
```

> [!NOTE]
> Building containers must be done on a machine *not* managed by the CRC as you will need admin access.

------------------------------------------------------------------------

#### Using Pre-Built Containers

To obtain and use a prebuilt container use the `pull` command to apptainer. The `pull` command can be used to obtain containers directly on CRC machines. The below example shows how to obtain a basic CentOS 7.8 container from dockerhub:

``` shell
$ apptainer pull CentOS_7.8.sif docker://centos:centos7.8.2003
```

You can pull images from the Sylabs `Container Library`, `dockerhub`, or `apptainer-hub`. Examples for each are below:

    $ apptainer pull alpine.sif library://alpine:latest
    $ apptainer pull CentOS_7.8.sif docker://centos:centos7.8.2003
    $ apptainer pull apptainer-images.sif shub://vsoch/apptainer-images

Once you pull an image once you do not have to pull it again as it will be stored wherever you tell apptainer to put it. If the container will be stored in AFS, review `apptainer_afs_note`.

For more information see [Apptainer pull documentation](https://apptainer.org/docs/user/main/cli/apptainer_pull.html).

Once you pull a pre-built container you can use it a base to build upon with your own custom definition file or simply use the container as is.

------------------------------------------------------------------------

## Accessing Containers

Once you have a container there are multiple ways you may access/use it:

- [apptainer shell](https://apptainer.org/docs/user/main/cli/apptainer_shell.html) - used to access the container interactively, more like a mini virtual machine. This command is very useful as it will run a shell that would allow you to manipulate and navigate through the container. You may use this command as follows:

``` shell
$ apptainer shell myContainer.img
```

- [apptainer run](https://apptainer.org/docs/user/main/cli/apptainer_run.html) - used to execute the runscript of the container. This requires a runscript definition within the build file of the container. Using the command may be as follows:

``` shell
$ apptainer run myContainer.img
```

- [apptainer exec](https://apptainer.org/docs/user/main/cli/apptainer_exec.html) - used to execute a particular command within the container, and it can be used as follows:

``` shell
$ apptainer exec myContainer.sif /usr/bin/myCommand
```

------------------------------------------------------------------------

## Using Apptainer in Batch Jobs

The first thing you will need to do is to transfer the container/image to your /scratch365 space. To do so, you may execute the following command on the machine that has the container:

``` shell
scp myContainer.img username@crcefe01.crc.nd.edu:/scratch365/username
```

If you're using `mobaxterm` you can transfer the image using the panel on the left.

Once the container is in your space, the apptainer container can be used in our batch systems as follows:

1.  Create a job submission file.
2.  Add the below content to the submission file. Keep in mind to change the content accordingly for your own specific needs.

``` shell
#!/bin/bash
#$ -q debug
#$ -M crcuser@nd.edu
#$ -m abe
#$ -N example-job

apptainer exec path/to/container/myContainer.img runme
```

Where runme is the command being executed within the container. This assumes the container is executable. The script is now ready to be submitted with qsub. Be sure to change the netid, container path, and queue for your own needs.

For further details on how to submit jobs, please visit `submitting_batch_jobs`.

### Apptainer on GPU Nodes

If you plan on using a container with a GPU, you'll need the extra flag `--nv` assuming the use of `run`, `shell`, or `exec`.

The `--nv` flag will setup the basics with CUDA are setup properly for use within a container. For example, a job with a container to run on a GPU could look as:

``` shell
#!/bin/bash
#$ -q gpu
#$ -l gpu=1
#$ -M crcuser@nd.edu
#$ -m abe
#$ -N example-job

export SINGULARITYENV_CUDA_VISIBLE_DEVICES=${CUDA_VISIBLE_DEVICES}

apptainer exec --nv tensorflow_latest-gpu.sif my_scipt.py
```

### Using Apptainer within AFS

Apptainer doesn't play nicely with symlinks, within AFS there is a symlink that sometimes causes obscure transient errors with executing a apptainer container. There are a few hacks which may get around this. If you'd like to use a apptainer container within your AFS space, try adding the flags:

    apptainer shell -B /afs --no-home my_img.simg
    singulairty exec -B /afs --no-home my_img.simg mycommand

The flags `-B /afs --no-home` trick singulairty into remounting AFS within the container. Note that your home space will still be mounted within the container as it lives within afs.

The following error is a transient issue seen from time to time while using apptainer images within AFS:

    failed to mount squashfs filesystem: input/output error

Should you begin to see this error, removing the apptainer cache can solve the issue.

``` shell
rm -rf ~/.apptainer/cache
```

### Using Modules within Apptainer containers

Sometimes your container needs to access a CRC module, if this is the case then you need to bind the path to where modules live in the file system to the proper place with in your container. For this to work, you *cannot* have `/opt/crc` used within your containers.

> [!NOTE]
> For the most part, to use modules within a container the container will need to match the `RHEL` version of the CRC nodes with a compatible `CentOS` container. You can find the RHEL version with `$ cat /etc/redhat-relase` For example:
>
> Bootstrap: docker From: centos:centos7.8.2003 .

To gain access to modules, add `-B /opt/crc` flags when calling apptainer and `APPTAINERENV_PATH=${PATH} APPTAINERENV_LD_LIBRARY_PATH=${LD_LIBRARY_PATH}` *before* calling apptainer as below.

``` shell
#!/bin/bash
#$ -q debug
#$ -M crcuser@nd.edu
#$ -m abe
#$ -N example-job

module load julia gcc

apptainer exec -B /afs /-B /opt/crc --no-home path/to/container/myContainer.img my_command
```

------------------------------------------------------------------------

## Important Notes

There are a few things that our users should keep in mind when using apptainer.

- "To be root **inside** the container, you must be root **outside** the container". In other words, all operations that need a super-user access must be done on a machine where you have **root** access (not the CRC machines).
- All of the apptainer container configuration must be done on a local machine, where the user has root access. Then, they should copy the container to their /scratch365 or AFS space. This is because users will not have root access on our front-end machines. Using prebuilt containers from `dockerhub`, the sylabs `container library`, or `apptainer-hub` will be fine to use, see `Using Pre-Built Containers <using_prebuilt_containers>`.
- It is helpful to double check the permissions of the folders and files on the container that the user intends to manipulate when moved into the cluster. Make sure that needed files and directories have the right permission sets to allow for reading, writing, and executing for all users. This is because once the container is moved to the cluster, the user will lose all super-user privileges.
- It is recommended that you move the containers to /scratch365 space, or copy containers first to /tmp of a front end before moving into AFS space. See `apptainer_afs_note` for more info if you'd like to run out of AFS.

------------------------------------------------------------------------

### Helpful Resources

There is plenty that can be done with these containers and below are a few sources that can be helpful for any additional information:

- <https://apptainer.org/docs/user/main/index.html>
- <https://hub.docker.com/>
- <https://singularityhub.github.io/>
- <https://singularityhub.github.io/singularity-catalog/>

Very well put together tutorial for CSE Grad Student's tutorial meeting by Brian DuSell.

- [Apptainer/Singularity-tutorial](https://github.com/bdusell/singularity-tutorial)
