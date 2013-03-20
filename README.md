# SWIR calibration software 3rd-party components #

As it turns out some of the external package dependencies for the SWIR
calibration software cannot be resolved through the package manager on
the given platform, such that 3rd-party components need to be installed
by hand. In order to simplify this process and provide better control
over the outcome, this package offers a mechanism to retrieve, configure,
build and install these external dependencies.

## Checking out a working copy ##

In order to retrieve a working copy or clone into a local bare Git repository:

~~~~
  git clone git@github.com:lbaehren/swir-3rd_party.git [local name]
  git clone --bare git@github.com:lbaehren/swir-3rd_party.git [local name]
~~~~

## Organization of files and directories ##

After checking out a working copy, you will be left with the following directory
structure:

    .
    |-- CMakeLists.txt        ...  Top-level CMake configuration script
    |-- cfitsio
    |-- doxygen
    |--  hdf5
    |-- libconfig
    `-- netcdf

## Dependencies ##

| Name  | Version  | Relevance | Notes                                  |
|-------|----------|-----------|----------------------------------------|
| Git   | > 1.7    | mandatory | Distributed revision control system.   |
| CMake | >= 2.8.3 | mandatory | Cross-platform build system generator. |


## Configuration and build ##

The software uses the CMake (www.cmake.org) cross-platform makefile generator in
order to configure for building library, tools and test programs.

It is advised to perform an out-of-source build, in order to prevent polluting
the source directories:

    mkdir build
    cd build
    cmake [OPTIONS] ..

If the configuration completed succesfully, in order to build all available
components simply type (the optional `-j` option is for parallel builds):

    make [-j]

Configuration options typically used are:

 * `-DCMAKE_BUILD_TYPE=Debug` -- build type: `Debug` or `Release`
 * `-DCMAKE_INSTALL_PREFIX=<path>` -- user defined installation prefix.

In order to build an individual target/component, pick one of the items on the
list return with

    make help


## Installation ##

Once the build has completed you can install the software: to do this run

    make install

such that header files, libraries, program executables and documentation will be
placed into the appropriate locations.
