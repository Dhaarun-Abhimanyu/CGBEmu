cmake_minimum_required(VERSION 3.10)
project(CGBEmu C)

# Set build type (Debug, Release, etc.)
set(CMAKE_BUILD_TYPE Debug)

# Include directories (if you have any headers in "include/")
include_directories(${PROJECT_SOURCE_DIR}/include)

# Define your source files (update as needed)
set(SOURCES
    src/main.c
)

# Create the executable
add_executable(CGBEmu ${SOURCES})

# Link libraries if needed (e.g. SDL2, etc.)
# target_link_libraries(CGBEmu ${SDL2_LIBRARIES})
