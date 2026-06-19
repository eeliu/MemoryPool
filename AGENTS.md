# Project Guidelines

## Code Style
- This is a C++11 header-only library. Development happens entirely in C++ headers. Do not create separate `.cpp` definition files for implementational additions to [C-11/MemoryPool.hpp](C-11/MemoryPool.hpp).
- Strictly adhere to C++11 standard-compatible features (e.g., using `std::forward`, `std::move`, variadic templates, perfect forwarding). Do not introduce C++14/17/20 features unless requested, to guarantee broad compiler compatibility.
- Ensure all custom memory operations handle correct element alignment using `alignof` and `padPointer`.

## Architecture
- Header-Only Library: The main interface is [C-11/MemoryPool.hpp](C-11/MemoryPool.hpp).
- Memory Allocation Strategy: To maximize performance and avoid memory fragmentation, the allocator reserves resources in large chunks specified by the `BlockSize` template parameter (defaults to 4096 bytes).
- Single-Element Constraint: Crucially, `MemoryPool` **cannot** allocate multiple elements in a single `allocate` call. The count argument `n` passed to `allocate(size_type n, ...)` and `deallocate(...)` is ignored. Only block slot allocations/deallocations of size 1 are supported.
- Retention Rule: Internal memory blocks are never returned to the operating system until the entire `MemoryPool` object is destroyed. Deallocated slots are maintained in an internal singly-linked free list for rapid future reuse.
- Thread Safety: The implementation is completely **thread-unsafe**. Distinct allocation pools must be used per-thread if multi-threading is present.

## Build and Test
- **CMake Configuration**: The principal targets are configured in [CMakeLists.txt](CMakeLists.txt) and [C-11/CMakeLists.txt](C-11/CMakeLists.txt). The library compiles into an interface library target `MemoryPool::C11` (`MemoryPoolC11`).
- **Standard CMake Build**:
  ```bash
  mkdir -p build && cd build
  cmake ..
  make
  ```
- **Tests and Benchmark**:
  - **CMake (Recommended)**: Enable the `MEMORYPOOL_BUILD_TESTS` option to build the benchmark.
    ```bash
    mkdir -p build && cd build
    cmake .. -DMEMORYPOOL_BUILD_TESTS=ON
    make
    ./tests/MemoryPoolTest
    ```
  - **Manual Compilation**: Build [tests/test.cpp](tests/test.cpp) directly:
    ```bash
    cd tests
    g++ -O3 -std=c++11 test.cpp -I../C-11 -o test_benchmark
    ./test_benchmark
    ```

## Conventions
- **Object Initialization**:
  - For non-trivial classes/structs with constructor definitions, rely on `newElement(Args&&...)` and `deleteElement(pointer)` rather than raw `allocate` or direct construction. These helpers automatically execute the placement `new` and explicitly trigger the object's destructor.
  - Standard allocator integration should utilize standard `std::allocator_traits` structures, specifically utilizing type rebinding (`rebind<Node>::other`) as demonstrated in [tests/StackAlloc.h](tests/StackAlloc.h#L44-L45).
- Find more comprehensive user guidance and API usage in [README.md](README.md).

## Common Pitfalls and Troubleshooting
- **MemoryPool.hpp**: The implementation is consolidated into a single `.hpp` file.
- **Test Compilation Path**: Standard test compilation directly with `g++ tests/test.cpp` will fail configuration because of missing search paths. Always pass `-I../C-11` or proper header directory includes:
  ```bash
  g++ -O3 -std=c++11 -I../C-11 tests/test.cpp -o test_benchmark
  ```

