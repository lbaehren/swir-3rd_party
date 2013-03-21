
## ==============================================================================
##
##  CMake project settings
##
## ==============================================================================

if (NOT PROJECT_NAME)
  ## Name of the project handled by CMake
  project (SWIR-3rd_party)
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

add_subdirectory (packages)

## ==============================================================================
##
##  Configuration summary
##
## ==============================================================================

message ( " ========================================================"       )
message ( " [swir_calib] Configuration summary."                            )
message ( " --------------------------------------------------------"       )
message ( " System configuration"                                           )
message ( " .. Processor type .............. = ${CMAKE_SYSTEM_PROCESSOR}"   )
message ( " .. CMake executable ............ = ${CMAKE_COMMAND}"            )
message ( " .. CMake version ............... = ${CMAKE_VERSION}"            )
message ( " .. System name ................. = ${CMAKE_SYSTEM_NAME}"        )
message ( " .. System version .............. = ${CMAKE_SYSTEM_VERSION}"     )
message ( " .. C compiler .................. = ${CMAKE_C_COMPILER}"         )
message ( " .. C compiler flags ............ = ${CMAKE_C_FLAGS}"            )
message ( " .. C++ compiler ................ = ${CMAKE_CXX_COMPILER}"       )
message ( " .. C++ compiler flags .......... = ${CMAKE_CXX_FLAGS}"          )
message ( " --------------------------------------------------------"       )
message ( " Project configuration"                                          )
message ( " .. Project name ................. = ${PROJECT_NAME}"            )
message ( " .. Project version .............. = ${PROJECT_VERSION}"         )
message ( " .. Installation prefix .......... = ${CMAKE_INSTALL_PREFIX}"    )
message ( " ========================================================"       )

