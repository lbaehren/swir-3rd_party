
## ==============================================================================
##
##  System inspection
##
## ==============================================================================

find_package (CFITSIO)
find_package (Doxygen)
find_package (HDF5)
find_package (LibConfig)
find_package (NetCDF)
find_package (PythonInterp)

## ==============================================================================
##
##  Project sub-directories
##
## ==============================================================================

add_subdirectory (cfitsio)
add_subdirectory (doxygen)
add_subdirectory (h5py)
add_subdirectory (hdf5)
add_subdirectory (kapteyn)
add_subdirectory (libconfig)
add_subdirectory (netcdf)
add_subdirectory (zlib)

## ==============================================================================
##
##  Package dependencies
##
## ==============================================================================

add_dependencies (netcdf hdf5 zlib)
add_dependencies (h5py hdf5)
add_dependencies (hdf5 zlib)

