################################################################################
# Copyright (C) 2017 Advanced Micro Devices, Inc.
################################################################################

cmake_minimum_required (VERSION 3.6)

set(CMAKE_INSTALL_PREFIX "/opt/rocm" CACHE PATH "")

project(rocm-cmake)

install(DIRECTORY share DESTINATION .)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/share/rocm/cmake)
include(ROCMCreatePackage)
include(ROCMSetupVersion)

rocm_setup_version(VERSION 0.4)

rocm_create_package(
    NAME rocm-cmake
    MAINTAINER "Paul Fultz II pfultz@amd.com"
)

set(CONFIGURE_DEPENDS)
if(CMAKE_VERSION VERSION_GREATER_EQUAL 3.12)
set(CONFIGURE_DEPENDS CONFIGURE_DEPENDS)
endif()
file(GLOB_RECURSE CMAKE_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ${CONFIGURE_DEPENDS} share/*.cmake test/*.cmake)

add_custom_target(analyze COMMAND cmake-lint ${CMAKE_FILES} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
add_custom_target(format COMMAND cmake-format -i ${CMAKE_FILES} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

enable_testing()
add_custom_target(check COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure -C ${CMAKE_CFG_INTDIR})
add_subdirectory(test)
