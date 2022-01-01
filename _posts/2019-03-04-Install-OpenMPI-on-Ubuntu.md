layout: post
title:  "Install OpenMPI on Ubuntu"
date:   2019-03-04 00:00:00 -0800
categories: OpenMPI, Ubuntu
---

An Open MPI installation guide for Ubuntu 16.04

Install necessary packages
```bash
sudo apt-get update
sudo apt-get install openmpi-bin openmpi-common openssh-client openssh-server libopenmpi1.10 libopenmpi-dev
```

## Test installation
Create an empty file named `hello_world.c` and enter codes from [mpi tutorial](http://mpitutorial.com/tutorials/mpi-hello-world/).
```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    // Initialize the MPI environment
    MPI_Init(NULL, NULL);

    // Get the number of processes
    int world_size;
    MPI_Comm_size(MPI_COMM_WORLD, &world_size);

    // Get the rank of the process
    int world_rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

    // Get the name of the processor
    char processor_name[MPI_MAX_PROCESSOR_NAME];
    int name_len;
    MPI_Get_processor_name(processor_name, &name_len);

    // Print off a hello world message
    printf("Hello world from processor %s, rank %d out of %d processors\n",
           processor_name, world_rank, world_size);

    // Finalize the MPI environment.
    MPI_Finalize();
}
```

Compile and execute the code.
```bash
mpicc -o hello_world hello_world.c
./hello_world
```

The output in my computer is
```
Hello world from processor FzUbuntu, rank 0 out of 1 processors
```
