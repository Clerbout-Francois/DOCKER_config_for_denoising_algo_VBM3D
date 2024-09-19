# Prepare your Docker to use the denoising algorithm VBM3D

I'll try to guide you, step by step, through the VBM3D algorithm utilisation in Docker. The complete source code is available [here](https://github.com/tehret/vbm3d).

<a name="table_of_contents"/>

## Table of Contents
1. [VBM3D Overview](#overview_)
2. [Introduction](#introduction_)
3. [Docker Installation](#docker_)
4. [Prerequisites](#prerequisites_)
5. [Docker Configuration](#dockerfile_)
6. [VBM3D Execution](#execution_)



<a name="overview_"/>

## VBM3D Overview

This source code provides an implementation of VBM3D developped in "Dabov, Kostadin, Alessandro Foi, and Karen Egiazarian. "*Video denoising by sparse 3D transform-domain collaborative filtering.*" 2007 15th European Signal Processing Conference. IEEE, 2007".

This code is part of an [IPOL](https://www.ipol.im/) publication. Please cite it if you use this code as part of your research ([link](https://www.ipol.im/pub/art/2021/340/)).

It is based on Marc Lebrun's code for the image denoising version of this algorithm (BM3D) available on the [BM3D IPOL page](https://www.ipol.im/pub/art/2012/l-bm3d/). It also uses the TVL1 optical flow from IPOL ([here](https://www.ipol.im/pub/art/2013/26/), available in the folder `tvl1flow`) and the multiscale from [here](https://github.com/npd/multiscaler) (in the folder `multiscale`).

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

You will need the following packages/libraries: **FFTW3**, **OpenMP**, **libpng**, **libtiff** and **libjpeg**.

You'll need to install them. To do this, you can copy the following command lines:

```
apt-get update
apt-get install build-essential
apt-get install fftw3-dev
apt-get install libomp-dev 
apt-get install libpng-dev 
apt-get install libtiff5-dev   
apt-get install libjpeg-dev 
```

[Table of Contents](#table_of_contents)
<a name="dockerfile_"/>

## Docker Configuration

### Clone the VBM3D repository

Open a terminal and clone the repository with the following command lines:

```
git clone https://github.com/tehret/vbm3d.git
cd vbm3d
```

### Create a Dockerfile

Inside the vbm3d directory (possible thanks to the previous command `cd vbm3d`), create a `Dockerfile` with the following content:

```
# Use a suitable base image
FROM ubuntu:20.04

# Set environment variables to avoid prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Update package list and install necessary packages
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    libpng-dev \
    libtiff5-dev  \
    libjpeg-dev \
    fftw3-dev \
    libomp-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /vbm3d

# Copy the repository files into the container
COPY . .

# Build the project
RUN mkdir build && cd build && cmake .. && make

# Set the entry point (optional)
CMD ["bash"]
```

### Build the Docker Image

In the Docker terminal, run the following command in the `vbm3d` directory to build the Docker image:

```
docker build -t vbm3d .
```

### Run the Docker Container

After the image is built, you can run the container with the following command :

```
docker run -it --rm vbm3d
```

This gives you a shell inside the container.
Now you should be inside the container's shell, and you can run your tests or execute the built binaries.

[Table of Contents](#table_of_contents)
<a name="execution_"/>

## VBM3D Execution

Now, you just need to follow the *USAGE* section of [this repository](https://github.com/tehret/vbm3d/blob/master/README.md).

Be sure to be in the `build\bin` directory.

You can check if all is deployed correctly, with the following line:

```
./VBM3Ddenoising --help
```

If the command line returns the following lines, you can use the algo.

```
    -i       =              : < Input sequence
    -b       =              : < Input basic sequence (replacing first pass)
    -nisy    =              : > Noisy sequence (only when -add is true)
    -deno    = deno_%03d.tiff : > Denoised sequence
    -bsic    = bsic_%03d.tiff : > Basic denoised sequence
    -diff    =              : > Difference sequence between noisy and output (only when -add is true)     
    -diffi   =              : > Difference sequence between input and output
    -meas    = measure.txt  : > Text file with PSNR/RMSE (only when -add is true)
    -fflow   =              : < Forward optical flow 
    -bflow   =              : < Backward optical flow 
    -f       = 0            : < Index of the first frame
    -l       = 0            : < Index of the last frame
    -s       = 1            : < Frame step
    -sigma   = 0            : < Noise standard deviation
    -add     = true         : < Add noise
    -verbose = true         : > Verbose output
    -kHard   = -1           : < Spatial size of the patch (first pass)
    -ktHard  = -1           : < Temporal size of the patch (first pass)
    -NfHard  = -1           : < Number frames used before and after the reference (first pass)
    -NsHard  = -1           : < Size of the search region in the reference frame (first pass)
    -NprHard = -1           : < Size of the search region in the other frames (first pass)
    -NbHard  = -1           : < Maximum number of neighbors per frame (first pass)
    -pHard   = -1           : < Step between each patch (first pass)
    -NHard   = -1           : < Maximum number of neighbors (first pass)
    -dHard   = -1           : < Bias toward center patches (first pass)
    -tauHard = -1           : < Distance threshold on neighbors (first pass)
    -lambda3d = -1           : < Coefficient threhsold (first pass)
    -T2dh    = -1           : < 2D transform (first pass), choice is 1 (dct) or 2 (bior)
    -T2dw    = -1           : < 2D transform (second pass), choice is 0 (dct) or 1 (bior)
    -T3dw    = -1           : < 1D transform (first pass), choice is 2 (hadamard) or 3 (haar)
    -color   = true         : < Transform the color space during the processing
    -mc      = false        : < Motion compensation for 3d patches
./VBM3Ddenoising: no input images.
```

This setup should allow you to build and run the `vbm3d` project within a Docker container on your Windows machine. If you encounter any issues or need further customization, feel free to ask!

**Have fun with Docker, new possibilities are opening up...**
