# Copyright (C) 2018-2021 Intel Corporation
# SPDX-License-Identifier: Apache-2.0
#

set(TARGET_NAME ie_tests)

file(GLOB TEST_INCLUDE ${CMAKE_CURRENT_SOURCE_DIR}/include/*.hpp)
file(GLOB TEST_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)

# create target

add_library(${TARGET_NAME} STATIC ${TEST_INCLUDE} ${TEST_SRC})

ie_faster_build(${TARGET_NAME}
    UNITY
)

list(APPEND EXPORT_DEPENDENCIES
        funcTestUtils
        ieTestHelpers
        )

target_include_directories(${TARGET_NAME}
    PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include"
    PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/src"
            $<TARGET_PROPERTY:inference_engine_plugin_api,INTERFACE_INCLUDE_DIRECTORIES>)

target_link_libraries(${TARGET_NAME} PUBLIC
        format_reader
        ${EXPORT_DEPENDENCIES}
        )

# developer package

openvino_developer_export_targets(COMPONENT inference_engine_tests
                                  TARGETS ${TARGET_NAME} ${EXPORT_DEPENDENCIES} ieTestHelpers_s)
