################################################################################
# Copyright (C) 2017 Advanced Micro Devices, Inc.
################################################################################


cmake_minimum_required (VERSION 3.5)
find_package(ROCM)
project(toolchain-var)
enable_language(CXX)

if(CHANGE_TOOLCHAIN)
set(CMAKE_CXX_COMPILER gcc)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -w")
endif()

add_library(lib INTERFACE)
