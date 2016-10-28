cmake_minimum_required(VERSION 3.4.1)
include(ExternalProject)

# TENSORFLOW_SRC_DIR:
#    root directory of tensorflow repo
#    used for shared source files and pre-built libs
set(TENSORFLOW_SRC_DIR /Users/gfan/pproj/tensorflow.org)
set(distribution_DIR ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/gen)

add_library(lib_proto STATIC IMPORTED )
set_target_properties(lib_proto PROPERTIES IMPORTED_LOCATION
    ${distribution_DIR}/protobuf/lib/libprotobuf.a)

add_library(lib_tf STATIC IMPORTED )
set_target_properties(lib_tf PROPERTIES IMPORTED_LOCATION
        ${distribution_DIR}/lib/libtensorflow-core.a)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -frtti -fno-exceptions -fpic -O3 -mfpu=neon -DTF_LEAN_BINARY")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,--allow-multiple-definition -Wl,--whole-archive")

add_library(tensorflow_wrapper SHARED
            ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/android/jni/jni_utils.cc
            ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/android/jni/tensorflow_inference_jni.cc)

# Include libraries needed for hello-jni lib
target_link_libraries(tensorflow_wrapper
                          android
                          log
                          m
                          z
                          lib_tf
                          lib_proto)
include_directories(
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/gen/protobuf/include
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/gen/protobuf-host/include
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/gen/host_obj
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/downloads/eigen
    ${TENSORFLOW_SRC_DIR}/tensorflow/contrib/makefile/downloads
    ${TENSORFLOW_SRC_DIR}
    ${TENSORFLOW_SRC_DIR}/protobuf/src)
