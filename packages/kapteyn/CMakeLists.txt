
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

set (KAPTEYN_SOURCE_VERSION "2.2" )
set (KAPTEYN_SOURCE_ARCHIVE "kapteyn-${KAPTEYN_SOURCE_VERSION}.tar.gz" )
set (KAPTEYN_URL            "http://www.astro.rug.nl/software/kapteyn/${KAPTEYN_SOURCE_ARCHIVE}" )
set (KAPTEYN_SVN            "" )

## =============================================================================
##
##  Options
##
## =============================================================================

option (KAPTEYN_FORCE_BUILD    "Force build and installation of package?" NO )
option (KAPTEYN_FROM_REPOS     "Install package from repository?"         NO )
option (KAPTEYN_ENABLE_TESTING "Enable testing and experimental builds?"  YES )

## Handle options

if (KAPTEYN_ENABLE_TESTING)
  include (CTest)
  enable_testing()
endif (KAPTEYN_ENABLE_TESTING)

## =============================================================================
##
##  System inspection
##
## =============================================================================

## =============================================================================
##
##  Build instructions
##
## =============================================================================

if (NOT KAPTEYN_FOUND OR KAPTEYN_FORCE_BUILD)

  ## Locate source file to build from

  find_file (KAPTEYN_SOURCE ${KAPTEYN_SOURCE_ARCHIVE}
    PATHS ${CMAKE_CURRENT_SOURCE_DIR}
    )

  if (NOT KAPTEYN_SOURCE)
    set (KAPTEYN_SOURCE ${KAPTEYN_URL})
  endif (NOT KAPTEYN_SOURCE)

  ## Build package from source

  if (KAPTEYN_FROM_REPOS)

    ExternalProject_Add (kapteyn
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      SVN_REPOSITORY ${KAPTEYN_SVN}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ""
      BUILD_COMMAND ""
      INSTALL_COMMAND ${PYTHON_EXECUTABLE} setup.py install --prefix=${CMAKE_INSTALL_PREFIX}
      )

  else (KAPTEYN_FROM_REPOS)

    ExternalProject_Add (kapteyn
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}
      DOWNLOAD_DIR download
      SOURCE_DIR source
      URL ${KAPTEYN_SOURCE}
      BUILD_IN_SOURCE 1
      CONFIGURE_COMMAND ""
      BUILD_COMMAND ""
      INSTALL_COMMAND ${PYTHON_EXECUTABLE} setup.py install --prefix=${CMAKE_INSTALL_PREFIX}
      )

  endif (KAPTEYN_FROM_REPOS)

else (NOT KAPTEYN_FOUND OR KAPTEYN_FORCE_BUILD)

  add_custom_target (kapteyn
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "[kapteyn] Found system-wide installation; skipping rebuild!"
    )

endif (NOT KAPTEYN_FOUND OR KAPTEYN_FORCE_BUILD)
