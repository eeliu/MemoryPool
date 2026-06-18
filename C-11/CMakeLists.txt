add_executable(test_c11 ../test.cpp)
target_include_directories(test_c11 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_SOURCE_DIR})
target_compile_options(test_c11 PRIVATE -O2)
set_target_properties(test_c11 PROPERTIES CXX_STANDARD 11 CXX_STANDARD_REQUIRED ON)
