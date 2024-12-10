if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

IF (NOT MSVC)
  set(CMAKE_CXX_FLAGS "-Wall -Wextra")
  set(CMAKE_CXX_FLAGS_DEBUG "-g")
  set(CMAKE_CXX_FLAGS_RELEASE "-O3")
ENDIF (NOT MSVC)

file(GLOB SRC_FILES
  "tests.cpp"
)
add_executable(hash-library-test ${SRC_FILES})
target_link_libraries(hash-library-test PRIVATE hash-library::hash-library)
