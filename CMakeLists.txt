# Copyright (c) Microsoft Corporation.
# Licensed under the MIT License.
cmake_minimum_required(VERSION 3.14)
project(d3d11on12)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)



# WDK headers without a WDK install: point WDK_INCLUDE_ROOT at a WDK Include/<ver> tree
# (e.g. extracted from the Microsoft.Windows.WDK.x64 NuGet). Global on purpose so the
# FetchContent'd D3D12TranslationLayer sees them too (its HAS_WDK probe + _wdk target).
if(DEFINED WDK_INCLUDE_ROOT AND NOT WDK_INCLUDE_ROOT STREQUAL "")
    include_directories("${WDK_INCLUDE_ROOT}/um" "${WDK_INCLUDE_ROOT}/shared" "${WDK_INCLUDE_ROOT}/km")
    set(CMAKE_REQUIRED_INCLUDES ${CMAKE_REQUIRED_INCLUDES} "${WDK_INCLUDE_ROOT}/um")
endif()
add_subdirectory(src)
