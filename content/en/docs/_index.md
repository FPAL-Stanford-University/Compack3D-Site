+++
title= "Documentation & User Guide"
linkTitle= "User Guide"
type = "docs"
weight = 20
no_list = true
[menu.main]
weight = 0
pre = "<i class='fa-solid fa-book'></i>"
+++

## Welcome to Compack3D

Welcome to the *Compack3D* user guide. *Compack3D* is an open-source project developed at Stanford University and supported by the National Science Foundation. *Compack3D* is relased under the [Apache-2.0 License](https://www.apache.org/licenses/LICENSE-2.0). This package provides optimized implementations of essential algorithms for rapid development of numerical solvers for partial differential equations.


## Why do I need it?

*Compack3D* is a light-weight package, originally designed for computational scientists who use compact finite difference methods (generalized Padé schemes) to accelerate their solvers. It solves the tridiagonal and penta-diagonal systems arising from compact numerical schemes on 3D computational mesh. *Compack3D* provides efficient solutions for hiearchical parallelization on distributed memory and shared memory and enables affordable large-scale simulations using compact finite difference methods. *Compack3D* provides both C++ and Fortran APIs, which makes it easy to interface with your own application, even a legacy code.


## How to cite our work
If your project benefits from *Compack3D*, we would appreciate it if you add the following works to your reference list

* Song, H., Matsuno, K. V., West, J. R., Subramaniam, A., Ghate, A. S., & Lele, S. K. (2022). [Scalable parallel linear solver for compact banded systems on heterogeneous architectures](https://doi.org/10.1016/j.jcp.2022.111443). *Journal of Computational Physics*, 468, 111443.
{{< details summary="See BibTex" >}}
~~~tex
@article{song2022scalable,
  title={Scalable parallel linear solver for compact banded systems on heterogeneous architectures},
  author={Song, Hang and Matsuno, Kristen V and West, Jacob R and Subramaniam, Akshay and Ghate, Aditya S and Lele, Sanjiva K},
  journal={Journal of Computational Physics},
  volume={468},
  pages={111443},
  year={2022},
  publisher={Elsevier}
}
~~~
{{< /details >}}

* Song, H., Matsuno, K. V., West, J. R., Subramaniam, A, Ghate, A.S., & Lele, S.K. (2024) [Scalable parallel linear solver for compact banded systems](https://www.nas.nasa.gov/pubs/ams/2024/10-24-24.html). In NASA Advanced Modeling and Simulation (AMS) Seminars.


## Who to talk to?
This project is led by Prof. Sanjiva K. Lele and the core development team consists of
{{< details summary="**Prof. Sanjiva K. Lele**" >}}
* <i class="fa-solid fa-circle-user"></i> Project P.I.
* <i class="fa-solid fa-building"></i> Stanford University
* <i class='fa-solid fa-envelope'></i> lele<i class="fa-light fa-at"></i>stanford.edu
{{< /details >}}
{{< details summary="**Dr. Hang Song**" >}}
* <i class="fa-solid fa-circle-user"></i> Core developer
* <i class="fa-solid fa-building"></i> Stanford University
* <i class='fa-solid fa-envelope'></i> songhang<i class="fa-light fa-at"></i>stanford.edu
{{< /details >}}
{{< details summary="**Andy Wu**" >}}
* <i class="fa-solid fa-circle-user"></i> Developer
* <i class="fa-solid fa-building"></i> Stanford University
* <i class='fa-solid fa-envelope'></i> awu1018<i class="fa-light fa-at"></i>stanford.edu
{{< /details >}}
{{< details summary="**Anjini Chandra**" >}}
* <i class="fa-solid fa-circle-user"></i> Developer
* <i class="fa-solid fa-building"></i> Stanford University
* <i class='fa-solid fa-envelope'></i> achandr3<i class="fa-light fa-at"></i>stanford.edu
{{< /details >}}

