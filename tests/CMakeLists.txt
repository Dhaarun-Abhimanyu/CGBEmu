# Minimum CMake version requirement
cmake_minimum_required(VERSION 3.10)

# Project name
project(CGBEmu_Tests C)

# Set the root directory (one level up from `tests/`)
set(CGBEMU_ROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR}/..)
set(CGBEMU_BINARY_DIR ${CMAKE_CURRENT_BINARY_DIR}/..)

# Ensure we can find the main project's include directory
include_directories(${CGBEMU_ROOT_DIR}/include)

# Configure the config.h file properly
configure_file(${CGBEMU_ROOT_DIR}/cmake/config.h.in ${CGBEMU_BINARY_DIR}/config.h)

# Add main project directories
add_subdirectory(${CGBEMU_ROOT_DIR}/lib ${CGBEMU_BINARY_DIR}/lib)
add_subdirectory(${CGBEMU_ROOT_DIR}/src ${CGBEMU_BINARY_DIR}/src)

# Add the Catch2 testing framework (if used, otherwise remove)
if(EXISTS ${CGBEMU_ROOT_DIR}/third_party/Catch2)
    add_subdirectory(${CGBEMU_ROOT_DIR}/third_party/Catch2 ${CGBEMU_BINARY_DIR}/third_party/Catch2)
endif()

# Find the testing framework (modify if you use another one)
find_package(Catch2 REQUIRED)

# Collect all test source files
file(GLOB TEST_SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp")

# Add executable for running tests
add_executable(CGBEmuTest ${TEST_SOURCES})

# Link against the main emulator library
target_link_libraries(CGBEmuTest PRIVATE emu Catch2::Catch2)

# Enable CTest for running tests
enable_testing()

# Add test cases
foreach(test_file ${TEST_SOURCES})
    get_filename_component(test_name ${test_file} NAME_WE)
    add_test(NAME ${test_name} COMMAND CGBEmuTest)
endforeach()
