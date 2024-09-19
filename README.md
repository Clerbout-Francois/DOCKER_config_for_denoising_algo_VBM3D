# Prepare your Docker to use the denoising algorithm VBM3D

I'll try to guide you, step by step, through the VBM3D algorithm utilisation in Docker. The complete source code is available [here](https://github.com/tehret/vbm3d).

<a name="table_of_contents"/>

## Table of Contents
1. [VBM3D Overview](#overview_)
2. [Introduction](#introduction_)
3. [Docker Installation](#docker_)
4. [Prerequisites](#prerequisites_)
5. [Dockerfile Edition](#dockerfile_) 
6. [Configure the DHCP server](#DHCP_)



<a name="overview_"/>

## VBM3D Overview

This source code provides an implementation of VBM3D developped in "Dabov, Kostadin, Alessandro Foi, and Karen Egiazarian. "*Video denoising by sparse 3D transform-domain collaborative filtering.*" 2007 15th European Signal Processing Conference. IEEE, 2007".

This code is part of an [IPOL](https://www.ipol.im/) publication. Plase cite it if you use this code as part of your research [link](https://www.ipol.im/pub/art/2021/340/).

It is based on Marc Lebrun's code for the image denoising version of this algorithm (BM3D) available on the [BM3D IPOL page](https://www.ipol.im/pub/art/2012/l-bm3d/). It also uses the TVL1 optical flow from IPOL ([here](https://www.ipol.im/pub/art/2013/26/), available in the folder 'tvl1flow') and the multiscale from [here](https://github.com/npd/multiscaler) (in the folder 'multiscale').

[Table of Contents](#table_of_contents)
<a name="introduction_"/>

## Introduction

Since you're on a Windows machine and the code is compilable on Unix/Linux, you'll need Docker to use it. To build the VBM3D repository on a Windows machine using Docker, you’ll need to create a Dockerfile that sets up the environment required for the project...don't worry, I'll guide you through the various stages.

[Table of Contents](#table_of_contents)
<a name="docker_"/>

## Docker Installation

### Install Docker Desktop on your Windows machine.

You will need to install Docker Desktop, you can do so by downloading the installer [here](https://www.docker.com/products/docker-desktop/).

### Make sure Docker is running.

To check if Docker is installed correctly and running with the right permissions, run:

```
docker run hello-world
```

If this command works, Docker is set up correctly.

[Table of Contents](#table_of_contents)
<a name="prerequisites_"/>

## Prerequisites

### Packages

You will need the following packages/libraries: **FFTW3**, **OpenMP**, **libpng**, **libtiff** and **libjpeg**.

You'll need to install them. To do this, you can copy the following command lines:

'''
apt-get update
apt-get install build-essential
apt-get install fftw3-dev
apt-get install libomp-dev 
apt-get install libpng-dev 
apt-get install libtiff5-dev   
apt-get install libjpeg-dev 
'''


