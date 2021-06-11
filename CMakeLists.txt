cmake_minimum_required(VERSION 3.2)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(bank)
SET(COVERAGE OFF CACHE BOOL "Coverage")
enable_testing()
add_subdirectory(third-party/gtest)
include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR})


add_library(lib1 Account.h Account.cpp)
add_library(lib2 Transaction.h Transaction.cpp)
add_executable(testst test.cpp)
target_compile_options(testst PRIVATE --coverage)
target_link_libraries(testst PRIVATE --coverage gtest gtest_main)
add_test( testst testst )
