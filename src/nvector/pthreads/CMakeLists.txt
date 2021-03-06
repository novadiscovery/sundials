# ---------------------------------------------------------------
# Programmer(s): Steve Smith and Cody J. Balos @ LLNL
# ---------------------------------------------------------------
# SUNDIALS Copyright Start
# Copyright (c) 2002-2020, Lawrence Livermore National Security
# and Southern Methodist University.
# All rights reserved.
#
# See the top-level LICENSE and NOTICE files for details.
#
# SPDX-License-Identifier: BSD-3-Clause
# SUNDIALS Copyright End
# ---------------------------------------------------------------
# CMakeLists.txt file for the pthreads NVECTOR library
# ---------------------------------------------------------------

install(CODE "MESSAGE(\"\nInstall NVECTOR_PTHREADS\n\")")

# Add variable nvecpthreads_HEADERS with the exported NVECPTHREADS header files
set(nvecpthreads_HEADERS
  ${SUNDIALS_SOURCE_DIR}/include/nvector/nvector_pthreads.h
  )

# Create the library
sundials_add_library(sundials_nvecpthreads
  SOURCES
    nvector_pthreads.c
  OBJECT_LIBRARIES
    sundials_generic_obj
  OUTPUT_NAME
    sundials_nvecpthreads
  VERSION
    ${nveclib_VERSION}
  SOVERSION
    ${nveclib_SOVERSION}
)

# Install the NVECPTHREADS header files
install(FILES ${nvecpthreads_HEADERS} DESTINATION include/nvector)

#
message(STATUS "Added NVECTOR_PTHREADS module")

# Add F2003 module if the interface is enabled
if(BUILD_FORTRAN_MODULE_INTERFACE)
  add_subdirectory(fmod)
endif()

# If FCMIX is enabled, build and install the Fortran77 library
if(BUILD_FORTRAN77_INTERFACE)
  sundials_add_library(sundials_fnvecpthreads
    SOURCES
      fnvector_pthreads.c
    LINK_LIBRARIES
      PRIVATE sundials_nvecpthreads
    OUTPUT_NAME
      sundials_fnvecpthreads
    VERSION
      ${nveclib_VERSION}
    SOVERSION
      ${nveclib_SOVERSION}
  )
  message(STATUS "Added NVECTOR_PTHREADS F77 interface")
endif()
