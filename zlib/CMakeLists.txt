
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

set (ZLIB_SOURCE_VERSION "1.2.7" )
set (ZLIB_SOURCE_ARCHIVE "zlib-${ZLIB_SOURCE_VERSION}.tar.gz" )
set (ZLIB_URL            "ftp://ftp.unidata.ucar.edu/pub/netcdf/netcdf-4/${ZLIB_SOURCE_ARCHIVE}" )
set (ZLIB_SVN            "" )

## ==============================================================================
##
##  Options
##
## ==============================================================================

option (ZLIB_FORCE_BUILD    "Force build and installation of package?" NO )
option (ZLIB_FROM_REPOS     "Install package from repository?"         NO )
option (ZLIB_ENABLE_TESTING "Enable testing and experimental builds?"  YES )

## Handle options

if (ZLIB_ENABLE_TESTING)
  include (CTest)
  enable_testing()
endif (ZLIB_ENABLE_TESTING)

## ==============================================================================
##
##  Build instructions
##
## ==============================================================================

if (NOT ZLIB_FOUND OR ZLIB_FORCE_BUILD)

  ## Locate source file to build from

  find_file (ZLIB_SOURCE ${ZLIB_SOURCE_ARCHIVE}
    PATHS ${CMAKE_CURRENT_SOURCE_DIR}
    )

  if (NOT ZLIB_SOURCE)
    set (ZLIB_SOURCE ${ZLIB_URL})
  endif (NOT ZLIB_SOURCE)

  ## Build package from source

  if (ZLIB_FROM_REPOS)

    ExternalProject_Add (zlib
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      SVN_REPOSITORY ${ZLIB_SVN}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX}
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  else (ZLIB_FROM_REPOS)

    ExternalProject_Add (zlib
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      URL ${ZLIB_SOURCE}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX}
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  endif (ZLIB_FROM_REPOS)

else (NOT ZLIB_FOUND OR ZLIB_FORCE_BUILD)

  add_custom_target (zlib
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "[zlib] Found system-wide installation; skipping rebuild!"
    )

endif (NOT ZLIB_FOUND OR ZLIB_FORCE_BUILD)
