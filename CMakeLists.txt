cmake_minimum_required (VERSION 2.8.10)

set(PMR_VERSION_MAJOR 0)
set(PMR_VERSION_MINOR 1)
set(PMR_VERSION_PATCH 1)
set(PMR_VERSION ${PMR_VERSION_MAJOR}.${PMR_VERSION_MINOR}.${PMR_VERSION_PATCH})

project(pmr)


file(GLOB_RECURSE pmr_srcs src/*.cpp)
add_library(pmr STATIC ${pmr_srcs})
target_include_directories(pmr PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
    PRIVATE src)

set_target_properties(pmr PROPERTIES
    CXX_STANDARD 11
    VERSION ${PMR_VERSION}
    SOVERSION ${PMR_VERSION_MAJOR})

target_compile_options(pmr
    PRIVATE -Wall
    PRIVATE -Wpedantic
    PRIVATE -Wextra
    PRIVATE -Werror
    PRIVATE -std=c++11
    #PRIVATE -fsanitize=address
    PRIVATE -fno-omit-frame-pointer)
#SET(CMAKE_EXE_LINKER_FLAGS ${CMAKE_EXE_LINKER_FLAGS} -fsanitize=address)

include(CTest)
add_subdirectory(test)


install(
  TARGETS pmr
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib)

install(DIRECTORY include DESTINATION .)
