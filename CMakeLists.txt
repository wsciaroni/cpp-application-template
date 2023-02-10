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

option(BUILD_TESTS OFF)

if(BUILD_TESTS)
    include(FetchContent)
    FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG v1.13.0
    )
    # For Windows: Prevent overriding the parent project's compiler/linker settings
    if(MSVC)
        set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
    endif(MSVC)
    FetchContent_MakeAvailable(googletest)

    if(MSVC)
    set_target_properties(gtest PROPERTIES FOLDER "external/Google")
    set_target_properties(gmock PROPERTIES FOLDER "external/Google")
    set_target_properties(gtest_main PROPERTIES FOLDER "external/Google")
    set_target_properties(gmock_main PROPERTIES FOLDER "external/Google")
    endif(MSVC)

    enable_testing()
    include(CTest)
endif(BUILD_TESTS)

# Include sub-projects.
add_subdirectory ("${CMAKE_CURRENT_LIST_DIR}/libraries")
add_subdirectory ("${CMAKE_CURRENT_LIST_DIR}/applications")
