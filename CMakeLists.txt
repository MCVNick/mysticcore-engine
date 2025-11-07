cmake_minimum_required(VERSION 3.21)
project(MysticCoreEngine VERSION 0.1.0 LANGUAGES CXX)

# C++20 standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Allow engine to be used as a subproject
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()

# Compiler flags
if(MSVC)
    add_compile_options(/W4 /WX)
    add_compile_options(/utf-8)
else()
    add_compile_options(-Wall -Wextra -Werror -pedantic)
endif()

# Debug-specific settings: AddressSanitizer and UndefinedBehaviorSanitizer
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    if(NOT MSVC)
        add_compile_options(-fsanitize=address -fsanitize=undefined -fno-omit-frame-pointer)
        add_link_options(-fsanitize=address -fsanitize=undefined)
    endif()
endif()

# vcpkg integration (only if not already set by parent project)
if(DEFINED ENV{VCPKG_ROOT} AND NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
        CACHE STRING "")
endif()

# Find packages
find_package(SDL2 CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)
find_package(EnTT CONFIG REQUIRED)
find_package(imgui CONFIG REQUIRED)
find_package(spdlog CONFIG REQUIRED)
find_package(Tracy CONFIG REQUIRED)
find_package(nlohmann_json CONFIG REQUIRED)
find_package(OpenGL REQUIRED)

# Engine library
add_library(MysticCoreEngine STATIC
    # Core
    src/core/App.cpp
    src/core/Time.cpp
    src/core/Logger.cpp
    src/core/Input.cpp
    
    # Files will be added as implemented
)

target_include_directories(MysticCoreEngine PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(MysticCoreEngine PUBLIC
    SDL2::SDL2
    SDL2::SDL2main
    glm::glm
    EnTT::EnTT
    imgui::imgui
    spdlog::spdlog
    Tracy::TracyClient
    nlohmann_json::nlohmann_json
    OpenGL::GL
)

target_compile_features(MysticCoreEngine PUBLIC cxx_std_20)

# Optional: Build examples if this is the main project
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    option(MYSTICCORE_BUILD_EXAMPLES "Build example applications" ON)
    if(MYSTICCORE_BUILD_EXAMPLES)
        add_subdirectory(examples)
    endif()
endif()
