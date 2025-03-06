+++
title= "Usage"
linkTitle= "Usage"
type = "docs"
weight = 100
+++

*Compack3D* is mostly implemented in C++, and it also provides Fortran interfaces using `ISO_C_BINDING`.
In order to improve the performance, the tridiagonal and penta-diagonal solvers in *Compack3D* prefactorize the linear systems.
Buffer for distributed communication is also required.
Depending on the system size, grid decomposition strategy, device that supports the solution process, and communication pattern,
the allocation of the buffer variables is application specific.
Additionally, the buffer may be able to reused for solving different linear systems on the same computational grid.
*Compack3D* supports customized memory allocation and management.
It also provides built-in memory allocators and deallocators.
The tridiagonal and pental-diagonal solvers in *Compack3D* can be used in either pure function calls or object member function calls.
From the perspective of simplifying the memory management, object member function calls are recommended.


## Namespace (`::cmpk`)
For C++ programming interface, *Compack3D* is protected by the namesapce `::cmpk`.
{{% alert %}}
In the following context, the base namespace, `::cmpk`, is assumed.
{{% /alert %}}



## Memory space
The concept of memory space is used in *Compack3D* to indicate the locality of the data and instruct the kernel launching.
*Compack3D* defines two types of memory space 
* `MemSpace::Host` host memory
* `MemSpace::Device` device memory

The host memory (`MemSpace::Host`) is guaranteed to be accessbile by a CPU thread.
If a GPU is specified as the device, the device memory space (`MemSpace::Device`) will be the GPU memory.
Otherwise, the device memory is the same as the host memory as degeneration.
Nevertheless, using CPU as the device indicates that a "kernel" will be launched on CPU with shared-memory parallelism, the execution will be mapped to OpenMP.



## High-level APIs
*Compack3D* provides high-level application programming interfaces (APIs).
The solvers are wrapped into C++ templated classes.
The data locality and factorization coefficients are automatically managed internally throughout the life time of a solver object.
For efficient memory usage, memory buffers for solvers on the same computational mesh and mesh partition strategy can be shared.
In order to use the high-level APIs, the header file `Compack3D_api.h` is required.
Some utility functions are provided for memory management are provided.
These utility functions allows the user to have single implementation for various runtime architectures.

### Memory management
The memory allocation and deallocations are managed by `memAllocArray` and `memFreeArray`, respsectively.
The signatures are provided below:
~~~c++
template<typename MemSpaceType, typename DataType>
void memAllocArray(DataType** ptr_addr, const unsigned int count);
~~~
and
~~~c++
template<typename MemSpaceType, typename DataType>
void memFreeArray(DataType* data);
~~~
where `MemSpacetype` can be either `MemSpace::Host` or `MemSpace:Device`;
and `memFreeArray` only requires the value of the pointer pointing to the allocated memory address.
`memAllocArray` takes the address of a pointer as `DataType**` and a count of memory in the group of `sizeof(DataType)` as `unsigned int`.
`memAllocArray` and `memFreeArray` only take raw pointers, which do not assume any higher-level data structures.
For a multi-dimensional array, the count is the total number of entries including the padded entries.
{{% alert %}}
To reduce the chance of causing memory leak, `memAllocArray` requires that `*ptr_addr` must be a null pointer, `nullptr`.
Otherwise, a runtime error will be thrown.
{{% /alert %}}


### Deep copy
Deep copy of an array can be conducted using the built-in utility functions `deepCopy`.
`deepCopy` automatically maps to a proper method for safe deep copy of data from host to host, from device to device, or between host and device.
The signature of `deepCopy` is
~~~c++
template<typename DstMemSpaceType, typename SrcMemSpaceType, typename DataType>
void deepCopy(DataType* dst, DataType* src, const unsigned int count);
~~~
The type arguments `DstMemSpaceType` and `SrcMemSpaceType` represent the destination and source memory space types, respectively.
The valid types are `MemSpace::Host` and `MemSpace::Device`.
`DataType` is an atomic data type of the arrays.
The function arguments `dst` and `src` are the raw pointers of the destination and source buffer respectively. `count` is the count of `DataType` entries.



### Solver APIs
The tridiagonal and penta-diagonal solvers operated on a 3D partitioned computataional mesh are:
* `DistTriSolDimI` -- solving a tridiagonal system along the dimension `i`.
* `DistTriSolDimJ` -- solving a tridiagonal system along the dimension `j`.
* `DistTriSolDimK` -- solving a tridiagonal system along the dimension `k`.
* `DistPentaSolDimI` -- solving a penta-diagonal system along the dimension `i`.
* `DistPentaSolDimJ` -- solving a penta-diagonal system along the dimension `j`.
* `DistPentaSolDimK` -- solving a penta-diagonal system along the dimension `k`.

