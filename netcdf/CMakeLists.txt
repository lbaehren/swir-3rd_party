
## ==============================================================================
##
##  Package information
##
## ==============================================================================

set (NETCDF_SOURCE_VERSION "4.0" )
set (NETCDF_SOURCE_ARCHIVE "netcdf-${NETCDF_SOURCE_VERSION}.tar.gz" )
set (NETCDF_URL            "http://www.unidata.ucar.edu/downloads/netcdf/ftp/${NETCDF_SOURCE_ARCHIVE}" )
set (NETCDF_SVN            "" )

## ==============================================================================
##
##  Options
##
## ==============================================================================

option (NETCDF_FORCE_BUILD    "Force build and installation of package?" NO )
option (NETCDF_FROM_REPOS     "Install package from repository?"         NO )
option (NETCDF_ENABLE_TESTING "Enable testing and experimental builds?"  YES )

## Handle options

if (NETCDF_ENABLE_TESTING)
  include (CTest)
  enable_testing()
endif (NETCDF_ENABLE_TESTING)

## ==============================================================================
##
##  System inspection
##
## ==============================================================================

#find_package (NETCDF)

## ==============================================================================
##
##  Build instructions
##
## ==============================================================================

if (NOT NETCDF_FOUND OR NETCDF_FORCE_BUILD)

  ## Locate source file to build from

  find_file (NETCDF_SOURCE ${NETCDF_SOURCE_ARCHIVE}
    PATHS ${CMAKE_CURRENT_SOURCE_DIR}
    )

  if (NOT NETCDF_SOURCE)
    set (NETCDF_SOURCE ${NETCDF_URL})
  endif (NOT NETCDF_SOURCE)

  ## Build package from source

  if (NETCDF_FROM_REPOS)

    ExternalProject_Add (netcdf
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      SVN_REPOSITORY ${NETCDF_SVN}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX} --disable-doxygen --disable-f77
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  else (NETCDF_FROM_REPOS)

    ExternalProject_Add (netcdf
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      URL ${NETCDF_SOURCE}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX} --disable-doxygen --disable-f77
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  endif (NETCDF_FROM_REPOS)

else (NOT NETCDF_FOUND OR NETCDF_FORCE_BUILD)

  add_custom_target (netcdf
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "[netcdf] Found system-wide installation; skipping rebuild!"
    )

endif (NOT NETCDF_FOUND OR NETCDF_FORCE_BUILD)
