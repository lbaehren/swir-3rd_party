
## ==============================================================================
##
##  Package information
##
## ==============================================================================

set (LIBCONFIG_VERSION        "1.4.9" )
set (LIBCONFIG_SOURCE_ARCHIVE "libconfig-${LIBCONFIG_VERSION}.tar.gz" )
set (LIBCONFIG_URL            "http://www.hyperrealm.com/libconfig/${LIBCONFIG_SOURCE_ARCHIVE}" )
set (LIBCONFIG_SVN            "" )

## ==============================================================================
##
##  Options
##
## ==============================================================================

option (LIBCONFIG_FORCE_BUILD    "Force build and installation of package?" NO )
option (LIBCONFIG_FROM_REPOS     "Install package from repository?"         NO )
option (LIBCONFIG_ENABLE_TESTING "Enable testing and experimental builds?"  YES )

## Handle options

if (LIBCONFIG_ENABLE_TESTING)
  include (CTest)
  enable_testing()
endif (LIBCONFIG_ENABLE_TESTING)

## ==============================================================================
##
##  Build instructions
##
## ==============================================================================

if (NOT LIBCONFIG_FOUND OR LIBCONFIG_FORCE_BUILD)

  ## Locate source file to build from

  find_file (LIBCONFIG_SOURCE ${LIBCONFIG_SOURCE_ARCHIVE}
    PATHS ${CMAKE_CURRENT_SOURCE_DIR}
    )

  if (NOT LIBCONFIG_SOURCE)
    set (LIBCONFIG_SOURCE ${LIBCONFIG_URL})
  endif (NOT LIBCONFIG_SOURCE)

  ## Build package from source

  if (LIBCONFIG_FROM_REPOS)

    ExternalProject_Add (libconfig
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      SVN_REPOSITORY ${LIBCONFIG_SVN}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX}
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  else (LIBCONFIG_FROM_REPOS)

    ExternalProject_Add (libconfig
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      URL ${LIBCONFIG_SOURCE}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX}
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  endif (LIBCONFIG_FROM_REPOS)

else (NOT LIBCONFIG_FOUND OR LIBCONFIG_FORCE_BUILD)

  add_custom_target (libconfig
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "[<package>] Found system-wide installation; skipping rebuild!"
    )

endif (NOT LIBCONFIG_FOUND OR LIBCONFIG_FORCE_BUILD)
