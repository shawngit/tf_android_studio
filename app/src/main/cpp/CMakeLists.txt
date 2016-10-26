#
# Copyright (C)  The Android Open Source Project
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_VERBOSE_MAKEFILE on)

# TENSORFLOW_SRC_DIR:
#    root directory of tensorflow repo
#    used for shared source files
set(TENSORFLOW_SRC_DIR /Users/gfan/pproj/tensorflow.org)

# Include libraries needed for com_tensorflow_demo lib
include_directories(
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/gen/protobuf/include
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/gen/protobuf-host/include
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/gen/host_obj
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/downloads/eigen
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/downloads
    ${TENSORFLOW_SRC_DIR}
    ${TENSORFLOW_SRC_DIR}/protobuf/src)


set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -frtti -fno-exceptions -fpic -O3 -mfpu=neon -DTF_LEAN_BINARY")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,--allow-multiple-definition -Wl,--whole-archive")

add_library(com_tensorflow_demo SHARED
                imageutils_jni.cc
                yuv2rgb.cc
                rgb2yuv.cc)

target_link_libraries(com_tensorflow_demo
                         android
                         log
                         jnigraphics
                         m
                         atomic
                         z)
