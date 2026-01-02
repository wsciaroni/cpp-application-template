
if(BUILD_TESTING)
    # Tests
    file(GLOB_RECURSE unittests "${CMAKE_CURRENT_LIST_DIR}/unittests/*.cxx")
    add_executable(
    testLib
    ${unittests}
    )
    target_link_libraries(
        testLib
        PUBLIC
        coreLib
        GTest::gtest_main
        GTest::gmock_main
    )

    common_library_configure_target(testLib "${NO_WARN_AS_ERROR}")

    include(GoogleTest)
    gtest_discover_tests(testLib)
endif(BUILD_TESTING)
