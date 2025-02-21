# Conda

![image](../images/conda.png)

## General Description

Conda is a popular package management system used in machine learning and artificial intelligence research. It is built as a part of Anaconda distribution and provides a useful alternative for the pip package manager. Conda allows users to create many different environments containing different packages without there being any overlap or crossover that may occur when using pip. Each environment may be customized to a specific program's needs and therefore allows for easy package management and access.

!!! warning
    In order to use `conda`, you must be using `bash` as your shell. If you have an account created after May of 2018, your default shell is bash. To check your shell, type: `echo $0`. If your shell is tcsh and you'd like it to be bash, email us at <CRCSupport@nd.edu>.

------------------------------------------------------------------------

## Initial setup

If you have never used Conda on the CRC before or want to start from scratch after removing any existing Conda files, you will likely need to first load the Conda module. After the initial configuration is setup, you will no longer need this step.

``` shell
module load conda 
conda init
source ~/.bashrc
module unload conda
```

To verify that conda was successfully installed and to check the version of conda installed you may run the following.

``` shell
conda info 
```

------------------------------------------------------------------------

## Environment Management

In Conda you are able to create multiple, unique environments. Each environment will be able to be filled with packages specifically suited for various different problems.

To view what environments are already created you may run the following code. ''Note that there will already be a base environment installed.''

``` shell
conda info --envs 
```

To add to this list of environments you may create a new environment by using the following

``` shell
conda create -n ENVNAME 
```

Keep in mind that environments will be installed by default into the envs directory in your conda directory. You can specify a different path if you would like with the `--prefix=` option.

``` shell
conda create --prefix=~/env_name
```

After an environment is created it is essentially dormant until activated. When you activate you are jumping into that environment and therefore will then have access to all the packages associated with that environment. You may activate any of your environments using the following

``` shell
conda activate ENVNAME 
```

If this throws an error, try directly sourcing the activate script within the environments directory.

``` shell
source ~/env_name/bin/activate
```

If you would like to jump out of your currently loaded environment you may use the following command to bring you back to the base environment.

``` shell
conda deactivate 
```

If you would like to create an exact copy of an environment you may use the following

``` shell
conda create -n nameofnewenvironment --clone nameoforiginalenvironment 
```

If you would like to delete an environment you may use the following.

``` shell
conda remove -n ENVNAME --all 
```

------------------------------------------------------------------------

## Package Management

In each environment within conda you may load different packages into the environment. Each package is a different piece of software that you may find to be useful in solving whatever problem you may have.

To view what packages may be available use the following command

``` shell
conda list 
```

If you are looking for a specific package simply use the following search command

``` shell
conda search PKGNAME 
```

The above search only searches among the default channel. However, if you would like to search across all channels then use the following

``` shell
anaconda search PKGNAME 
```

For more specific information about all of the package versions use the following

``` shell
conda search PKGNAME--info 
```

Once you have found the name of the package you want to install you may install use the following code to install it into an environment.

``` shell
conda install -n ENVNAME PKGNAME 
```

!!! note
    If you omit the "-n ENVNAME" portion of code the package will be installed in your current environment. All installs must be executed in a specific conda environment, not the base environment. This means that (ENVNAME) must appear to the left of your [username]. This ensures that no base packages are uninstalled, for example pip or python.

When you need to update one of your packages you may use the following update command.

``` shell
conda update PKGNAME 
```

If you need to update all of your packages in your currently loaded environment simply use the following

``` shell
conda --update-all 
```

If you would like to delete a package from your environment you may do so with the following uninstall command

``` shell
conda uninstall PKGNAME 
```

------------------------------------------------------------------------

## Python Management

Many times when using conda for machine learning applications we will be using python. To look for the specific versions of python available for install use the following

``` shell
conda search -f python 
```

Once you have found the specific version of python you want to install you may use the following install command to specify which python you need.

``` shell
conda install -n ENVNAME python=3.4 
```

To verify which version of python your current environment is using, use the following

``` shell
python --version 
```

------------------------------------------------------------------------

## Channel Management

There are several main channels available for use in Anaconda. These include, anaconda, conda-forge, r, and bioconda. Each channel contains different packages that may be installed into your environment. None of these channels are more important than another, but instead are there for organization of packages. By default, all users are on the default channel. If there is a specific package that you are looking to install that is not available on this default channel, then you can search for that package on another channel.

For example, the bioconda channel is a Conda channel that provides bioinformatic packages. If we wanted to switch to the bioconda channel and install a bioconda specific package we would do so using the following

``` shell
conda config --add channels bioconda
conda config --set channel_priority strict 
```

When adding this channel using the "add" command we are telling Conda to add the channel at the top, or highest priority of the channels accessible to our manager. The order of channels in your Conda matters due to the potential of channel collisions. To circumvent this issue and ensure that we do not encounter any channel collisions from duplicate packages we use the second line of code above. This ensures that all of the dependencies come from the bioconda channel as opposed to the default channel.

If we wanted to set the priority back to our default channel we would have to edit the ~/.condarc file so that defaults is the first channel shown. The ~/.condarc file is only created upon creation of a new channel.

``` shell
channels:
defaults
bioconda 
```

Then we must run these lines of code in the command-line

``` shell
conda config --set channel_priority true
conda update --all
```

This will allow us to make changes to our default channel, without interfering with the previously made changes in the bioconda channel.

We would recommend only using the default channel unless absolutely necessary, in order to ensure no channel collisions. If you only need one or two packages from another channel that may not exist in the default channel then you should use the following

``` shell
conda config --append channels bioconda 
```

This will push the bioconda channel to the bottom of the priority list, ensuring that there are no conflicts in dependencies with the default channel.

Once the desired channel is installed we must use the following to install a package from that specific package

``` shell
conda install bioconda::PKGNAME 
```

------------------------------------------------------------------------

## Job Submission Using Conda Environments

Since your environments are saved in a unique file path on your node all of the packages will already be installed in the referenced environment, allowing you to customize your environment before submitting your job. Once the job is submitted it will be referencing the packages in your environment, meaning that you don't need to redo any of your previous installations.

To load your environment use the following code, keeping in mind that the first three lines are example job submission code. Everything after those initial three lines will be as if you are running the same code in your node.

!!! note
    Unlike many software packages at the CRC, you generally should never load the conda module in your job scripts or configuration files after the initial configuration is complete.

``` shell
#!/bin/bash
#$ -q long
#$ -pe smp 1
#$ -N jobname

conda activate ENVNAME
python3 example.py 
```

This will run your python code in the environment that you specify, allowing you access to whatever packages you have previously loaded. If you would like to run multiple jobs at once then you should refer to the `submitting_batch_jobs` page.
