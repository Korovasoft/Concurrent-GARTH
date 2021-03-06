########################################
########################################
##
##  Copyright (C) 2014 Korovasoft, Inc.
##
##  File:
##      \file CMakeLists.txt
##
##  Description:
##      \brief Top-level CMakeLists.txt
##
##  Author:
##      \author J. Caleb Wherry
##
########################################
########################################


#
# CMake version and project name:
#
cmake_minimum_required(VERSION 2.6)
project(Concurrent-GARTH)


#
# Check compiler versions:
#
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")

  # Check version. If empty, warn. If too old, error out:
  if ("${CMAKE_CXX_COMPILER_VERSION}" STREQUAL "")
    message(WARNING "GCC Compiler version is unknown, proceed at your own risk!")
  elseif (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 4.8)
    message(FATAL_ERROR "GCC compiler version must be at least 4.8!")
  endif()

	# Set compiler specific flags:
	set(CMAKE_CXX_FLAGS_DEBUG "-Wall -Wextra -ggdb -O2 -std=c++11 -pthread")
  set(CMAKE_CXX_FLAGS_RELEASE "-O2 -std=c++11 -pthread")

  # Additional compiler flags depending on architecture:
  message( STATUS "System processor: ${CMAKE_SYSTEM_PROCESSOR}")
  if ("${CMAKE_SYSTEM_PROCESSOR}" MATCHES "(.*)arm(.*)")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wl,--no-as-needed")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -Wl,--no-as-needed")
  endif()

elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")

  # Check version. If empty, warn. If too old, error out:
  if ("${CMAKE_CXX_COMPILER_VERSION}" STREQUAL "")
    message(WARNING "Clang compiler version is unknown, proceed at your own risk!")
  elseif (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 3.4)
    message(FATAL_ERROR "Clang compiler version must be at least 3.4!")
  endif()

  # Set compiler specific flags:
  set(CMAKE_CXX_FLAGS_DEBUG "-Wall -Wextra -ggdb -O2 -std=c++11 -stdlib=libc++ -pthread")
  set(CMAKE_CXX_FLAGS_RELEASE "-O2 -std=c++11 -stdlib=libc++ -pthread")

elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")

  # Check version. If empty, warn. If too old, error out:
  if ("${CMAKE_CXX_COMPILER_VERSION}" STREQUAL "")
    message(WARNING "MSVC compiler version is unknown, proceed at your own risk!")
  elseif (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 18.0)
    message(FATAL_ERROR "MSVC compiler version must be at least 18.0!")
  endif()

  # Set compiler specific flags:
  set(CMAKE_CXX_FLAGS "")

elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Intel")

  # Check version. If empty, warn. If too old, error out:
  if ("${CMAKE_CXX_COMPILER_VERSION}" STREQUAL "")
    message(WARNING "Intel compiler version is unknown, proceed at your own risk!")
  elseif (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 14.0)
    message(FATAL_ERROR "MSVC compiler version must be at least 14.0!")
  endif()

  # Set compiler specific flags:
  set(CMAKE_CXX_FLAGS "")

else()

  message(WARNING "The '${CMAKE_CXX_COMPILER_ID}' compiler is untested and unsupported, continue at your own risk!")

endif()


#
# Set global project path variables:
#
set(PROJECT_ROOT_DIR "${PROJECT_SOURCE_DIR}")
set(PROJECT_SRC_DIR "${PROJECT_ROOT_DIR}/src")
set(PROJECT_LIB_DIR "${PROJECT_SRC_DIR}/lib")
set(PROJECT_APP_DIR "${PROJECT_SRC_DIR}/app")
set(PROJECT_TEST_DIR "${PROJECT_SRC_DIR}/test")
set(PROJECT_DOC_DIR "${PROJECT_ROOT_DIR}/doc")


#
# Testing:
#
include( CTest )


#
# Add subdirectories:
#
add_subdirectory(src)
add_subdirectory(doc)
