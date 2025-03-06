+++
title= "Installation"
linkTitle= "Installation"
type = "docs"
weight = 10
+++

## Prerequisites

*Compack3D* is implemented in C++ using the C++20 standard. The parallelization follows the "MPI+X" paradiam. The following packages are required:

* C++ compiler
* "X" compiler for CUDA or HIP
* CMake (minimum version 3.23)
* Message Passing Interface (MPI)

{{% alert %}}
The HIP feature is still under development.
The package will be updated soon.
{{% /alert %}}


## Download *Compack3D*

The source code of [*Compack3D*](https://github.com/songhanglucky/Compack3D/tree/master) is hosted on GitHub.
It is highly recommended to obtain the source code using Git.
In order to do this, navigate to the target directory where the *Compack3D* is expected to be placed.
```bash
git clone git@github.com:songhanglucky/Compack3D.git
```


## Configuration using CMake
*Compack3D* provids CMake support to build the project.
CMake v3.23 or above is required.
To build *Compack3D*, first navigate into the *Compack3D* root directory `<path/to>/Compack3D/`.
Then create a build directory and navigate into it.
```bash
mkdir build && cd build
```
Make sure to load all necessary modules, e.g., compilers, CMake, MPI.
```bash
cmake -D<optoinal_var_0>=<val_0> -D<optional_var_1>=<val_1> ..
```
For more detailed usage of CMake, please refer to the [official CMake documentation](https://cmake.org/documentation/)
Available optional variables are listed in the following table.
| Variable Name     | Type |      Explanation      |
|-------------------|------|-----------------------|
|`CMAKE_BUILD_TYPE` | `STRING` | Valid values are `Debug`, `Release`, `Profiling`, or `Help`. The `Debug` mode enables safe launch check and does not enable compiler optimization. Default value is `Release`.|
|`CMAKE_INSTALL_PREFIX`| `STRING` | A valid path with `rwx` permission. Install root directory. Default is `./`|
|`COMPACK3D_ENABLE_DEVICE_{TYPE}`| `BOOL` | Default is `OFF`. Valid of `{TYPE}` is `CUDA` or `HIP`. Different devices cannot be enabled in one build.|
|`COMPACK3D_DEVICE_ARCH`| `STRING` | Specification of the device architecture. Needs to be consistent with the specified device type. Valid values are `Default`, `V100`.|
|`COMPACK3D_ENABLE_DEVICE_AWARE_COMM`| `BOOL` | If enabled, use GPU-to-GPU communication.|
|`COMPACK3D_BUILD_UNIT_TESTS`| `BOOL` | Compile unit tests associated with the specified device. |
|`COMPACK3D_BUILD_BENCHMARK_TESTS`|`BOOL`| Compile benchmark tests associated with the specified device.|
|`COMPACK3D_MPI_EXEC_PREFIX`| `STRING` | MPI launcher and flags, required for unit tests. This value can be alternatively set by environmental variable `COMPACK3D_MPI_EXEC_PREFIX`. Default value is `mpiexec -n 1`, and a warning will be rased.|


## Linking to your own project
We working on it...
