cmake_minimum_required(VERSION 3.12)
project(rpi_cam_previewer VERSION 1.0.0 LANGUAGES CXX)


# prevent in-source build
if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
    message(FATAL_ERROR "In-source build detected!")
endif()

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE RelWithDebInfo)
endif(NOT CMAKE_BUILD_TYPE)

message(STATUS "Build of type ${CMAKE_BUILD_TYPE}")

set (CMAKE_CXX_STANDARD 14)

#find_package(bcm_host REQUIRED)

# First try the non standard path (raspbian and co)
if(EXISTS "/opt/vc/include/bcm_host.h")
    set(BCMHOST found)
    set(RPI_ROOT_PATH "/opt/vc")
endif()

if(EXISTS "${CMAKE_FIND_ROOT_PATH}/usr/include/bcm_host.h")
    set(BCMHOST found)
    set(RPI_ROOT_PATH "${CMAKE_FIND_ROOT_PATH}/usr")
endif()

if (DEFINED BCMHOST)
    MESSAGE("bcm_host.h found")
    set(GLSystem "OpenGL ES")
else()
    MESSAGE(FATAL_ERROR "bcm_host.h not found")
endif()

if(DEFINED BCMHOST)
    LIST(APPEND COMMON_LIBRARIES
            bcm_host
            vcos
            mmal_core
            mmal_util
            mmal_vc_client)
endif()

set(COMMON_INCLUDE_DIRS "${RPI_ROOT_PATH}/include")

add_executable(${PROJECT_NAME} rpi_cam_previewer.cc)
#//bcm_host -lvcos -lmmal_core -lmmal_util -lmmal_vc_client
