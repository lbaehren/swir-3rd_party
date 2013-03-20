
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

set (HDF5_SOURCE_VERSION "1.8.9" )
set (HDF5_SOURCE_ARCHIVE "hdf5-${HDF5_SOURCE_VERSION}.tar.gz" )
set (HDF5_URL            "http://www.hdfgroup.org/ftp/HDF5/prev-releases/hdf5-${HDF5_SOURCE_VERSION}/src/${HDF5_SOURCE_ARCHIVE}" )
set (HDF5_SVN            "" )

## ==============================================================================
##
##  Options
##
## ==============================================================================

option (HDF5_FORCE_BUILD    "Force build and installation of package?" NO )
option (HDF5_FROM_REPOS     "Install package from repository?"         NO )
option (HDF5_ENABLE_TESTING "Enable testing and experimental builds?"  YES )

## Handle options

if (HDF5_ENABLE_TESTING)
  include (CTest)
  enable_testing()
endif (HDF5_ENABLE_TESTING)

## ==============================================================================
##
##  Build instructions
##
## ==============================================================================

if (NOT HDF5_FOUND OR HDF5_FORCE_BUILD)

  ## Locate source file to build from

  find_file (HDF5_SOURCE ${HDF5_SOURCE_ARCHIVE}
    PATHS ${CMAKE_CURRENT_SOURCE_DIR}
    )

  if (NOT HDF5_SOURCE)
    set (HDF5_SOURCE ${HDF5_URL})
  endif (NOT HDF5_SOURCE)

  ## Build package from source

  if (HDF5_FROM_REPOS)

    ExternalProject_Add (hdf5
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      SVN_REPOSITORY ${HDF5_SVN}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX} --enable-cxx
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  else (HDF5_FROM_REPOS)

    ExternalProject_Add (hdf5
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      URL ${HDF5_SOURCE}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX} --enable-cxx
      INSTALL_COMMAND ${PORT_INSTALL_COMMAND} make install
      )

  endif (HDF5_FROM_REPOS)

else (NOT HDF5_FOUND OR HDF5_FORCE_BUILD)

  add_custom_target (hdf5
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "[HDF5] Found system-wide installation; skipping rebuild!"
    )

endif (NOT HDF5_FOUND OR HDF5_FORCE_BUILD)
