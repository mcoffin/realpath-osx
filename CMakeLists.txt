# Project Setup
cmake_minimum_required(VERSION 2.6)
project(realpath)

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

# List Source Files
set(SOURCES realpath.c)

# Create Executables
add_executable(${CMAKE_PROJECT_NAME} ${SOURCES})

# Create Install Target
install(TARGETS ${CMAKE_PROJECT_NAME} DESTINATION bin)
