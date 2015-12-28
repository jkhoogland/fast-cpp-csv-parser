# basic setup for running some tests using cmake
#
# - uses gtest for testing framework
#   - automatically downloaded
# - add tests testA.cpp, testB.cpp in test directory
#   - automatically picked up by cmake when building
#
# build/test recipy:
#
# 1. cmake -H. -B_bld
# 2. cmake --build _bld
# 3. cd _bld/test
# 4. ctest

cmake_minimum_required(VERSION 3.0)

project( fast_cpp_csv_parser CXX )

####
# set some compiler flags
if( "${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU" OR
    "${CMAKE_CXX_COMPILER_ID}" STREQUAL "AppleClang" OR
    "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang" )

  # silence some warnings -Wno-comment
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Wextra -Wno-comment" )
  
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
    
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4 /WX /EHsc")
  
endif()

####
# add test projects
add_subdirectory( test )
