add_executable(test_c98 ../test.cpp)
target_include_directories(test_c98 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_SOURCE_DIR})
target_compile_options(test_c98 PRIVATE -O2)
set_target_properties(test_c98 PROPERTIES CXX_STANDARD 98 CXX_STANDARD_REQUIRED ON)
