add_library(dronecode_sdk_mission ${PLUGIN_LIBRARY_TYPE}
    mission.cpp
    mission_impl.cpp
    mission_item.cpp
    mission_item_impl.cpp
)

include_directories(
    ${PROJECT_SOURCE_DIR}/core
    SYSTEM third_party/json11
)

# JSON parser library for parsing QGC plan for Mission
add_subdirectory(third_party/json11)

target_link_libraries(dronecode_sdk_mission
    dronecode_sdk
    json11
)

install(FILES
    mission.h
    mission_item.h
    DESTINATION ${dronecode_sdk_install_include_dir}
)

install(TARGETS dronecode_sdk_mission
    #EXPORT dronecode_sdk-targets
    DESTINATION ${dronecode_sdk_install_lib_dir}
)

if(ANDROID)
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/third_party/json11/libjson11.so
        DESTINATION ${lib_path}
    )
endif()

list(APPEND UNIT_TEST_SOURCES
    ${CMAKE_CURRENT_SOURCE_DIR}/mission_import_qgc_test.cpp
)
set(UNIT_TEST_SOURCES ${UNIT_TEST_SOURCES} PARENT_SCOPE)
