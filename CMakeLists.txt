
## ==============================================================================
##
##  CMake project settings
##
## ==============================================================================

if (NOT PROJECT_NAME)
  ## Name of the project handled by CMake
  project (SWIR-3rd_paty)
  ## Minimum required version of CMake to configure the project
  cmake_minimum_required (VERSION 2.8.3)
  ## Enforced CMake policy
  cmake_policy (VERSION 2.8.3)
endif (NOT PROJECT_NAME)

## CMake modules
include (ExternalProject)

## ==============================================================================
##
##  Project sub-directories
##
## ==============================================================================

add_subdirectory (cfitsio)
add_subdirectory (doxygen)
add_subdirectory (hdf5)
add_subdirectory (libconfig)
add_subdirectory (netcdf)
