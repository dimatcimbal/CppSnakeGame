cmake_minimum_required(VERSION 3.7)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# set output directories to the build
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR})

add_definitions(-std=c++17)

# platform-specific settings
if(WIN32)
    # setting up vcpkg toolchain
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake")
else()
    set(CXX_FLAGS "-Wall")
    set(CMAKE_CXX_FLAGS, "${CXX_FLAGS}")
endif()

project(CppSnakeGame)

# declare SDL2 dependency path
if(DEFINED VCPKG_INSTALLED_DIR)
    set(SDL2_DIR "${CMAKE_SOURCE_DIR}/vcpkg_installed/${VCPKG_TARGET_TRIPLET}/share/sdl2")
else()
    message(WARNING "VCPKG_INSTALLED_DIR not defined. SDL2_DIR might need to be set manually.")
endif()

# add SDL2 dependency
find_package(SDL2 REQUIRED)
include_directories(${SDL2_INCLUDE_DIRS} src)

# define the executable and sources
add_definitions(-DSDL_MAIN_HANDLED)

add_executable(SnakeGame 
    src/main.cpp 
    src/game.cpp 
    src/controller.cpp 
    src/renderer.cpp 
    src/snake.cpp
)

# link libraries
target_link_libraries(SnakeGame ${SDL2_LIBRARIES})
