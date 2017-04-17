# Docker image for Intel MPI

Goal: We want a lightweight runtime image and a basic build image that will work with MS Azure RDMA network nodes.

You need the Intel MPI library from (registration required) https://software.intel.com/en-us/intel-mpi-library/

Starting point is the Azure Batch Shipyard (ABS) [recipe for HPCG](https://github.com/Azure/batch-shipyard/tree/master/recipes/HPCG-Infiniband-IntelMPI). Note this image doesn't have compilers etc so is possibly a good fit.

I'm going to start with a Dockerfile, but I suspect that [Packer](https://www.packer.io/docs/builders/docker.html) may end up being more suitable.

## Centos based

The ABS recipe is based on Centos 7, so we will try that first.

Note that the GCC provided by Centos 7 is ancient, so we use SCL to get a recent one. [This Dockerfile](https://github.com/sclorg/devtoolset-container/blob/master/4-toolchain/Dockerfile) provides some inspiration for setting up the environment.
