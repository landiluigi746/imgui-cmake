cmake_minimum_required(VERSION 3.10)

project(imgui LANGUAGES CXX)

if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR})
    set(IMGUI_STANDALONE ON)
else()
    set(IMGUI_STANDALONE OFF)
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/lib)

add_library(imgui STATIC
    imgui.cpp
    imgui_draw.cpp
    imgui_widgets.cpp
    imgui_tables.cpp
    imgui_demo.cpp
)

set_target_properties(imgui PROPERTIES LINKER_LANGUAGE CXX)

target_include_directories(imgui PUBLIC .)
target_compile_features(imgui PUBLIC cxx_std_11)

if(MSVC)
    target_compile_options(imgui PUBLIC /W4)

    if(CMAKE_BUILD_TYPE STREQUAL "Release")
        target_compile_options(imgui PRIVATE /O2)
    elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
        target_compile_options(imgui PRIVATE /Zi)
    elseif(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
        target_compile_options(imgui PRIVATE /O2 /Zi)
    elseif(CMAKE_BUILD_TYPE STREQUAL "MinSizeRel")
        target_compile_options(imgui PRIVATE /Os)
    endif()
elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    target_compile_options(imgui PUBLIC -Wall -Wpedantic)

    if(CMAKE_BUILD_TYPE STREQUAL "Release")
        target_compile_options(imgui PRIVATE -O3)
    elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
        target_compile_options(imgui PRIVATE -g)
    elseif(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
        target_compile_options(imgui PRIVATE -O2 -g)
    elseif(CMAKE_BUILD_TYPE STREQUAL "MinSizeRel")
        target_compile_options(imgui PRIVATE -Os)
    endif()
endif()
