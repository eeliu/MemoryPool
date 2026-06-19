add_executable(MemoryPoolTest test.cpp)

target_link_libraries(MemoryPoolTest PRIVATE MemoryPool::C11)

add_test(NAME MemoryPoolTest COMMAND MemoryPoolTest)
