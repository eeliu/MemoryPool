include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(MemoryPoolC11 INTERFACE)
add_library(MemoryPool::C11 ALIAS MemoryPoolC11)

target_compile_features(MemoryPoolC11 INTERFACE cxx_std_11)

target_include_directories(MemoryPoolC11
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/MemoryPoolC11>
)

install(
  TARGETS MemoryPoolC11
  EXPORT MemoryPoolC11Targets
)

install(
  FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/MemoryPool.hpp
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/MemoryPoolC11
)

install(
  EXPORT MemoryPoolC11Targets
  FILE MemoryPoolC11Targets.cmake
  NAMESPACE MemoryPool::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/MemoryPoolC11
)

configure_package_config_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/MemoryPoolC11Config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/MemoryPoolC11Config.cmake
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/MemoryPoolC11
)

write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/MemoryPoolC11ConfigVersion.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion
)

install(
  FILES
    ${CMAKE_CURRENT_BINARY_DIR}/MemoryPoolC11Config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/MemoryPoolC11ConfigVersion.cmake
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/MemoryPoolC11
)