{{% alert %}}
The convention of a 3D dimensional array index is (`i`, `j`, `k`), which holds for both C- and Fortran-order memory layouts.
For the C-order layout, the index `k` maps to the contiguous memory access,
while for the Fortran-order layout, the index `i` maps to the contiguous memory access.
{{% /alert %}}

For simplicity, denote `DimX` be an arbitrary dimension of {`DimI`, `DimJ`, `DimK`}.

#### Construction
All the listed solvers do not support default construction.
The signatures of the tridiagonal and penta-diagoanal solves are:
~~~c++
template<typename RealType, typename RealTypeComm, typename MemSpaceType>
DistTriSolDimX<RealType, RealTypeComm, MemSpaceType>::DistTriSolDimX(
        const unsigned int num_grids_i,
        const unsigned int num_grids_j,
        const unsigned int num_grids_k,
        const unsigned int arr_stride_i,
        const unsigned int arr_stride_j,
        MPI_Comm mpi_comm
)
~~~
~~~c++
template<typename RealType, typename RealTypeComm, typename MemSpaceType>
DistPentaSolDimX<RealType, RealTypeComm, MemSpaceType>::DistPentaSolDimX(
        const unsigned int num_grids_i,
        const unsigned int num_grids_j,
        const unsigned int num_grids_k,
        const unsigned int arr_stride_i,
        const unsigned int arr_stride_j,
        MPI_Comm mpi_comm
)
~~~
The construction signature contains the description of the decomposed computational mesh in the local distributed memory partition.
The template argument `RealType` represents the atomic floating-point (FP) type.
Currently supported types are FP64 and FP32.
The communication between distributed memory partitions includes mixed-precision operations.
The template argument `RealTypeComm` indicates the reduced-precision FP type.
If `RealTypeComm` is same as `RealType`, then no mixed-precision operation will be performed.
`RealTypeComm` is an optional argument, which is default to `RealType`.

{{% alert %}}
The precision of `RealTypeComm` must be less or equal to that of `RealType`.
Otherwise, a compile error will be raised.
The mixed-precision operation can, though not is not guaranteed to, result in the higher precision accuracy.
The accuracy depends on the runtime computational mesh size, the mesh partitioning strategy, as well as the linear system itself.
The development is working on the runtime check feature to estimate the magnitude of the final round-off error.
{{% /alert %}}

The template argument `MemSpaceType` indicates the data locality.
Valid types are `MemSpace::Host` and `MemSpace::Device`.
Here the data is referred to the input and output during the solution process.
The `MemSpaceType` must be consistent with the locality of the shared buffer.
The allocation of all internal buffer is guaranteed to be consistent with the specified `MemSpaceType`.

The arguments `num_grid_i`, `num_grid_j`, and `num_grid_k`, are number of grid points in the current distributed-memory partition.
These numbers only count for valid grid points. Do not count any padded entries.
The arguments `arr_stride_i` and `arr_stride_j` are the memory strides in terms of counts of entries on the linear allocated memory.
These arguments provide instructions of accessing the 3D array from (`i`, `j`, `k`) to (`i+1`, `j`, `k`) and  from (`i`, `j`, `k`) to (`i`, `j+1`, `k`), respectively.

The argument `mpi_comm` is an MPI sub-communicator that only contains the partitions involved in an individual solution process.
This means that the MPI sub-communicator should only contain the pencil of partitions in along the solve dimension.
See [the figure of schematics](/docs/algorithms/#overview) for more details.
{{% alert %}}
The ranks assigned by `mpi_comm` must be continuous intergers starting from `0`.
The solution process uses the ranks to determine the data locality in different distributed memory partitions.
{{% /alert %}}








## Low-level APIs (use with caution)
All low-level APIs in *Compack3D* are included in the high-level APIs.
The high-level APIs provide extensive flexibility for use the solvers in various scenarios.
Although, the low-level APIs offers much more flexibility, it is still highly recommended to use the high-level APIs if sufficient.
Again, it is highly recommended to use the low-level APIs only when it is necessary.

If you have carefully read the previous paragraph still decide to use the low-level APIs, the instructions are listed as follows.
The possibly needed header files are listed below:
* `Compack3D_util.h` constains all the utility functions
* `Compack3D_tri.h` contains all the low-level functions for the tridiagonal solvers
* `Compack3D_penta.h` contains all the low-level functions for the penta-diagonal solvers

The low-level functions for the tridiagonal and penta-diagonal solvers are protected within the namespace `::cmpk::tri` and `::cmpk::penta`, respectively.

{{% alert %}}
This section is still under construction and will be updated soon.
{{% /alert %}}

