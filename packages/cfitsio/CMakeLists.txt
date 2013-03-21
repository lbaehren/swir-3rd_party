
#--------------------------------------------------------------------------------
# Copyright (c) 2012-2013, Lars Baehren <lbaehren@gmail.com>
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without modification,
# are permitted provided that the following conditions are met:
#
#  * Redistributions of source code must retain the above copyright notice, this
#    list of conditions and the following disclaimer.
#  * Redistributions in binary form must reproduce the above copyright notice,
#    this list of conditions and the following disclaimer in the documentation
#    and/or other materials provided with the distribution.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
# FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
# DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
# SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
# CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
# OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
#--------------------------------------------------------------------------------

## ==============================================================================
##
##  Package information
##
## ==============================================================================

set (CFITSIO_SOURCE_VERSION "3330" )
set (CFITSIO_SOURCE_ARCHIVE "cfitsio${CFITSIO_SOURCE_VERSION}.tar.gz" )
set (CFITSIO_URL            "ftp://heasarc.gsfc.nasa.gov/software/fitsio/c/${CFITSIO_SOURCE_ARCHIVE}" )
set (CFITSIO_SVN            "" )

## ==============================================================================
##
##  Options
##
## ==============================================================================

option (CFITSIO_FORCE_BUILD    "Force build and installation of package?" NO )
option (CFITSIO_FROM_REPOS     "Install package from repository?"         NO )
option (CFITSIO_ENABLE_TESTING "Enable testing and experimental builds?"  YES )

## Handle options

if (CFITSIO_ENABLE_TESTING)
  include (CTest)
  enable_testing()
endif (CFITSIO_ENABLE_TESTING)

## ==============================================================================
##
##  System inspection
##
## ==============================================================================

include (FindCFITSIO.cmake)

## ==============================================================================
##
##  Build instructions
##
## ==============================================================================

if (NOT CFITSIO_FOUND OR CFITSIO_FORCE_BUILD)

  ## Locate source file to build from

  find_file (CFITSIO_SOURCE ${CFITSIO_SOURCE_ARCHIVE}
    PATHS ${CMAKE_CURRENT_SOURCE_DIR}
    )

  if (NOT CFITSIO_SOURCE)
    set (CFITSIO_SOURCE ${CFITSIO_URL})
  endif (NOT CFITSIO_SOURCE)

  ## Build package from source

  if (CFITSIO_FROM_REPOS)

    ExternalProject_Add (cfitsio
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      SVN_REPOSITORY ${CFITSIO_SVN}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX}
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  else (CFITSIO_FROM_REPOS)

    ExternalProject_Add (cfitsio
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      URL ${CFITSIO_SOURCE}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX}
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  endif (CFITSIO_FROM_REPOS)

else (NOT CFITSIO_FOUND OR CFITSIO_FORCE_BUILD)

  add_custom_target (cfitsio
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "[CFITSIO] Found system-wide installation; skipping rebuild!"
    )

endif (NOT CFITSIO_FOUND OR CFITSIO_FORCE_BUILD)
