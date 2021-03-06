# Copyright (C) 2017 Microsoft.  All rights reserved.
# See LICENSE file in the project root for full license information.

cmake_minimum_required(VERSION 3.8.0 FATAL_ERROR)
project (OverrideLanguageSample)

# Define two variables in order not to repeat ourselves.
set(BINARY_NAME OverrideLanguageSample)

if(WIN32)
    set(DESCRIPTION "OverrideLanguageSample manifest")
    configure_file(${CMAKE_PROJECT_ROOT}/manifest.cmakein ${CMAKE_CURRENT_BINARY_DIR}/${BINARY_NAME}.exe.manifest CRLF)
    set(MANIFEST ${CMAKE_CURRENT_BINARY_DIR}/${BINARY_NAME}.exe.manifest)
endif()

add_executable(${BINARY_NAME}
    OverrideLanguageSample.cpp
    ${MANIFEST}
    )

target_include_directories(${BINARY_NAME} PRIVATE ${CMAKE_BINARY_DIR}/src/msix)

add_dependencies(${BINARY_NAME} msix)
if (LINUX OR AOSP)
    target_link_libraries(${BINARY_NAME} PRIVATE -latomic)
endif()
target_link_libraries(${BINARY_NAME} PRIVATE msix)
