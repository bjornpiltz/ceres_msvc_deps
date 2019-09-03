[![Build status](https://ci.appveyor.com/api/projects/status/y9aabolrn9hx94kg?svg=true)](https://ci.appveyor.com/project/bjornpiltz/ceres_msvc_deps) 

# MSVC Dependencies for Ceres-Solver
This repository contains binary releases of the third party libraries needed to build Ceres-Solver with Microsoft Visual Studio.

## Content
The following C++ libraries are contained in this package:
* [Eigen](http://eigen.tuxfamily.org)
* [SuiteSparse](http://faculty.cse.tamu.edu/davis/suitesparse.html) (including BLAS/LAPACK)
* [gflags](https://github.com/gflags/gflags)
* [glog](https://github.com/google/glog)

## Visual Studio versions
The following versions of Visual Studio are supported at the moment:
* "Visual Studio 14 2015 Win64"
* "Visual Studio 14 2015"
* "Visual Studio 15 2017 Win64"
* "Visual Studio 15 2017"
* "Visual Studio 15 2019 Win64"

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
  -DEIGEN_SPARSE=ON^
  -DBUILD_SHARED_LIBS=<ON/OFF>^
  -DBUILD_TESTING=<ON/OFF>^
  -DBUILD_EXAMPLES=<ON/OFF>
cmake --build <build_dir> --config Release --target INSTALL  
```
## Enabling SuiteSparse
As of Ceres 1.14, you need to apply the following patch to make the flag -DSUITESPARSE work:
```diff
diff -ruN ceres-solver-1.14.0_pristine/CMakeLists.txt ceres-solver-1.14.0_changed/CMakeLists.txt
--- ceres-solver-1.14.0_pristine/CMakeLists.txt	2018-03-22 05:00:14.000000000 +0100
+++ ceres-solver-1.14.0_changed/CMakeLists.txt	2018-04-12 12:42:11.294191800 +0200
@@ -245,17 +245,8 @@
 endif (EIGEN_FOUND)
 
 if (LAPACK)
-  find_package(LAPACK QUIET)
-  if (LAPACK_FOUND)
-    message("-- Found LAPACK library: ${LAPACK_LIBRARIES}")
-  else (LAPACK_FOUND)
-    message("-- Did not find LAPACK library, disabling LAPACK support.")
-    update_cache_variable(LAPACK OFF)
-    list(APPEND CERES_COMPILE_OPTIONS CERES_NO_LAPACK)
-  endif (LAPACK_FOUND)
-else (LAPACK)
-  message("-- Building without LAPACK.")
-  list(APPEND CERES_COMPILE_OPTIONS CERES_NO_LAPACK)
+    set(SuiteSparse_USE_LAPACK_BLAS ON)
+    message("-- Using LAPACK from SuiteSparse package")
 endif (LAPACK)
 
 if (SUITESPARSE)
@@ -263,7 +254,10 @@
   # built with SuiteSparse support.
 
   # Check for SuiteSparse and dependencies.
-  find_package(SuiteSparse)
+  find_package(SuiteSparse CONFIG)
+  set(SUITESPARSE_FOUND ON)
+  include(${USE_SuiteSparse})
+  list(APPEND SUITESPARSE_LIBRARIES ${SuiteSparse_LIBRARIES})
   if (SUITESPARSE_FOUND)
     # On Ubuntu the system install of SuiteSparse (v3.4.0) up to at least
     # Ubuntu 13.10 cannot be used to link shared libraries.
```
