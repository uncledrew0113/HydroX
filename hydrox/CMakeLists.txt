# Add all subdirectories (libraries)
add_subdirectory(core)
add_subdirectory(data)
add_subdirectory(model)
add_subdirectory(utils)
add_subdirectory(web)

# Main executable
set(PROJECT_SOURCES main.cc)

add_executable(${PROJECT_NAME} ${PROJECT_SOURCES})

target_include_directories(${PROJECT_NAME}
    PRIVATE
        ${PROJECT_SOURCE_DIR}/include   # 公共头文件目录
        ${CMAKE_CURRENT_SOURCE_DIR}     # 如果 main.cc 同级有头文件
)

target_link_libraries(${PROJECT_NAME}
    PRIVATE
        # Internal modules
        ${PROJECT_NAME}_core
        ${PROJECT_NAME}_data
        ${PROJECT_NAME}_model
        ${PROJECT_NAME}_utils
        ${PROJECT_NAME}_web

        # External dependencies
        spdlog::spdlog
        nlohmann_json::nlohmann_json
        Boost::filesystem
        pthread
        PahoMqttCpp::paho-mqttpp3-static
        SQLiteCpp
)