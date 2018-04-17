[![Build status](https://ci.appveyor.com/api/projects/status/y9aabolrn9hx94kg?svg=true)](https://ci.appveyor.com/project/bjornpiltz/ceres_msvc_deps) 

# MSVC Dependencies for Ceres-Solver
This repository contains binary releases of the third party libraries needed to build Ceres-Solver with Microsoft Visual Studio.

## Content
The following C++ libraries are contained in this package:
* [Eigen](http://eigen.tuxfamily.org)
* [SuiteSparse](http://faculty.cse.tamu.edu/davis/suitesparse.html) (including BLAS/LAPACK)
* [gflags](https://github.com/gflags/gflags)
* [glog](https://github.com/google/glog)

## Download
Grab the latest release at [the releases page](https://github.com/bjornpiltz/ceres_msvc_deps/releases)

## Usage
If you download and extract this package to the directory `ceres_msvc_deps_2017_x64`, you can compile Ceres like this:
```batch
cmake^
  -G"Visual Studio 15 2017 Win64"^
  -H<ceres_src_dir>
  -B<build_dir>^
  -DCMAKE_INSTALL_PREFIX=<install_dir>^
  -DCMAKE_PREFIX_PATH=<ceres_msvc_deps_2017_x64>^
cmake --build <build_dir> --config Release --target INSTALL  
```

## Visual Studio versions
The following versions of Visual Studio are supported at the moment:
* "Visual Studio 14 2015 Win64"
* "Visual Studio 14 2015"
* "Visual Studio 15 2017 Win64"
* "Visual Studio 15 2017"
