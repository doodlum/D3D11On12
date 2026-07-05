# Copyright (c) Microsoft Corporation.
# Licensed under the MIT License.
cmake_minimum_required(VERSION 3.14)
include(FetchContent)

file(GLOB SRC CONFIGURE_DEPENDS *.cpp)
file(GLOB INC ../include/*.h ../include/*.hpp ../interface/*.h *.h *.hpp)
file(GLOB INL ../include/*.inl *.inl)

file(GLOB_RECURSE EXTERNAL_INC ../external/*.h ../external/*.hpp)

add_library(d3d11on12 STATIC ${SRC} ${INC} ${INL} ${EXTERNAL_INC} d3d11on12.rc d3d11on12.def)

if (CMAKE_VERSION VERSION_GREATER 3.16)
	target_precompile_headers(d3d11on12 PRIVATE ../include/pch.hpp)
endif()

set(USE_PIX OFF CACHE BOOL "" FORCE)
# Vendored (external/D3D12TranslationLayer) instead of FetchContent: the host project
# controls dependency targets (DirectX-Headers guard lives in its CMakeLists).
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../external/D3D12TranslationLayer d3d12translationlayer)
set(d3d12translationlayer_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../external/D3D12TranslationLayer)

target_link_libraries(d3d11on12 d3d12translationlayer_wdk Microsoft::DirectX-Headers)
target_include_directories(d3d11on12
    PRIVATE ${d3d12translationlayer_SOURCE_DIR}/include
    PRIVATE ${d3d12translationlayer_SOURCE_DIR}/external)
target_include_directories(d3d11on12
    PRIVATE ../include
    PRIVATE ../external
    PUBLIC ../interface
    PRIVATE ./)
source_group(Inlines FILES ${INL})
source_group("Header Files\\External" FILES ${EXTERNAL_INC})
