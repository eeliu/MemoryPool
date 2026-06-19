cmake_minimum_required(VERSION 3.16)

project(MemoryPool VERSION 1.0.0 LANGUAGES CXX)

option(MEMORYPOOL_BUILD_C11 "Enable C++11 MemoryPool target" ON)
option(MEMORYPOOL_BUILD_TESTS "Build tests" OFF)

if(MEMORYPOOL_BUILD_C11)
  add_subdirectory(C-11)
endif()

if(MEMORYPOOL_BUILD_TESTS)
  enable_testing()
  add_subdirectory(tests)
endif()
