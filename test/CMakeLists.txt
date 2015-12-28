#####
# add gtest

include( ExternalProject )
include( CTest ) # internally calls enable_testing()
enable_testing()

set( GTEST_SOURCE_DIR ${PROJECT_BINARY_DIR}/3rd/Source/gtest )
set( GTEST_BINARY_DIR ${PROJECT_BINARY_DIR}/3rd/Build/gtest )

externalproject_add( gtest
  URL https://googletest.googlecode.com/files/gtest-1.7.0.zip
  URL_HASH SHA1=f85f6d2481e2c6c4a18539e391aa4ea8ab0394af
  SOURCE_DIR "${GTEST_SOURCE_DIR}"
  BINARY_DIR "${GTEST_BINARY_DIR}"
  CMAKE_ARGS "${gtest_cmake_args}" "-DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}"
  INSTALL_COMMAND ""
  )

set( GTEST_INCLUDE_DIRS "${GTEST_SOURCE_DIR}/include" )
set( GTEST_LIBRARY "${GTEST_BINARY_DIR}/libgtest${CMAKE_STATIC_LIBRARY_SUFFIX}" )
set( GTEST_MAIN "${GTEST_BINARY_DIR}/libgtest_main${CMAKE_STATIC_LIBRARY_SUFFIX}" )

####
# add location of csv.h file

set( CSV_INCLUDE_DIRS ${CMAKE_SOURCE_DIR} )

####
# add test project

project( fast_cpp_csv_parser_test CXX )

file( GLOB SourceFiles RELATIVE ${PROJECT_SOURCE_DIR} *.cpp )

set( target_name test_csv )

add_executable( ${target_name} ${SourceFiles} )

add_test(${target_name} ${target_name})

target_include_directories( ${target_name}
  PUBLIC
  ${CSV_INCLUDE_DIRS}
  ${GTEST_INCLUDE_DIRS} )

target_link_libraries( ${target_name}
  ${GTEST_LIBRARY}
  ${GTEST_MAIN}
  ${CMAKE_THREAD_LIBS_INIT} )

####
# copy test data to test bin directory

file( GLOB CSVFiles *.csv )

# linux/osx specific... need to add some win version as well
add_custom_command( TARGET ${target_name}
  POST_BUILD COMMAND cp ${CSVFiles} ${CMAKE_BINARY_DIR}/test )
