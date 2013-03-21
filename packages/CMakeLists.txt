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
add_subdirectory (zlib)

## Package dependencies

add_dependencies (netcdf hdf5 zlib)
add_dependencies (hdf5 zlib)

