### GoogleTest
# Adopted from https://github.com/google/googletest/blob/master/googletest/README.md
# Prevent overriding the parent project's compiler/linker
# settings on Windows
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

# Fix pthread related crash with GMock when running on Cygwin
# This should be included *before* adding the googletest directory
if ("${CMAKE_SYSTEM_NAME}" STREQUAL "CYGWIN")
    add_definitions(-DTEST_HAS_PTHREAD=0)
    set(gtest_disable_pthreads ON)
endif ()

# Add googletest directly to our build. This defines
# the gtest and gtest_main targets.
set(gtest_release_version "1.15.2")
add_subdirectory(googletest-release-${gtest_release_version} EXCLUDE_FROM_ALL)

# The gtest/gtest_main targets carry header search path
# dependencies automatically when using CMake 2.8.11 or
# later. Otherwise we have to add them here ourselves.
if (CMAKE_VERSION VERSION_LESS 2.8.11)
    include_directories("${gtest_SOURCE_DIR}/include"
            "${gmock_SOURCE_DIR}/include")
endif ()

# GTest and GMock do not play nicely with these flags, so disable them
target_compile_options(gtest PRIVATE
        -Wno-ctor-dtor-privacy
        -Wno-missing-include-dirs
        -Wno-sign-promo)
target_compile_options(gmock PRIVATE -Wno-pedantic)

function(configure_test testExecutable)
    # Link against gtest library
    target_link_libraries(${testExecutable} gtest gtest_main gmock_main)
    # Disable variadic macro warnings (can be a problem when compiling with Clang)
    target_compile_options(${testExecutable} PRIVATE -Wno-gnu-zero-variadic-macro-arguments)
    # Create test name as the capitalized form of the test executable
    string(TOUPPER ${testExecutable} testName)
    # Add executable to test suite
    add_test(${testName} ${testExecutable} ${GTEST_RUN_FLAGS})
endfunction(configure_test)

### Boost
add_library(boost INTERFACE)
target_include_directories(boost SYSTEM INTERFACE boost_1_86_0)
target_compile_definitions(boost INTERFACE BOOST_DATE_TIME_NO_LIB)
