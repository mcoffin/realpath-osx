# Project Setup
cmake_minimum_required(VERSION 2.6)
project(realpath)

# Versioning
set(REALPATH_VERSION_MAJOR 0)
set(REALPATH_VERSION_MINOR 1)
set(REALPATH_VERSION_PATCH 0)

# Platform checks
include(CheckFunctionExists)
check_function_exists(realpath HAVE_REALPATH)
if(NOT HAVE_REALPATH)
	message(FATAL_ERROR "Can't continue without POSIX function realpath()")
endif(NOT HAVE_REALPATH)

include(CheckIncludeFiles)
check_include_files("sys/param.h" HAVE_SYS_PARAM_H)
if(NOT HAVE_SYS_PARAM_H)
	message(FATAL_ERROR "Can't continue without sys/param.h header")
endif(NOT HAVE_SYS_PARAM_H)

# Package Generation Setup
if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
	set(CPACK_GENERATOR "TBZ2")
else(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
	set(CPACK_PACKAGE_CONTACT "mcoffin@irys.com")
	set(CPACK_GENERATOR "DEB")
endif(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
set(CPACK_PACKAGE_NAME realpath-minimal)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "A minimal implementation of the realpath utility based on the POSIX realpath() function.")
set(CPACK_PACKAGE_VERSION_MAJOR ${REALPATH_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${REALPATH_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${REALPATH_VERSION_PATCH})
set(CPACK_PACKAGE_VENDOR "Matt Coffin")
set(CPACK_PACKAGE_MAINTAINER "Matt Coffin")
include(CPack)

# List Source Files
set(SOURCES realpath.c)

# Create Executables
add_executable(${CMAKE_PROJECT_NAME} ${SOURCES})

# Create Install Target
install(TARGETS ${CMAKE_PROJECT_NAME} DESTINATION bin)
