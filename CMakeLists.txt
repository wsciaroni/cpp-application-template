# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.25)

project ("CXX_PROJECT_TEMPLATE")

# GoogleTest requires at least C++14
set(CMAKE_CXX_STANDARD 17)

# To organize projects in VisualStudio:
if(MSVC)
    set_property(GLOBAL PROPERTY USE_FOLDERS ON)
endif(MSVC)

include(cmake/EnableGTest.cmake)
enable_gtest()

# Strict Compiler Flags (Global)
if(MSVC)
    add_compile_options(/W4 /permissive-)
else()
    add_compile_options(
        -Wall
        -Wextra
        -Wpedantic
        -Wshadow
        -Wconversion
        -Wsign-conversion
        -Wcast-align
        -Wformat=2
        -Wunused
        -Wnon-virtual-dtor
    )
endif()

# Warnings as Errors (Global)
option(NO_WARN_AS_ERROR "Disable warnings as errors" OFF)
if(NOT NO_WARN_AS_ERROR)
    if(MSVC)
        add_compile_options(/WX)
    else()
        add_compile_options(-Werror)
    endif()
endif()

include(cmake/AddCommonLibraryDirectory.cmake)
option(COMMON_LIB_GLOBAL_USE_CTEST_ONLY "Force all common libraries to use CTest only (skip gtest discovery)" OFF)

# Include sub-projects.
add_subdirectory ("${CMAKE_CURRENT_LIST_DIR}/libraries")
add_subdirectory ("${CMAKE_CURRENT_LIST_DIR}/applications")

add_subdirectory ("${CMAKE_CURRENT_LIST_DIR}/tests")
