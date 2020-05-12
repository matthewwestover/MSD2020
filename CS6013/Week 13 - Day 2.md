## Week 13 - Day 2
### GPU Architecture and CUDA
GPUs started as dedicated processors for Graphics and Gaming  
Their core goals:

* Must be cheap enough for home use
* Speed more important than accuracy

Consequences

* Graphics-specific design (names like “texture memory”, “pixel processor”)
* Years before double precision floating point supported.
* Still possible to buy low double precision devices
* Custom hardware for trig functions, not very accurate

Researchers realized GPUs could be used as a “low cost” parallel computer  
Research on mixed precision computation (single & double)  
Math libraries for GPUs  
Gradual improvement in double precision performance  
Introduction of CUDA programming language  
Architectural generalization, such as data caches  
Support for GPU Offload in OpenMP  

### CUDA
Compute Unified Device Architecture  
Data-parallel programming interface to GPU

* Data to be operated on is discretized into independent partition of memory
* Each thread performs roughly same computation to different partition of data
* When appropriate, easy to express and very efficient parallelization

Programmer expresses

* Thread programs to be launched on GPU, and how to launch
* Data placement and movement between host and GPU
* Synchronization, memory management, testing, ...

CUDA is one of first to support heterogeneous architectures  
CUDA environment: Compiler, run-time utilities, libraries, emulation, performance