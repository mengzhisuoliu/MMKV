#
# Tencent is pleased to support the open source community by making
# MMKV available.
#
# Copyright (C) 2020 THL A29 Limited, a Tencent company.
# All rights reserved.
#
# Licensed under the BSD 3-Clause License (the "License"); you may not use
# this file except in compliance with the License. You may obtain a copy of
# the License at
#
#       https://opensource.org/licenses/BSD-3-Clause
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.10.0)

project(mmkv_python)

IF(APPLE)
    add_compile_definitions(FORCE_POSIX)
ENDIF()

add_subdirectory(../Core Core)
add_subdirectory(pybind11)


pybind11_add_module( # Sets the name of the library.
             mmkv

             # Sets the library as a python module library.
             MODULE

             # Provides a relative path to your source file(s).
             libmmkv_python.cpp
        )

target_include_directories(mmkv PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(mmkv PROPERTIES
            CXX_STANDARD 17
            CXX_EXTENSIONS OFF
            )

target_link_libraries( # Specifies the target library.
                       mmkv PRIVATE
                       core
        )

IF (NOT WIN32)
    target_link_libraries(mmkv
            PRIVATE
            pthread)
ENDIF()
