cmake_minimum_required(VERSION 3.8...3.31)
if(DEFINED PROJECT_NAME)
  set(HASH-LIBRARY_SUBPROJECT ON)
endif()
project(hash-library VERSION 1.0.0 LANGUAGES CXX)
include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

file(GLOB SRC_FILES
  "*.cpp"
)
file(GLOB HDR_FILES
  "*.h"
)
add_library(hash-library ${SRC_FILES})
add_library(hash-library::hash-library ALIAS hash-library)
target_compile_features(hash-library PUBLIC cxx_std_11)
target_include_directories(hash-library INTERFACE
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>)

option(BUILD_TESTS "Build hash-library tests")
if(BUILD_TESTS)
  add_subdirectory(tests)
endif()

if(NOT HASH-LIBRARY_SUBPROJECT)
  configure_package_config_file(hash-libraryConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/hash-libraryConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/hash-library)
  write_basic_package_version_file(hash-libraryConfigVersion.cmake
    COMPATIBILITY AnyNewerVersion)

  configure_file(hash-library.pc.in hash-library.pc @ONLY)

  install(TARGETS hash-library EXPORT hash-libraryTargets)
  install(EXPORT hash-libraryTargets
          FILE hash-libraryTargets.cmake
          NAMESPACE hash-library::
          DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/hash-library)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/hash-libraryConfig.cmake
                ${CMAKE_CURRENT_BINARY_DIR}/hash-libraryConfigVersion.cmake
          DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/hash-library)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/hash-library.pc
          DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)
  install(FILES ${HDR_FILES}
          DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/hash-library)
endif()
