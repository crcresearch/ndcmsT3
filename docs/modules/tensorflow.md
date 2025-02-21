# Tensorflow

## General Description

TensorFlow™ is an open source software library for high performance numerical computation. Its flexible architecture allows easy deployment of computation across a variety of platforms (CPUs and GPUs), and from desktops to clusters of servers to mobile and edge devices. Originally developed by researchers and engineers from the Google Brain team within Google’s AI organization, it comes with strong support for machine learning and deep learning and the flexible numerical computation core is used across many other scientific domains.

------------------------------------------------------------------------

## Basic Usage

CRC version of Tensorflow support both GPU and CPU. The following is an example of UGE job submission script requesting 1 GPU

``` shell
#!/bin/bash

#$ -M netid@nd.edu      # Email address for job notification
#$ -m abe               # Send mail when job begins, ends and aborts
#$ -q gpu               # Specify queue
#$ -l gpu_card=1        # Specify number of GPU cards to use.
#$ -N TFjob             # Specify job name

module load tensorflow

python3  pythonscript.py
```

------------------------------------------------------------------------

## Tensorflow Python

As you can see above, no python module manually loaded for a tensorflow job. The tensorflow module itself will bring it's own version of python with several necessary packages preconfigured to use the CRC infrastructure.

``` shell
$ module list
Currently Loaded Modulefiles:
 1) CRC_default/1.1
$ python3 --version
Python 3.6.8
$ module load tensorflow
$ python3 --version
Python 3.9.5
```

------------------------------------------------------------------------

### Installing Python Packages Locally

If a python package is not installed with the CRC version of Tensorflow, you can easily install it locally in your personal AFS space using pip.

``` shell
module load tensorflow

pip3 install --user package_name
```
