
#-------------------------------------------------------------------------------
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
#-------------------------------------------------------------------------------

## =============================================================================
##
##  Package information
##
## =============================================================================

set (H5PY_SOURCE_VERSION "2.1.1" )
set (H5PY_SOURCE_ARCHIVE "h5py-${H5PY_SOURCE_VERSION}.tar.gz" )
set (H5PY_URL            "http://h5py.googlecode.com/files/${H5PY_SOURCE_ARCHIVE}" )
set (H5PY_GIT            "git://github.com/h5py/h5py.git" )

## =============================================================================
##
##  Options
##
## =============================================================================

option (H5PY_FORCE_BUILD    "Force build and installation of package?" NO )
option (H5PY_FROM_REPOS     "Install package from repository?"         NO )
option (H5PY_ENABLE_TESTING "Enable testing and experimental builds?"  YES )

## Handle options

if (H5PY_ENABLE_TESTING)
  include (CTest)
  enable_testing()
endif (H5PY_ENABLE_TESTING)

## =============================================================================
##
##  System inspection
##
## =============================================================================

if (NOT PYTHON_EXECUTABLE)
  find_package (PythonInterp)
endif (NOT PYTHON_EXECUTABLE)

## =============================================================================
##
##  Build instructions
##
## =============================================================================

if (NOT H5PY_FOUND OR H5PY_FORCE_BUILD)

  ## Locate source file to build from

  find_file (H5PY_SOURCE ${H5PY_SOURCE_ARCHIVE}
    PATHS ${CMAKE_CURRENT_SOURCE_DIR}
    )

  if (NOT H5PY_SOURCE)
    set (H5PY_SOURCE ${H5PY_URL})
  endif (NOT H5PY_SOURCE)

  ## Build package from source

  if (H5PY_FROM_REPOS)

    ExternalProject_Add (h5py
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      GIT_REPOSITORY ${H5PY_GIT}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ""
      BUILD_COMMAND ""
      INSTALL_COMMAND ${PYTHON_EXECUTABLE} setup.py install --prefix=${CMAKE_INSTALL_PREFIX}
      )

  else (H5PY_FROM_REPOS)

    ExternalProject_Add (h5py
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      URL ${H5PY_SOURCE}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ""
      BUILD_COMMAND ""
      INSTALL_COMMAND ${PYTHON_EXECUTABLE} setup.py install --prefix=${CMAKE_INSTALL_PREFIX}
      )

  endif (H5PY_FROM_REPOS)

else (NOT H5PY_FOUND OR H5PY_FORCE_BUILD)

  add_custom_target (h5py
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "[h5py] Found system-wide installation; skipping rebuild!"
    )

endif (NOT H5PY_FOUND OR H5PY_FORCE_BUILD)
