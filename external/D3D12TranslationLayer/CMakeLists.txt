# Copyright (c) Microsoft Corporation.
# Licensed under the MIT License.
cmake_minimum_required(VERSION 3.14)
project(d3d12translationlayer)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)
# Host projects (Community Shaders) may already provide DirectX-Headers (vcpkg);
# only fetch when the target is absent.
if(NOT TARGET DirectX-Headers AND NOT TARGET Microsoft::DirectX-Headers)
FetchContent_Declare(
    DirectX-Headers
    GIT_REPOSITORY https://github.com/Microsoft/DirectX-Headers.git
    GIT_TAG v1.619.1
)
FetchContent_MakeAvailable(DirectX-Headers)
endif()

option(USE_PIX "Enable the use of PIX markers" ON)

add_subdirectory(src)

if (HAS_WDK)
    add_subdirectory(DxbcParser)
    target_link_libraries(d3d12translationlayer dxbcparser)
endif()
