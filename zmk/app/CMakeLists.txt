# This file is the main entry point for ZMK's build process

cmake_minimum_required(VERSION 3.13.1)

set(BOARD_ROOT ${CMAKE_SOURCE_DIR})
set(ZMK_BASE ${CMAKE_SOURCE_DIR}/..)
set(KEYMAP_DIR ${CMAKE_SOURCE_DIR}/../config)

# Import the Zephyr build system
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})

project(zmk)

# Add ZMK as a subdirectory to include its CMake configuration
add_subdirectory(src)

# Include board-specific code
if(EXISTS ${CMAKE_SOURCE_DIR}/boards/${BOARD})
  add_subdirectory(boards/${BOARD})
endif()

# Ensure configuration headers are correctly included
zephyr_include_directories(
  ${CMAKE_SOURCE_DIR}/include
  ${CMAKE_SOURCE_DIR}/../config
)

# Process app-specific configuration
if(EXISTS ${CMAKE_SOURCE_DIR}/app_config.cmake)
  include(${CMAKE_SOURCE_DIR}/app_config.cmake)
endif()